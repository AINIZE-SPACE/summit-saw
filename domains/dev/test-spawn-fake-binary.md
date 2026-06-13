# 技能: 进程 spawn 测试 — fake binary 强制 ENOENT

> 避免"依赖 binary 不存在来快速失败"的脆弱测试设计
> 适用: Node.js / Python / Go / 任何 spawn 子进程的测试
> 反例来源: slack4ccmcp STORY-8 review (`tests/reply-engine.test.ts`)

## Trigger

- 写一个会 spawn 外部 CLI（`claude`/`codex`/`gh`/`kubectl` 等）的测试
- 期望该测试在没有真实 binary 时快速失败
- 该测试在装了对应 CLI 的开发机 / CI 上会出现"挂住 runner"或"超时不准"

## 症状

- CI 上绿、本机红（或反过来）
- `npm test` / `pytest -x` / `go test` 在某个 case 卡死数分钟
- `node --test --test-timeout=5000` 不生效（per-test 超时未触发）
- spawn 的 child process 持续输出，但 parent promise 永远不 resolve

## 根因

测试想"快速失败"，但失败路径是**没找到 binary** 还是**binary 启动了跑不完**，两种情况的触发条件完全不同：

```ts
// 写法 A：什么都不设，依赖环境
test("claude spawn fails gracefully", async () => {
  const r = await spawnClaude("prompt", { timeoutMs: 500 });
  assert.equal(r.ok, false);  // ❌ 装了 claude 时会真的启动它
});
```

- 没装 `claude`：`spawn("claude")` → ENOENT → 立即失败 ✅
- 装了 `claude`：`spawn("claude")` → 真启动 → Claude 进程开始读 stdin / 调 API → 500ms 内 SIGKILL 也杀不掉（Windows: `SIGKILL` → `TerminateProcess`，对 `.exe` 包装器不一定即时生效）→ parent promise 永远 pending ❌

**`timeoutMs` 只在 spawn 成功后才生效。ENOENT 不走这个路径。**

## 修复模式

显式把要 spawn 的 binary 路径设成**显式不存在**的值，让 ENOENT 在任何环境下都成立：

```ts
test("claude spawn fails gracefully", async () => {
  // 关键：env var 在 import 模块前设置（确保 module load 时读到的就是 fake 值）
  const origBin = process.env.CLAUDE_BIN;
  process.env.CLAUDE_BIN = "nonexistent-claude-binary-for-test";

  try {
    const { generateReply } = await import("../src/reply-engine.js");
    const r = await generateReply("test", { timeoutMs: 500 });
    assert.equal(r.ok, false);
  } finally {
    if (origBin) process.env.CLAUDE_BIN = origBin;
    else delete process.env.CLAUDE_BIN;
  }
});
```

### 三条必须满足

1. **fake 字符串要包含子串让 grep 时一眼能识别**，例如 `nonexistent-claude-binary-for-test`，不要用 `xxx` / `fake`。
2. **import 必须在 `process.env.<BIN>` 设置之后**——很多 module 在 top-level 就读 env（`const BIN = process.env.CLAUDE_BIN || "claude"`）。在 `await import` 之前设好。
3. **`finally` 恢复原值**——同一进程跑多个 case 时 env 泄漏会让别的 case 行为变化。

## 验证

```bash
# 在装了真 claude 的本机跑
where claude    # 确认有
npm test        # 应该全部 <2s
```

如果还有 case 卡住，按"卡的是哪个 case"二分定位：
- 单独跑该 case → 卡 → 走 fake binary 模式
- 单独跑该 case → 仍卡 → 模块内部有别的问题（查 spawn 后 stdin 行为、信号处理、unhandled rejection）

## 决策树

```
写 spawn 失败路径的测试
  ├─ 用 fake binary (env var override)？
  │   ├─ 是 → 在 import 之前设，finally 恢复 → 提交
  │   └─ 否 → 装了 CLI 的本机会卡 → 用 fake 模式重写
  └─ 必须用真 CLI 跑？
      ├─ 是 → 单独 mark 为 integration test，跳过 unit runner
      │        或用 mock spawn (e.g. child_process.spawn patched)
      └─ 否 → 不写这种测试，或拆出独立可注入的接口
```

## 其它 spawn 库（Python / Go 等）

| 语言 | 模式 |
|------|------|
| Python `subprocess` | `monkeypatch.setenv("CLAUDE_BIN", "nonexistent-...")` in `monkeypatch` fixture 自动恢复 |
| Go `exec.Command` | `t.Setenv("CLAUDE_BIN", "nonexistent-...")`（Go 1.17+ 自动恢复）|
| Rust `std::process::Command` | `std::env::set_var` + 显式 restore，或在测试串行时用 `serial_test` crate |

## 关联坑位

- Windows 上 `SIGKILL` 等价 `TerminateProcess`，对 native executable 的 wrapper 进程不一定即时退出
- `child.kill()` 之后 child 可能已经 close stdout，但 parent 的 `.on("close")` 还有别的 listener 卡住
- 用 `--test-timeout=5000` 控制单 case 超时，但 case 内的 promise 永远不 resolve 还是会卡 runner
- ESM 动态 `import()` 是 async 的——fake env 必须在 `await import(...)` 之前

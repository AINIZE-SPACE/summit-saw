# summit-saw

`summit-saw` 是一个面向研发 agent 的自进化 skills 与程序化记忆库。

这个名字取“攀登工具”的感觉：agent 在复杂项目里向上攀爬，同时把可复用路径整理成更锋利、更可传播的资产。

它服务于这样的工作方式：Codex、Claude Code、Harness、OpenClaw 等 agent 在真实项目中工作，边执行边沉淀，把有效的工作流、判断规则、故障处理经验、项目约束与安装方式整理成可复用资产，然后在项目、个人全局、团队、组织层级逐步传播。

核心原则：

**通用能力沉淀到 `skills/base`；agent 特有能力沉淀到 `skills/agents/<agent>`；项目私有能力先留在项目级。**

## 项目目标

agent 在工作中天然会积累程序化记忆：仓库约定、调试路径、发布步骤、评审标准、环境坑位、失败恢复方式。如果这些记忆只留在单个会话、单个 agent runtime 或某个开发者本地，就无法组织化复利。

本仓库把这些经验整理为可管理、可安装、可演化的资产：

- **Base skills**: cross-agent workflows and principles that should work everywhere.
- **Agent adapters**: Codex / Claude Code / Harness / OpenClaw specific instructions, formats, commands, and install paths.
- **Scopes**: maturity stages that decide whether a skill is experimental, promoted, or organization-shared.
- **Reflection workflows**: skills that inspect sessions, extract patterns, evolve existing skills, and install updates at the right level.

## 目录结构

```text
.
+-- README.md
+-- docs/
|   +-- domain-branching.md
|   +-- install-targets.md
|   +-- lifecycle.md
|   +-- memory-propagation.md
|   +-- platform-integration.md
+-- domains/
|   +-- fitness/
+-- schemas/
|   +-- domain.manifest.schema.json
|   +-- program.manifest.schema.json
|   +-- skill.manifest.schema.json
+-- skills/
|   +-- agents/
|   |   +-- claude-code/
|   |   +-- codex/
|   |   +-- harness/
|   |   +-- openclaw/
|   +-- base/
|   |   +-- find-program/
|   |   +-- install-program/
|   |   +-- publish-program/
|   |   +-- reflection-skill-evolution/
|   |   +-- skill-library-governance/
|   +-- collaboration/
|   +-- workflow/
+-- templates/
    +-- agent-adapter-skill/
    +-- base-skill/
    +-- domain/
```

## Skill 模型

每个 skill 都应被当作一个小产品维护：

- 有明确使用对象；
- 声明 scope 和成熟度；
- 可安装到项目级或全局 agent runtime；
- 来自真实工作沉淀，而不是凭空分类；
- 能把可复用部分持续上提到 `skills/base`。

推荐目录：

```text
skill-name/
+-- SKILL.md
+-- manifest.json
+-- examples/
+-- references/
+-- scripts/
```

其中 `SKILL.md` 和 `manifest.json` 必需。`examples/`、`references/`、`scripts/` 只在能降低歧义或提升复用性时添加。

## Scope 等级

scope 用来描述一个 skill 可以被多大范围信任与复用。

| Scope | Meaning | Default install target |
| --- | --- | --- |
| `personal-experiment` | A single agent or developer is testing the workflow. | Personal or project-local |
| `project-incubating` | Useful in one repository or delivery stream, but not proven elsewhere. | Project-local |
| `team-promoted` | Reused by multiple people or agents in a team. | Team/project templates |
| `organization-shared` | Stable enough to publish as a standard organizational asset. | Global or org registry |

晋级必须基于证据：多次成功复用、减少人工步骤、失败处理更清楚、没有隐藏的 agent 私有假设。

## 自进化工作流

当 agent 完成一次有意义的工作后，可以执行反思工作流：

1. 读取相关 session、diff、命令结果、失败过程和用户反馈。
2. 先搜索已有 skills，再决定是否创建新 skill。
3. 判断本次经验属于哪类：
   - 通用经验：进入 `skills/base`；
   - agent 特有经验：进入 `skills/agents/<agent>`；
   - 项目私有经验：留在项目级；
   - 证据不足：暂缓沉淀。
4. 优先修改最匹配的已有 skill。
5. 没有合适 skill 时，基于模板创建新 skill。
6. 先安装到项目级，证据积累后再推广到个人全局、团队或组织级。
7. 在 manifest 中记录变更证据。

当前已内置第一个反思生成 skill：[`reflection-skill-evolution`](./skills/base/reflection-skill-evolution/SKILL.md)。

## Agent 适配策略

推荐结构：

- 可复用工作流写入 `skills/base/<skill-name>/SKILL.md`。
- runtime 加载方式、命令语法、memory 路径、安装路径写入 `skills/agents/<agent>/<skill-name>/SKILL.md`。
- adapter 中一旦出现可复用规律，就抽回 `base`。

适配对象：

- Codex：可能需要项目/全局 skill 安装路径、memory citation、final response directive。
- Claude Code：可能需要命令约定、hook、项目 memory 文件、skill 加载元数据。
- Harness：可能需要 CI/CD pipeline、approval gate、artifact 约束。
- OpenClaw：可能需要 plugin bridge、命令/runtime 约定、模型或工具兼容说明。

这些差异应该停留在 adapter 层，除非核心工作流本身真的不同。

## 安装与推广层级

默认路径：

1. **Project local**：新 skill 或不稳定 skill 先安装到项目级。
2. **Personal global**：个人/单 agent 反复验证后，可装到个人全局。
3. **Team promoted**：多个项目或多个成员复用后，进入团队模板。
4. **Organization shared**：跨团队验证并经过 review 后，成为组织共享资产。

推广不是复制文件，而是证明这个 skill 能在更大范围内稳定工作。

## 领域分支机制

每个领域默认采用 Git 分支管理。领域目录只是该分支内的资产布局，不是主要隔离机制。

这样做更干净：领域可以独立演化、实验、回滚、归档和推广；`main` 只接收全域稳定资产，不长期堆所有领域的私有记忆。

推荐分支层级：

| 层级 | 分支命名 | 用途 |
| --- | --- | --- |
| agent 个人级 | `agent/<agent>/<domain>/<topic>` | agent 私有或本地实验，不默认共享 |
| 领域通用级 | `domain/<domain>` | 一个领域内的通用程序化记忆 |
| 工程开发级 | `dev` | 仓库日常开发和集成分支，不代表全域推广完成 |
| 全域推广级 | `ascent` | 从领域向全域攀升的推广候选层 |
| 全域稳定级 | `main` | 已验证的全域共享资产 |

目录层级与分支层级配合使用：

- `domains/<domain>`：领域程序化记忆、领域说明、领域 promotion 记录。
- `skills/base`：跨领域通用能力。
- `skills/agents/<agent>`：agent runtime 适配。

已初始化领域：

- [`domains/fitness`](./domains/fitness/README.md)：健身领域，先承接 `E:\my_project\aifitness` 项目的 program memory。

## Program 操作技能

本仓库同时作为一个技能平台，需要给各 agent 平台提供统一操作入口：

- [`find-program`](./skills/base/find-program/SKILL.md)：跨分支、领域、skill、外部平台发现可用 program。
- [`install-program`](./skills/base/install-program/SKILL.md)：把 program 安装到项目、agent runtime、领域分支或全局层。
- [`publish-program`](./skills/base/publish-program/SKILL.md)：校验、提交、推送、发布 program 到本仓库或外部技能平台。

这里的 program 比 skill 更大：它可以包含 skill、领域程序化记忆、agent adapter、脚本、引用资料、安装元数据和发布规则。

后续 MCP 可以直接围绕这三个能力暴露工具：

- `find_program`
- `install_program`
- `publish_program`

## 记忆泛化路径

程序化记忆的泛化路径固定为：

```text
private/local -> domain/<domain> -> ascent -> main
```

- `private/local`：agent 私有、本机、本项目实验记忆。
- `domain/<domain>`：领域共享分支，每个领域一个分支。
- `ascent`：全域推广候选分支，用来承接准备跨领域复用的内容。
- `main`：稳定全域记忆，只保留经过验证的通用资产。

## 初始路线图

- Create base reflection and governance skills.
- Add minimal agent adapter templates.
- Define manifest schema and scope lifecycle.
- Use this repository in real development sessions.
- Promote only the skills that survive real use.

## 工作原则

这个仓库不是静态 cookbook，而是一个资产飞轮：

**work -> reflect -> extract -> install -> reuse -> improve -> promote**

目标不是让所有 skill 都通用，而是让“可通用的部分”足够清楚、可迁移、可持续进化。

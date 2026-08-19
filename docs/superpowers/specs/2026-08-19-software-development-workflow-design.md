# Software Development Workflow Plugin Design

## 背景

本插件为个人 Codex 环境提供一套跨项目、长期可复用的软件开发执行流程。用户只描述要完成的工程任务，Codex 负责理解项目、分类任务、制定并自检计划、实施修改、验证结果和披露剩余风险。

插件仅自动接管工程执行类请求：新增功能、修复问题、保持行为不变的重构、代码审查，以及明确要求修改项目的任务。普通代码解释、概念咨询和不要求执行的技术讨论不应触发本插件。

## 目标

- 以 `development-orchestrator` 作为统一入口。
- 提供 `feature-dev`、`bug-fix`、`refactor`、`code-review` 四个专业 skill。
- 不依赖 Superpowers 或其他第三方插件，单独安装即可运行。
- 遵循当前项目事实和适用的 `AGENTS.md`，不把特定技术栈或业务结构写死。
- 所有代码修改在实施前有计划，在交付前有可复核的验证证据。
- 以个人本地插件安装，同时保留独立 Git 源码仓库，便于升级、迁移和后续分发。

## 非目标

- 不为每个项目自动创建 `AGENTS.md`、ADR、CHANGELOG 或完整架构文档。
- 不强制项目采用某种分层、测试框架、构建工具或 Git 分支策略。
- 不接管普通问答，也不把一次项目中的业务事实固化为全局规则。
- 不承诺基于自然语言描述的隐式 skill 匹配达到传统程序路由器式的 100% 命中率。

## 方案选择

采用“统一入口 + 四个专业 skill”。不采用单一大 skill，因为其上下文成本和维护复杂度会持续增长；不采用五个平级入口，因为混合任务缺少可靠的阶段编排。

## 插件结构

```text
software-development-workflow/
├── .codex-plugin/
│   └── plugin.json
├── skills/
│   ├── development-orchestrator/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── feature-dev/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── bug-fix/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   ├── refactor/
│   │   ├── SKILL.md
│   │   └── agents/openai.yaml
│   └── code-review/
│       ├── SKILL.md
│       └── agents/openai.yaml
├── tests/
│   └── behavioral-scenarios/
└── docs/
    └── superpowers/specs/
```

仅在真实需要时增加 references 或 scripts；不创建空目录、重复说明或无用途的模板。

## 触发与路由

Codex 根据 skill 的 `description` 进行隐式匹配。`development-orchestrator` 的描述覆盖工程执行请求并排除普通咨询；四个专业 skill 使用更窄的任务症状和边界描述。

运行路径：

```text
工程执行请求
→ development-orchestrator
→ 渐进式理解项目
→ 分类任务并划定边界
→ 运行一个或多个专业流程
→ 汇总计划完成度、验证证据和风险
```

路由规则：

| 用户意图 | 专业 skill |
| --- | --- |
| 新增功能、接口、页面、能力或场景支持 | `feature-dev` |
| 修复错误、异常、失败测试或不符合预期的行为 | `bug-fix` |
| 保持外部行为不变的结构或可维护性调整 | `refactor` |
| 检查当前修改、diff 或 PR | `code-review` |

混合任务由 orchestrator 拆分阶段。默认先处理正确性问题，再判断结构调整是否为必要修改；会扩大风险的可选重构进入后续建议。每个专业 skill 也必须包含完整的计划和验证门槛，使其被直接匹配时仍能安全执行。

隐式匹配由 Codex 模型决定。插件通过清晰描述、窄边界和行为测试提升命中可靠性，但不会在文档或交付说明中声称绝对强制路由。

## 统一生命周期

`development-orchestrator` 执行以下生命周期：

1. 确认请求属于工程执行任务，并确定允许的修改范围。
2. 定位项目根目录和从根到目标文件最近适用的 `AGENTS.md`。
3. 按 Project → Module → Domain → Feature → Code 渐进式收集事实。
4. 分类任务；混合任务形成有顺序的阶段。
5. 制定包含任务理解、相关文件、现有模式、拟议改动、依赖影响、测试策略和风险的 Plan。
6. 自检 Plan，检查遗漏、不必要修改、兼容性、项目模式和文档需求。
7. 在外部 API、框架、依赖、配置或版本差异影响实现时核对官方文档。
8. 按专业 skill 实施，每个重要改动后执行尽可能小而相关的验证。
9. 建立 Plan Item → Changed Files → Verification 对照，未映射项目不得标记完成。
10. 运行项目实际存在且与改动相关的测试、lint、format、类型检查、构建或配置校验。
11. 检查 Git 状态与 diff，识别无关改动、调试代码、临时文件、兼容性和未验证修改。
12. 仅在验证证据支持时宣布完成；否则明确失败项、原因、关联性和剩余风险。

## 专业 Skill 职责

### feature-dev

明确需求边界和验收行为，复用现有项目模式，制定测试策略，再以最小必要范围实现。不得借新增功能进行无关架构重构或依赖升级。新增行为必须有自动化测试或可说明的替代验证。

### bug-fix

区分症状、根因和修复。先复现或建立等价失败证据，再形成根因假设并验证，随后实施最小修复和回归测试。不得通过削弱测试、吞掉异常或堆叠无意义的异常处理来掩盖问题。

### refactor

先定义必须保持的行为不变量和验证保护，再调整结构。除非用户明确扩大范围，不改变业务行为、不顺便修复独立 Bug、不增加新功能。交付时说明 before、after 和行为保持证据。

### code-review

默认只读。先检查 Git 状态、diff、修改意图、项目规则和相关版本文档，再按 correctness、architecture、API、database、exception handling、logging、concurrency、performance、security、testing、maintainability、compatibility 和 unrelated changes 检查。发现按 P0-P3 排序并引用具体文件位置；没有问题时明确说明未发现阻塞问题、测试空白和残余风险。

## 项目理解与 AGENTS.md

事实来源优先级为：当前代码和配置、适用的 `AGENTS.md`、测试、README 和项目文档、Git 历史、官方文档、通用知识。

skill 自动识别项目根目录，检查与任务相关的构建文件、测试配置、CI、迁移、lint 和 formatter。它不扫描整个仓库，也不假设 Java、Spring Boot、React 或任何固定技术栈。

`AGENTS.md` 规则从项目根向目标文件所在目录继承，越近的适用规则优先。没有 `AGENTS.md` 时继续工作；只有发现长期稳定且值得固化的规则时，才在最终建议中提出创建它。

## 官方文档与版本

当实现依赖框架、库、SDK、CLI、配置、数据库、云服务或第三方 API 的具体行为时：

1. 从项目文件确定实际版本。
2. 优先读取该版本的官方文档或官方发布说明。
3. 检查弃用、破坏性变化和版本差异。
4. 不因文档展示了新版本而无理由升级项目。
5. 文档不可访问时明确披露未完成文档验证，不伪造依据。

项目实际版本优先于面向其他版本的文档示例；现有代码可作为项目模式证据，但不能覆盖已确认的版本约束。

## 失败处理与工作区保护

- 仅在业务要求确实无法从项目事实推断且不同答案会实质改变结果时询问用户。
- 不运行项目中不存在的校验命令，不把未执行的检查报告为通过。
- 测试、lint、build 或类型检查失败时记录命令、核心原因、与本次修改的关联性和交付影响。
- 保留用户已有未提交修改，不清理、覆盖或回滚无关文件。
- 不使用破坏性 Git 操作处理脏工作区。
- 计划项缺少实现或验证映射时，任务保持未完成。
- Review 请求默认不写代码；只有用户随后明确要求修复时才进入相应执行流程。

## 插件验证策略

技能创建遵循面向行为的 RED → GREEN → REFACTOR 验证：先对代表性请求记录无新 skill 时的基线行为，再创建最小指令，随后在加载 skill 的条件下复测并关闭观察到的漏洞。

验证范围：

- `plugin.json`、YAML frontmatter 和 `agents/openai.yaml` 的结构检查。
- 新增功能、Bug 修复、重构和 Review 的单一意图触发场景。
- “修 Bug 并顺便重构”等混合意图的边界与顺序场景。
- 普通代码解释和技术咨询不应触发 orchestrator 的负向场景。
- 专业 skill 被直接匹配时仍执行 Plan 和 Verification 的兜底场景。
- 文档不可访问、测试失败、缺少构建命令和脏工作区等失败披露场景。
- 每个 Plan Item 都映射到改动文件与验证证据的完整性场景。
- 从个人本地 marketplace 安装后，在新会话中用代表性自然语言请求进行匹配验证。

行为验证发现的问题必须回写到对应 skill，并重新执行相关场景。测试材料保留在源码仓库中，便于后续版本回归。

## 本地安装与版本管理

当前源码工作副本位于 `/Users/liuwenwen/Documents/Codex/2026-08-19/superpowers-plugin-superpowers-openai-api-curated/outputs/software-development-workflow`，并作为独立 Git 仓库管理；后续可克隆或移动到任意个人开发目录。插件 manifest 使用稳定的 kebab-case 名称 `software-development-workflow`。通过个人本地 marketplace 安装，使其对后续项目可用；源码仓库保持为唯一编辑来源，不直接修改安装缓存。

升级流程为：修改源码 → 执行静态与行为验证 → 更新插件版本 → 刷新个人 marketplace 安装 → 在新会话中进行冒烟测试 → 提交 Git 变更。

## 验收标准

- 插件包含有效 manifest 和 5 个可发现的 skill。
- 自然语言工程执行请求能够匹配统一入口或安全的专业兜底流程。
- 普通解释和咨询不被设计为自动触发目标。
- 单一和混合任务均产生清晰边界、Plan、Plan Validation 和逐项验证映射。
- 官方文档检查以项目版本为准，失败时如实披露。
- Review 默认只读并以严重程度优先输出问题。
- 本地安装不依赖 Superpowers，源码可由 Git 独立管理。
- 静态验证、行为场景和安装后冒烟验证均有可复核结果。

## 已知限制

- Codex 的隐式 skill 选择是模型行为，描述优化和测试不能提供绝对命中保证。
- 不同项目可用的测试、构建和文档访问能力不同，插件只能选择实际存在的验证手段。
- 通用 skill 不保存项目业务知识；稳定项目规则仍应由代码库文档和 `AGENTS.md` 承载。
- 本插件不替代人工业务决策；真正无法推断且会改变交付结果的需求仍需用户确认。

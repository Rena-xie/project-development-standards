# 项目开发规范包

版本：`0.2.0`

## 1. 定位

本规范包用于跨项目复用阶段门禁，判断某个开发阶段是否已经具备进入下一阶段的
条件。它不定义具体产品需求、项目架构、任务进度或技术栈命令。

规范包采用可直接使用的目录结构：新项目将 `AGENTS.md`、`standards/` 和 `templates/`
放到项目根目录后即可开始工作，不需要替换路径占位符或建立第二个 Agent 配置入口。

可复用规范只回答四个问题：

1. 当前阶段必须产出什么。
2. 产物必须满足哪些可验证标准。
3. 什么情况下必须返工，什么情况下可以记录例外。
4. 如何证明门禁已经执行且结论对应当前产物。

项目自身的 `PROJECT.md`、版本规格、设计文档、任务清单和验证证据仍然属于项目
仓库，本规范包不替代它们。

## 2. 设计原则

- **按需加载**：Agent 只读取当前阶段及风险触发所需的规范，不一次性加载全部内容。
- **单一入口**：新项目默认只保留项目根目录一份 `AGENTS.md`，配置直接内嵌，不要求
  第二个常驻 Agent 指令文件。
- **门禁优先**：规范不是建议清单；未通过阻断项时，不得进入下一阶段。
- **证据驱动**：门禁结论必须引用具体产物、版本、命令或运行证据。
- **最小充分**：规范判断设计是否足以交付，不鼓励与规模不匹配的复杂架构。
- **技术中立**：规范提出必须回答的问题，不强制某一种架构、目录或测试框架。
- **可重新打开**：上游范围或设计变化会使下游门禁失效，必须重新评估受影响部分。
- **不追求分数**：优先使用 `PASS / REWORK / WAIVER`，避免总分掩盖严重问题。

## 3. 阶段路由

| 阶段 | 必读规范 | 主要产物 | 通过后 |
| --- | --- | --- | --- |
| 需求确认 | [`standards/01-requirements.md`](standards/01-requirements.md) | 需求与验收清单 | 进入架构设计 |
| 架构设计 | [`standards/02-architecture.md`](standards/02-architecture.md) | 架构说明、决策与风险 | 进入任务拆分 |
| 任务拆分 | [`standards/03-task-decomposition.md`](standards/03-task-decomposition.md) | 任务依赖图与追踪矩阵 | 进入实现 |
| 实现与验证 | [`standards/04-implementation-testing.md`](standards/04-implementation-testing.md) | 代码、测试与变更证据 | 进入发布准备 |
| 发布与运维 | [`standards/05-release-operations.md`](standards/05-release-operations.md) | 发布、回滚与运行验证记录 | 完成交付 |

安全相关变更还必须加载
[`standards/security.md`](standards/security.md)。安全门禁是横切检查，嵌入架构、
实现或发布阶段，不单独增加开发阶段。

## 4. 门禁结论

每道门禁只能使用以下结论：

- `PASS`：所有阻断项通过，未解决问题不会阻止下一阶段。
- `REWORK`：至少一个阻断项失败；修正产物后重新评审。
- `WAIVER`：存在经明确批准、范围受限且有恢复措施的例外。没有批准依据时不得使用。

规则：

- 阻断项失败默认进入 `REWORK`。
- 非阻断观察项可以记录为后续改进，不影响 `PASS`。
- 安全、数据丢失、公共契约破坏等高危阻断项不得由 Agent 自行豁免。
- `N/A` 必须写明原因；未检查不能等同于 `N/A` 或 `PASS`。
- 门禁只对具体产物版本有效。产物发生实质变化后，原结论失效。

每次评审使用
[`templates/gate-review.md`](templates/gate-review.md) 记录结论和证据。

## 5. 评审独立性

- 低风险阶段可以先由产物作者完成自评，再由评审记录明确标注评审方式。
- 架构、安全、数据完整性和发布门禁应由未直接负责该产物的人员或独立评审上下文
  复核；单人环境可以由用户复核高风险结论。
- 无法进行独立复核时，可以继续做技术自评，但不得把自评限制写成“独立评审通过”。

## 6. 在新项目中使用

1. 新建项目根目录。
2. 将本包根目录的 [`AGENTS.md`](AGENTS.md)、`standards/` 和 `templates/` 原样放入
   新项目根目录，不重命名、不嵌套到额外子目录。
3. 不需要替换技术栈、命令、文档路径或规范包路径占位符。工具链从项目清单、脚本和
   CI 中发现；新项目首次产生产物时使用默认 `docs/` 路径。
4. 在阶段开始时读取对应门禁规范。
5. 使用对应产物模板完成设计和计划。
6. 使用统一评审模板执行门禁，并把证据保存到 `docs/gates/`。
7. 只有 `PASS` 或经批准的 `WAIVER` 可以进入下一阶段。

规范仓库中的 `README.md`、`VALIDATION.md`、`CHANGELOG.md` 和 `VERSION` 是规范包维护
资料，不需要复制到新项目。新项目只需要上述三个直接运行项。

推荐的项目内最小引用方式：

```text
AGENTS.md
  -> 规范包固定版本
  -> 当前阶段规范
  -> 当前阶段产物
  -> 门禁评审记录
```

规范包升级时先在试点项目验证，再更新引用版本。项目不得静默继承未经验证的规范
变化。

## 7. 目录

```text
project-development-standards/
├── AGENTS.md
├── CHANGELOG.md
├── README.md
├── VERSION
├── standards/
│   ├── 01-requirements.md
│   ├── 02-architecture.md
│   ├── 03-task-decomposition.md
│   ├── 04-implementation-testing.md
│   ├── 05-release-operations.md
│   └── security.md
├── templates/
│   ├── architecture.md
│   ├── gate-review.md
│   ├── project-profile.md  # 可选扩展
│   ├── requirements.md
│   └── task-plan.md
└── VALIDATION.md
```

## 8. 当前边界

`0.2.0` 是直接使用版，暂不包含：

- 产品规格和任务进度管理。
- 特定语言、框架或仓库结构。
- CI 平台实现。
- Codex skill 封装。
- 组织级多人审批和合规流程。

规范包仓库由 Git 管理；新项目直接复制可运行项即可，不需要继承规范仓库的 Git
历史。是否封装为 skill 或增加自动检查脚本，在首个新项目实际使用后决定。

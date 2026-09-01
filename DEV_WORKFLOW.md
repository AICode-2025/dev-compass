# 统一开发流程 DEV-WORKFLOW v1.1（grilling → ui-skills → diagnosing-bugs 组合管线）

> 本文件是工程罗盘 Dev Compass 的「编排说明」：与 `dev-compass/SKILL.md` 同源，职责边界为——**SKILL.md = 触发编排的入口定义；本文件 = 各阶段落地细节与配置说明**。SKILL.md 负责入口判定与路由，本文件负责每道 Gate 的操作细则。二者不重复罗列入口判定，只各司其职。
> 组合方式：**按生命周期阶段串行编排，各自独立触发，互不耦合**（非合并、非重写）。
> 适用：任何新功能 / UI 改动 / 疑难排错类型的工程。

## 配置模型（每次会话先定，不展开细粒度表）

- **mode**：`light`（默认，省 token）/ `full`（显式开启，完整流水线）。
- **platform**：核心渲染平台，仅 `web` 或 `miniprogram`，与业务形态解耦。
- **is_admin_project**：顶层布尔预留位（默认 false），标记 B 端后台类项目；**不实现 admin_profile 细粒度开关**，待首个真实后台项目再迭代。

## 入口判定
- `mode:light`：按需加载、轻量推进，不强制完整拷问、不强制落地文件，不自动调重型设计技能。
- `mode:full`：完整 grilling 拷问、按要求产出 DESIGN / MASTER 产物并归档。
按生命周期定位任务阶段：
- **A · 新功能 / 需求有歧义** → 进 Gate1
- **B · UI 视觉 / 打磨** → 进 Phase-A → Gate2
- **C · 出 bug / 性能卡顿** → 直接进 Phase-D

## Gate1 · 需求拷问（grilling / grill-me）
- 触发技能：`grilling`（别名入口 `grill-me` 自动转发）。
- 时机：有歧义 / 高风险 / 新品类才展开完整设计树；清晰需求跳过（照常做，不束缚）。
- 做法：待决问题画成决策树 → 逐轮抛「前沿」问题（每条带推荐答案）→ 前沿为空收敛。
- 产出受 mode 控制：仅 `full` 在收敛后写入一次归档到 `project_memory.md`（含 platform 与 is_admin_project，便于复现上下文）；`light` 不强制写文件。
- 查事实（读文件/跑命令）是执行者职责，不抛给用户。

## Gate1.5 · 软依赖降级（ui-ux-pro-max，软依赖）
`ui-ux-pro-max` 非核心。规则：
1. **缺失即降级**：检测不到时自动回退本地 `create-design-md + baseline-ui`，流程不中断。
2. **light 禁用**：light 禁止自动调用，不注入其上下文；仅用户显式指令才允许。
3. **full+admin 才准入**：仅 `mode:full AND is_admin_project=true` 才允许优先调度生成 `MASTER.md`。
4. **运行时双护**：脚本缺失→回退本地；环境（Python 3）缺失→禁止进高 token 纯 prompt 回退，直接回退本地组合并提示。
5. **职责边界**：只产 `MASTER.md`，完成后交还本地链路 `impeccable → improve-ui + fixing-* → web-design-guidelines`。
6. **子集注入**：不整体注入其知识库，仅注入检索匹配子集。
7. **软删完整**：删除它后整套流程 100% 可用。
8. **MASTER.md 复用**：已生成则读盘复用，不重跑检索。

## Phase-A · 设计基建（design-first）
- 触发技能：`create-design-md`（或手工写 DESIGN 规范）+ `baseline-ui` 作约束。
- 有 UI 工程开工前先建立/校核 `DESIGN.md`（token、间距/圆角/阴影刻度、动效红线）。
- admin + full：按 Gate1.5 规则考虑 `MASTER.md`；否则本地组合。
- 网络注意：CLI 类走 npm 若不通用 `--registry=https://registry.npmmirror.com`。

## Phase-B · 构建
- 通用开发，技能不参与，仅遵循 DESIGN 规范 + baseline-ui 约束防 "AI 味" 代码混入。

## Gate2 · UI 验收（设计路由 · 按任务坡度选一件主技能）
先看平台/任务坡度，只选一件主技能，不堆叠；第二件仅场景允许时叠加（保持最小技能集）。
- **小程序 / 原生 / 跨栈精修** → `impeccable`（含 native 分支），打磨用 critique/audit/polish/animate/typeset，只出计划用 audit/critique。
- **Web 落地页 / 作品集** → `design-taste-frontend`(TASTE)，防模板化；可叠加 impeccable。
- **需 DESIGN 契约文档** → 默认 `create-design-md`；已在跑 impeccable 则用其 `document`.
- **备选**（能力被 impeccable 覆盖，仅需时才调）：`baseline-ui` / `improve-ui` / `fixing-accessibility` / `fixing-metadata` / `fixing-motion-performance`。
- `ui-skills-root`：仅方向未定、需初判时作路由器。

## Phase-D · 疑难排错（diagnosing-bugs）
- 触发技能：`diagnosing-bugs`。
- 六阶段：复现（先造能变红的反馈环）→ 最小化 → 3-5 个可证伪假设 → 插桩验证 → 修复+回归 → 清理。
- 平时零占用，出问题才启用。

## 纪律（已敲定）
1. mode 分级：light 默认省 token；full 显式完整流水线；light 不强制写产物文件。
2. platform 与业务形态解耦；is_admin_project 仅布尔预留位，不提前实现 admin_profile。
3. 分级启用：有歧义才完整 grilling，清晰照做（不束缚）。
4. UI 技能按平台路由选一件主技能（impeccable / design-taste-frontend），最小集、不堆叠；baseline-ui / improve-ui / fixing-* 降级为需时才调。
5. diagnose 按需启用，零常驻 token。
6. 只调度本机真实技能，不调用虚构技能名作为调度名。
7. 触发逻辑以 `dev-compass/SKILL.md` 为准；本文件侧重阶段落地细则（C3 文档去重：总说明放 SKILL.md，本文件不重复入口判定罗列）。

## 落地清单
- [ ] 启动：定 mode（默认 light）→ 记 platform → 记 is_admin_project
- [ ] 新功能：判需求清晰度 → 不清跑 grilling → full 才归档进 project_memory
- [ ] 有 UI：Design 规范就绪；小程序/跨栈走 impeccable，Web 落地页走 TASTE
- [ ] admin+full：按降级规则考虑 MASTER.md，否则 create-design-md + baseline-ui
- [ ] UI 完成：impeccable critique/audit 复核（或备选 improve-ui→fixing-*）
- [ ] 出 bug：diagnosing-bugs 六阶段闭环
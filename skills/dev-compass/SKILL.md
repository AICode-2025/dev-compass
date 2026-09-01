---
name: "工程罗盘"
description: "工程罗盘：统一开发流程编排，串起需求拷问(grilling)、设计基建与UI验收(impeccable/design-taste-frontend/create-design-md，备选 baseline-ui/improve-ui/fixing-*)、疑难排错(diagnosing-bugs)成一条分阶段门禁式流水线。当你需要按标准流程推进新功能、UI改动或排查疑难bug时使用；或用户提到『工程罗盘』『dev-compass』『按流程来』『开发流程』。"
---

# 工程罗盘（Dev Compass）

融合三套独立技能家族，按**生命周期阶段**串行触发、各司其职、互不耦合（非合并、非重写）。跨项目可复用。

## 入口判定
先判定任务类型：
- **A · 新功能 / 需求有歧义** → 进「1. 需求拷问」
- **B · UI 视觉 / 打磨** → 进「2. 设计基建」→「4. UI 验收」
- **C · 出 bug / 性能卡顿** → 直接进「5. 疑难排错」

## 1. 需求拷问（grilling / grill-me）
- 触发：`grilling`（别名入口 `grill-me` 自动转发）。
- 何时完整展开设计树：**有歧义 / 高风险 / 新品类**；清晰需求跳过，照常做，不束缚。
- 做法：待决问题画成决策树 → 逐轮抛「前沿」问题（每条带推荐答案）→ 前沿为空收敛。
- 产出：结论沉淀到项目记忆（如 `project_memory.md`）供复用，避免重复拷问、省 token。
- 查事实（读文件/跑命令）是执行者职责，不抛给用户。

## 2. 设计基建（design-first）
- 触发：`create-design-md` 或手工写 DESIGN 规范 + `baseline-ui` 作约束。
- 有 UI 的工程开工前，先建立/校核 `DESIGN.md`：token、间距/圆角/阴影刻度、动效红线、反 "AI 味" 基线。
- 网络注意：CLI 类（`create-design-md`/`ui-skills`）若 npm 源不通，改用国内镜像（如 npmmirror）。

## 3. 构建
- 通用开发，技能不参与，仅遵循 DESIGN 规范与基线约束。

## 4. UI 验收（设计路由 · 按任务坡度选一件主技能）
先看**平台/任务坡度**，只选一件主技能，不堆叠；第二件仅场景允许时叠加（保持最小技能集）。
- **小程序 / 原生 / 跨栈 UI 精修（主力）** → `impeccable`（含 native 分支 `audit.native.md`/`adapt.native.md`）。打磨用其 `critique`/`audit`/`polish`/`animate`/`typeset` 等子命令；仅出计划、不直接改源码时用 audit/critique。
- **Web 落地页 / 作品集（专项）** → `design-taste-frontend`（TASTE），防模板化；可叠加 `impeccable`。
- **需要 DESIGN 契约文档** → 默认沿用 `create-design-md`（避免引入命令）；已在跑 `impeccable` 则用其 `document` 子命令。
- **备选（能力已被 impeccable 覆盖，仅需时才调）**：`baseline-ui`（快速清渣）、`improve-ui`（只审计出计划不动源码）、`fixing-accessibility`/`fixing-metadata`/`fixing-motion-performance`（定向修复）。
- `ui-skills-root`：仅当方向未定、需初判时作路由器。
- 注：`impeccable` 依赖 `node scripts/*` 运行时，按"多栈按需调用"破例允许；npm 源不通用 `--registry=https://registry.npmmirror.com`。

## 5. 疑难排错（diagnosing-bugs）
- 触发：`diagnosing-bugs`。
- 六阶段纪律：**复现(先造能变红的反馈环) → 最小化 → 3-5 个可证伪假设 → 插桩验证 → 修复+回归 → 清理**。
- 平时零占用，出问题才启用。

## 纪律
1. 分级启用：有歧义才完整 grilling，清晰照做（不束缚）。
2. UI 技能按平台路由选一件主技能（impeccable / design-taste-frontend），最小集、不堆叠；原 baseline-ui / improve-ui / fixing-* 降级为需时才调。
3. diagnose 按需启用，零常驻 token。

## 落地清单
- [ ] 新功能：先判需求是否清晰 → 不清跑 grilling → 结论进记忆
- [ ] 有 UI：Design 规范就绪；小程序/跨栈精修走 impeccable，Web 落地页走 TASTE
- [ ] UI 完成：impeccable critique/audit 复核（或备选 improve-ui→fixing-*）
- [ ] 出 bug：diagnosing-bugs 六阶段闭环
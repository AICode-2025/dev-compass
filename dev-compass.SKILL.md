---
name: "工程罗盘"
description: "工程罗盘 v1.1：统一开发流程编排（mode:light/full 分级 + platform:web/miniprogram + is_admin_project 预留位），串起需求拷问(grilling)、设计基建与UI验收(impeccable/design-taste-frontend/create-design-md，备选 baseline-ui/improve-ui/fixing-*)、疑难排错(diagnosing-bugs)成一条分阶段门禁式流水线。token 开销防护：light 默认省 token、full 完整流水线；软依赖 ui-ux-pro-max 缺失时自动降级本地技能集不中断。当你需要按标准流程推进新功能、UI改动或排查疑难bug时使用；或用户提到『工程罗盘』『dev-compass』『按流程来』『开发流程』。"
---

# 工程罗盘（Dev Compass）v1.1

融合三套独立技能家族，按**生命周期阶段**串行触发、各司其职、互不耦合（非合并、非重写）。跨项目可复用。本版本为核心编排层，本地 UI 打磨由真实存在技能集承担。

## 0. 模式模型（mode 分级 · 默认 light）

每次会话先确定运行模式，决定流程深度与产出强度：

- **`mode:light`（默认）**：省 token 模式。按需加载、不强制完整拷问、不强制落地文件；迭代/小页面/改 bug 走此档。**本档禁止自动调用任何重型设计技能。**
- **`mode:full`（显式开启）**：完整流水线。才触发完整 grilling 拷问、写 DESIGN/MASTER 产物、按要求归档；适合新品类/高风险/完整交付。
- 判定：启动时若用户未指定，默认 `light`；升级到 `full` 需用户显式要求。
- `light` 模式下，用户手动指定单个技能时仍允许调用，不锁死能力。

## 1. 入口判定（platform/is_admin_project）

进入各阶段前先记录两条静态配置（只读，不展开细粒度表）：

- `platform`：核心渲染平台，仅 `web` 或 `miniprogram`，控制底层渲染（DOM / rpx 等原生约束）。**与业务形态解耦。**
- `is_admin_project`：顶层布尔预留位，标记是否 B 端后台类项目。`false` 为默认。**当前不做 `admin_profile` 细粒度开关表**，等首个真实后台项目出现再迭代该部分。

再判定任务类型：
- **A · 新功能 / 需求有歧义** → 进「2. 需求拷问」
- **B · UI 视觉 / 打磨** → 进「3. 设计基建」→「5. UI 验收」
- **C · 出 bug / 性能卡顿** → 直接进「6. 疑难排错」

## 2. 需求拷问（grilling / grill-me）
- 触发：`grilling`（别名入口 `grill-me` 自动转发）。
- 何时完整展开设计树：**有歧义 / 高风险 / 新品类**；清晰需求跳过，照常做，不束缚。
- 做法：待决问题画成决策树 → 逐轮抛「前沿」问题（每条带推荐答案）→ 前沿为空收敛。
- 产出受 mode 控制：**仅 `full` 模式**在收敛后写入一次结构化归档到项目记忆（`project_memory.md`），字段含 platform / is_admin_project，便于后续会话复现上下文；**`light` 不强制写文件**。
- 查事实（读文件/跑命令）是执行者职责，不抛给用户。

## 3. 设计基建（design-first）
- 触发：`create-design-md` 或手工写 DESIGN 规范 + `baseline-ui` 作约束。
- 有 UI 的工程开工前，先建立/校核 `DESIGN.md`：token、间距/圆角/阴影刻度、动效红线、反 "AI 味" 基线。
- `full` 模式且 `is_admin_project=true` 时，若安装了软依赖 `ui-ux-pro-max` 才考虑用其生成 `MASTER.md` 设计系统；否则一律走本地 `create-design-md + baseline-ui`。
- 网络注意：CLI 类（`create-design-md`/`ui-skills`）若 npm 源不通，改用国内镜像（如 npmmirror）。

## 4. 构建
- 通用开发，技能不参与，仅遵循 DESIGN 规范与基线约束。

## 5. UI 验收（设计路由 · 按任务坡度选一件主技能）
先看**平台/任务坡度**，只选一件主技能，不堆叠；第二件仅场景允许时叠加（保持最小技能集）。
- **小程序 / 原生 / 跨栈 UI 精修（主力）** → `impeccable`（含 native 分支 `audit.native.md`/`adapt.native.md`）。打磨用其 `critique`/`audit`/`polish`/`animate`/`typeset` 等子命令；仅出计划、不直接改源码时用 audit/critique。
- **Web 落地页 / 作品集（专项）** → `design-taste-frontend`（TASTE），防模板化；可叠加 `impeccable`。
- **需要 DESIGN 契约文档** → 默认沿用 `create-design-md`（避免引入命令）；已在跑 `impeccable` 则用其 `document` 子命令。
- **备选（能力已被 impeccable 覆盖，仅需时才调）**：`baseline-ui`（快速清渣）、`improve-ui`（只审计出计划不动源码）、`fixing-accessibility`/`fixing-metadata`/`fixing-motion-performance`（定向修复）。
- `ui-skills-root`：仅当方向未定、需初判时作路由器。
- 注：`impeccable` 依赖 `node scripts/*` 运行时，按"多栈按需调用"破例允许；npm 源不通用 `--registry=https://registry.npmmirror.com`。

## 6. 疑难排错（diagnosing-bugs）
- 触发：`diagnosing-bugs`。
- 六阶段纪律：**复现(先造能变红的反馈环) → 最小化 → 3-5 个可证伪假设 → 插桩验证 → 修复+回归 → 清理**。
- 平时零占用，出问题才启用。

## 7. 软依赖降级骨架（ui-ux-pro-max）

`ui-ux-pro-max` 是本工作流的**软依赖**，非核心。遵守：

1. **缺失即降级**：检测不到 `ui-ux-pro-max` 技能/脚本时，自动回退本地组合 `create-design-md + baseline-ui`，流程不中断、不报错。
2. **light 禁用**：`light` 模式禁止自动调用它，不注入其上下文（增量 token ≈ 0）；仅用户显式手动指令才允许。
3. **full+admin 才准入**：仅当 `mode:full AND is_admin_project=true` 才允许优先调度它生成 `MASTER.md`。
4. **运行时降级双护**：(a) 脚本不存在→回退本地；(b) 脚本存在但执行环境（如 Python 3）缺失→**禁止进入其高 token 纯 prompt 回退**，直接回退本地组合并提示。
5. **职责边界**：只产出 `MASTER.md`，完成后立即交还本地链路 `impeccable → improve-ui + fixing-* → web-design-guidelines`，不重复加载其知识库。
6. **子集注入**：不把其全部知识库整体注入上下文，仅注入检索匹配的子集。
7. **软删完整性**：删除它后整套工作流 100% 可用。
8. **MASTER.md 复用**：若已生成，优先读取磁盘复用，不重跑全套检索。

## 纪律
1. mode 分级：light 默认省 token，full 显式完整流水线；light 不强制写产物文件。
2. platform 与业务形态解耦；is_admin_project 仅布尔预留位，不提前实现 admin_profile。
3. 分级启用：有歧义才完整 grilling，清晰照做（不束缚）。
4. UI 技能按平台路由选一件主技能（impeccable / design-taste-frontend），最小集、不堆叠；原 baseline-ui / improve-ui / fixing-* 降级为需时才调。
5. diagnose 按需启用，零常驻 token。
6. 只调度本机真实技能；不调用虚构技能名（ui-ux-pro-max / Make Interfaces Feel Better 不直接作为调度名）。

## 落地清单
- [ ] 启动：确定 mode（默认 light）；记录 platform 与 is_admin_project
- [ ] 新功能：先判需求是否清晰 → 不清跑 grilling → full 才归档结论进记忆
- [ ] 有 UI：Design 规范就绪；小程序/跨栈精修走 impeccable，Web 落地页走 TASTE
- [ ] is_admin_project=true 且 full：按软依赖降级规则考虑 MASTER.md，否则 create-design-md + baseline-ui
- [ ] UI 完成：impeccable critique/audit 复核（或备选 improve-ui→fixing-*）
- [ ] 出 bug：diagnosing-bugs 六阶段闭环
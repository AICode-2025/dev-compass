# 工程罗盘 Dev Compass

统一开发流程编排（私有个人工作流仓库）。把三套独立技能家族串成一条「分阶段门禁式」开发流水线，跨项目可复用。

## 组成方式

按**生命周期阶段**串行编排、各司其职、互不耦合（非合并、非重写）：

| 阶段 | 技能 | 职责 |
| --- | --- | --- |
| 需求拷问 | `grilling`（别名 `grill-me`） | 有歧义/高风险/新品类才展开完整设计树，结论沉淀记忆 |
| 设计基建 | `create-design-md` / 手写 DESIGN | 先建/校核 `DESIGN.md` 设计规范 |
| UI 验收 | `impeccable` + `design-taste-frontend` | 按平台/坡度选一件主技能，不堆叠 |
| 疑难排错 | `diagnosing-bugs` | 复现→最小化→假设→插桩→修复→回归，零常驻 |

> 备选：`baseline-ui` / `improve-ui` / `fixing-accessibility` / `fixing-metadata` / `fixing-motion-performance` 仅在 impeccable 能力未覆盖时按需调用；`ui-skills-root` 仅作方向未定时的路由器。

## 入口判定

- **A 新功能 / 需求有歧义** → 需求拷问
- **B UI 视觉 / 打磨** → 设计基建 → UI 验收
- **C 出 bug / 性能卡顿** → 直接疑难排错

## 详细流程

- `DEV_WORKFLOW.md` — 完整流水线说明（Gate1 需求拷问 → 设计基建 → UI 验收 → 疑难排错）
- `skills/` — dev-compass 及其编排涉及的技能源码

## 技能说明

> 注：`impeccable`、`design-taste-frontend` 集成自第三方开源技能源码，各自遵守其原始许可证。本仓库仅作个人工作流归档与复用，归集各自仅限本人使用（私有仓库）。

- `dev-compass`：工程罗盘编排核心（SKILL.md）
- `grilling` / `grill-me`：需求拷问
- `diagnosing-bugs`：疑难排错
- `impeccable`：生产级 UI 精修（含 native 分支，依赖 `node scripts/*`）
- `design-taste-frontend`：Web 落地页 / 作品集 anti-slop 设计
# 关键工程决策日志

> 记录 fork patch、换版本、**破坏性接口变更**、架构取舍。
> 格式见 `foray_docs/repository_structure.md` §7.2。

---

## Decision

Date: 2026-02-19

Context: 本仓从 0 初始化。需要确定它在整体架构中的位置与依赖边界。

Decision: 本仓定位为 **R** 层，owner 角色为「集成」，
依赖层级 X2、L1、L2、L3、L4、L5。

Reason:
- 切分判据 C1–C6 见 `foray_docs/repository_structure.md` §1
- 粒度结论：**一个仓 = 一个团队角色 + 一个变更率档位**
- **唯一允许知道全局**的地方，且**不含任何算法逻辑**

Alternatives:
- 并入相邻仓（减少仓库数量，但违反团队边界或变更率差异）

Rejected:
- 按 ROS 2 package 粒度切仓 —— 会导致版本矩阵地狱
- 按分层机械切仓 —— 会切断团队边界（Conway）

## Decision/Date: 2026-11-14

Context:

视觉两个仓分家并改名：`diesel_vision`→`diesel_vision_frontend`（检测 + PnP）、
`diesel_auto_aim`→`diesel_vision_backend`（融合 + 跟踪 + 瞄准）。本仓 README 的
上游依赖表仍写旧名，装配根按表取依赖时会指到不存在的仓。

Decision:

README 上游依赖表改为 `diesel_vision_frontend` · `diesel_vision_backend`；本仓不新增其他
依赖，也不改装配根任务清单。

Reason:

- 装配根最终要 `find_package(diesel_vision_backend)`，名字必须与仓名一致，否则 M6 打 tag 时
  才发现拉不到仓。
- 前端与后端是装配根的两个不同上游：前端提供 Observation，后端提供 WorldSnapshot 与云台指令。

Alternatives:
- 保留旧名并加注释「已改名」：注释不会出现在依赖解析里，等于留着断链

Rejected:
- 在本仓提前写 `package.xml` 依赖：接口尚未冻结，等 I8 的自研仓入账后再落

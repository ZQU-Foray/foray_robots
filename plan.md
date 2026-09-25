# 开发计划

## 任务列表

- [ ] 建 `sentry_bringup` / `infantry_bringup` 两个装配根
- [ ] 抽出 `config/shared.yaml` + `config/<robot_type>.yaml` 两级参数
- [ ] 建场地资产目录并接 Git LFS
- [ ] 建调试装配根与无硬件回放根（与生产根并存、都进 CI）
- [ ] 验证：同一套 L1–L5 代码，三个根启动出三种系统

## 当前 Agent State

PLAN_READY

## Before Snapshot

commit hash:   （首次提交）
branch:        main
modified files: （首次初始化）
risk level:    L1

## 模糊点与待确认项

- 本仓接口尚未冻结，任务清单为**方向性**的，落地顺序以 `foray_docs/algorithm_structure.md` §9 演进阶段为准
- 依赖的自研仓尚未建齐，跨仓任务需等对方 `README.md` 明确接口后再启动

## Vector Backend Status

Backend: Markdown
Status:  ready
Environment: 仓库内 Markdown 文档（无外部向量后端）
Index: 本仓 `README.md` / `tree.md` / `decision.md`
Initialization: 2026-02-19
Commit: （首次提交）

## Acceptance Criteria

- [ ] 本仓能独立 `colcon build`（无代码时跳过）
- [ ] CI 绿灯
- [ ] 每个新增模块带独立可执行测试

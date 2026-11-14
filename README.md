# diesel_robots

> **R** · owner 角色：集成

兵种装配根 + 参数 + 场地资产

**唯一允许知道全局**的地方，且**不含任何算法逻辑**。

---

## 快速开始

> 本仓处于**初始化状态**，尚无源码。以下流程随 P0/P1 落地逐步可用。

```bash
# 1. 拉取（仓齐备后改为从元仓 diesel_ws 一键拉取）
git clone git@github.com:ZQU-Foray/diesel_robots.git
cd diesel_robots

# 2. 依赖安装
rosdep install -y -r --from-paths . --ignore-src --rosdistro humble

# 3. 构建
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release

# 4. 运行
# （待装配根就绪后补充）
```

## 依赖

| 类型 | 依赖 |
|---|---|
| **自研仓** | X2、L1、L2、L3、L4、L5 |
| **第三方** | ROS 2 launch · `robot_state_publisher` · Git LFS（场地资产） |

## 接口

| 产出 | 内容 |
|---|---|
| 装配根 | `sentry_bringup` / `infantry_bringup` |
| 参数 | `config/shared.yaml` + `config/<robot_type>.yaml` |
| 场地资产 | 2D 栅格 + 3D 先验点云（Git LFS） |

## 职责（仅限）



1. 选择并实例化模块组合

2. 注入配置

3. 决定进程/线程/执行器拓扑（哪些模块同进程、哪些独立进程）

4. 设置 namespace / remap / TF 静态外参

5. 切换 sim / real / 无硬件回放



## 禁止



- **包含任何算法逻辑**

- 生产根绝不链接调试逻辑

- 禁止 `#ifdef DEBUG` 式分支——生产根与调试根**都进仓库、都进 CI**



## 参数两级覆盖



```

config/shared.yaml          ← 全兵种共享（坐标系统一、通用阈值）

   ↑ 覆盖

config/<robot_type>.yaml    ← 兵种 + 场景差异（sentry / standard3；sim / reality）

```



## 兵种差异只有三个来源



1. 硬件拓扑 —— `diesel_platform` 的 HAL 抽象接口 + 可替换驱动

2. 传感器配置 —— 驱动插件 + 参数系统

3. 策略与参数 —— L5 行为树/策略数据 + 参数分层覆盖



## 复用度量（本仓是唯一能验证它的地方）



| 指标 | 目标 |

|---|---|

| 新增兵种改动行数 / 总行数 | **< 10%** |

| 跨层反向依赖数 | **0** |

| 策略改动的重编译次数 | **0** |



## 场地资产



用 **Git LFS** 管理（PCD 可达数十 MB）。成熟后拆出 `diesel_field`。

## 上下游

| 方向 | 对象 |
|---|---|
| **上游**（本仓依赖谁） | `diesel_interfaces` · `diesel_platform` · `diesel_localization` · `diesel_vision_frontend` · `diesel_vision_backend` · `diesel_navigation` · `diesel_decision` |
| **下游**（谁依赖本仓） | `diesel_ws` |

## 参考

- [算法结构](https://github.com/ZQU-Foray/foray_docs/blob/main/algorithm_structure.md)
- [仓库结构](https://github.com/ZQU-Foray/foray_docs/blob/main/repository_structure.md)
- [组织贡献指南](https://github.com/ZQU-Foray/.github/blob/main/CONTRIBUTING.md)

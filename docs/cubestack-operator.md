# 平台 Operator（由版本树提供）

公共 installer **不含**平台 operator：它的模块、chart、镜像清单与操作手册都随版本树
（`--release`）分发。

- **它是什么**：一个 CR `CubeStackCluster` 描述整个平台，operator 按组件目录把各组件渲染并
  apply 进集群，管理健康与卸载。`cubestack-operator` 自身由版本树的 helm chart 安装。
- **为什么不在本仓**：它管理的组件（`cubepilot` / `cuberouter` / `cubestack-portal` /
  `cubestack-controller-manager` / `model-collection` 等）不是开源版的一部分。开源在线安装
  （`deployments/scripts/install-online.sh`）显式设 `CUBESTACK_OPERATOR_ENABLED=false`，这些
  组件由使用者手动 `helm install`。
- **它的文档在哪**：随模块一起在私有仓库 `cubestack-release` 的 docs/operator-module.md，
  包含 chart 分析、镜像推送链路、滚动 `:latest` 的三个坑、组件级更新与故障排查。
- **本仓与它的接口**：只有两条 —— `CUBESTACK_OPERATOR_ENABLED`（开关）和
  `CUBESTACK_OPERATOR_NAMESPACE` / `_RELEASE`（覆盖安装时恢复 Ceph 卷消费者用）。
- **模块如何被发现**：版本树的 `modules/` 经 `--release` 自动并入模块根，见
  `deployments/scripts/README.md` 的「额外模块目录」。

## 排障

platform operator 相关故障的排查步骤在私有仓 cubestack-release 的 docs/operator-module.md。本仓
`docs/troubleshooting.md` 里保留的那几条（如 `:latest` 无法触发滚动、`verify_*` 冒烟 CR
误删命名空间）是"读过之后能识别症状"的摘要，完整处置流程见该文档。

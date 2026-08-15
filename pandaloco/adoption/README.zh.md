# Pandaloco 采用控制面

[English](README.md) | 中文

本目录是 Pandaloco 采用 DeepSeek Harness 规则的版本化来源。它标识不可变 runtime epoch，分配跨仓 owner，定义内部执行义务，并记录后续 phase 获准运行前必须具备的证据。

这些文档不会配置或启动 runtime。缺失或为 RED 的要求一律保持 blocked；本目录中的任何文档都不授权模型调用、工具、宿主机写入、部署或公共 API 变更。

## Authority

[`authority-manifest.yaml`](authority-manifest.yaml) 指明已接受决策、精确 source baseline、write set 和 authorization ceiling。[`epochs/epoch-0001.yaml`](epochs/epoch-0001.yaml) 记录初始的 zero-source-delta epoch，但不声称 artifact、configuration 或 runtime 已经过测试。

Pandaloco task service 保留 admission、authentication、attempt fencing、receipt、artifact 与业务终态的 authority。Harness session、turn、item、model、process、checkpoint 或 pull-request completion 只算 evidence。

## Execution path

规定路径依次经过现有 Pandaloco facade、internal runtime selector、一个 legacy 或 DSH epoch、Pandaloco-owned model/tool broker，以及 Pandaloco terminal compare-and-set 操作。生产 DSH 模型调用终止于 Model Plane `/model-tasks` 接口；DSH 不拥有 provider credential、routing、fallback、egress 或 usage accounting authority。

V1 要求一个 active task attempt 独占一个 Linux DSH worker process，并拥有隔离的 root、environment、policy，以及覆盖完整 process tree 且 fail-closed 的 outer sandbox。DSH filesystem/tool policy 只是 inner guard，不构成 host、process、network、device 或 secret isolation。

## Review order

1. 阅读 [`authority-manifest.yaml`](authority-manifest.yaml) 和两份 source audit。
2. 阅读 epoch、attempt、terminal、receipt、persistence、physical execution、runtime selection、observability 和 platform 规则。
3. 评估 [`gates/gate-matrix-v1.yaml`](gates/gate-matrix-v1.yaml)，并使用 [`gates/gate-result-v1.schema.json`](gates/gate-result-v1.schema.json) 校验结果。
4. 使用 [`ownership/cross-repo-dag-v1.yaml`](ownership/cross-repo-dag-v1.yaml) 核验 ownership。
5. 只有在目标 phase 的全部 dependency 通过后，才可使用 upgrade 和 rollback 文档。

## Negative guarantees

- DSH 不会通过本 adoption layer 暴露平行的 public task API。
- task attempt 不会跟随浮动 branch、tag、version、configuration 或 runtime selection。
- 对于同一个 attempt，legacy 与 DSH execution 绝不会产生两条 authoritative effect 或 terminal path。
- Metrics label 排除 task、attempt、process、worker 和 receipt identifier；在 owner 接纳精确 source 之前，centralized log 保持禁用。
- macOS host-native write 或 executable-tool operation 不受支持，AP-03 sandbox reuse 仍不可用。

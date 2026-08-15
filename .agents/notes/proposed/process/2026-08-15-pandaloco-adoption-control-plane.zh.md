# Agent Note: Pandaloco 接纳控制面

Status: proposed

[English](2026-08-15-pandaloco-adoption-control-plane.md) | 中文

## 问题

Pandaloco 需要吸收 DeepSeek Harness 的能力，同时不能把公共任务、模型提供方、作用、部署或业务终态权威转交给 harness。上游项目仍处于预发布阶段，并允许对源码、配置、协议、持久化和插件作破坏性修改，因此只依赖发布包或保存一组未跟踪的本地补丁，既无法准确识别实际运行内容，也无法让升级可重现。

Pandaloco 已经运行 General 和 Research agent、Model Plane、监控、受控物理部署以及领域所有者管理的作用。绕过这些服务接入 DSH，会建立第二条公共任务路径、重复模型提供方治理、在 shadow 期间允许两条权威作用路径，或者把 harness 完成误当成业务结果。

DSH 的文件系统和工具控制也不足以充当宿主隔离。它们本身不能约束完整进程树、网络、宿主读取、设备或环境凭据。

## 提案

Pandaloco 将维护 tracked fork，并把每个可执行 DSH 版本标识为不可变的 runtime epoch（运行时纪元），其中包含确定的上游与 fork 源码、下游差异、已解析配置、已测试产物、来源证明、SBOM、许可证、补丁账本、质量门禁矩阵、存储策略和回滚身份。候选与当前 epoch 将并存验证；浮动升级或覆盖升级均为非法。

静态 gate 声明使用两层身份。Candidate-subject manifest 排除 gate result 和最终 distribution manifest；每个 G0-G5 result 绑定这个稳定 subject 及其 evidence；最终 distribution manifest 再覆盖这些 result。缺少该版本化绑定的静态 PASS 无效。

DSH 将作为现有 Pandaloco facade 后面的内部执行引擎。内部选择器会在一个已准入任务 attempt 创建时，将它终身绑定到唯一的 legacy 或 DSH epoch。Pandaloco 保留准入、认证、attempt fencing、工具和 specialist 授权、receipt、产物以及唯一业务终态 compare-and-set 操作。

生产 DSH 模型调用将使用 Model Plane 的 `/model-tasks` 接口。模型提供方凭据、profile、路由、fallback、egress、错误归一化、延迟和用量仍由 Model Plane 负责。DSH 只把认知工作映射为模型任务，生产 epoch 不加载直连模型提供方。

V1 会让一个 active task attempt 独占一个 Linux DSH worker 进程，并使用私有 root、干净环境、显式 policy，以及一个覆盖完整进程树和辅助可执行程序的 fail-closed 外层权威。DSH policy 仍然只是内层文件系统和工具 guard。外层沙箱缺失或不可验证时，执行必须被拒绝。

Legacy 与 DSH runtime 将并存，直到另行评审通过移除旧 runtime 的决定。默认 shadow 评估会回放已记录输入且不产生作用。Live dual execution 必须具有明确的 worker、GPU、模型驻留、并发、时间、成本和 effect-suppression 预算，并且只能有一条路径发布权威作用或终态。

指标将使用现有 Prometheus 和 Grafana 路径，并只采用有界标签。Per-attempt 标识留在 Pandaloco 所有的结构化证据中，不进入指标标签。集中日志在日志所有者准入精确 DSH source 前保持关闭；默认 telemetry 排除提示词、模型输出、工具 payload、secret 和 credential。

物理注册表、服务接入以及部署、切换和回滚操作仍分别由物理控制、agent-services 和 deploy-ops 仓库拥有。DSH fork 携带可部署身份和接纳规则，但不能自行激活。

## 考虑过的替代方案

**只使用已发布的 DSH 包。** 拒绝，因为包 API 无法保存审计破坏性预发布升级所需的精确上游源码、配置、本地适配和补丁重放信息。

**Fork 后自由修改 DSH。** 拒绝，因为无界的下游代码会增加吸收上游变化的成本，也会掩盖哪些补丁可以替换或删除。

**用 DSH 替换现有 Agent Plane。** 拒绝，因为现有 Pandaloco facade、任务权威、Model Plane、监控、部署控制和领域 receipt 承担了 DSH 无法替代的职责。

**让 DSH 直接调用模型提供方。** 拒绝，因为这会重复凭据、路由、fallback、egress、错误和用量治理，并绕过当前资源控制。

**把 DSH 文件系统 policy 当成完整沙箱。** 拒绝，因为它不提供覆盖完整进程树的宿主、网络、syscall、设备、secret 和宿主读取隔离。

**默认同时运行 live legacy 与 DSH shadow。** 拒绝，因为它会让模型和工具压力翻倍，而且在证明容量与 effect suppression 前可能产生重复作用。

**原地升级共享 runtime。** 拒绝，因为现有 attempt 会失去不可变 runtime 身份，而且回滚将依赖可变文件系统和持久化状态。

## 验收标准

- Fork 包含版本化接纳文件、candidate-subject 与 distribution manifest、schema-valid G0-G5 result，以及完整的英文、中文和配对记录三件套。
- 机器可读规则会拒绝生产环境直连模型提供方、平行公共 API、含糊 runtime selection、多条权威作用路径、无所有者部署、未准入日志、高基数指标标签和无预算 live shadow execution。
- Epoch 身份覆盖源码、配置、产物、来源证明、供应链、补丁、存储、质量门禁和回滚，同时不会声称不存在的证据。
- 物理所有者和依赖记录会让未完成的服务接入、容量、watch 健康、集中日志、外层沙箱和 runtime 证据保持 RED。
- 静态验证无需运行 DSH，即可重现 manifest 以及每一项结构检查的通过或失败。

## 风险

这些控制文档增加了维护成本；如果升级绕过评审，它们可能与源码或物理所有者约定发生漂移。静态验证无法证明 runtime 隔离、模型提供方路由、effect suppression、性能或恢复。与共享原地 runtime 相比，per-task 进程和 side-by-side epoch 会消耗更多内存、存储、GPU 余量和运维投入。该提案接受这些成本，以保留任务隔离、可重现性、受控 fork 漂移和回滚能力。

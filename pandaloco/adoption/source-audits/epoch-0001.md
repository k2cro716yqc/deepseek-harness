# Epoch 0001 source audit

## Source identity

- Official repository: `https://github.com/deepseek-ai/deepseek-harness.git`
- Fork repository: `https://github.com/k2cro716yqc/deepseek-harness.git`
- Candidate branch: `pandaloco/wp0-adoption-control-plane`
- Commit: `47f943859bef60e4160492346772ded9b24f765a`
- Tree: `f904efab9ef435201d6ba4da88a34d6366568272`
- Candidate divergence from `upstream/master` before WP-0 authoring: `0 ahead / 0 behind`
- Downstream source delta before WP-0 authoring: empty

## Relevant DSH facts

- `packages/sdk/client/src/client.ts:196-209`: the TypeScript SDK may inherit `process.env` when its caller does not provide an explicit environment.
- `packages/sdk/protocol/README.md:35-39`: the JSON-RPC transport has no version negotiation, and process termination is the available transport-wide cancellation mechanism.
- `packages/sdk/server/README.md:43-47`: the server has no per-session close, prompt cancellation, or per-prompt result operation.
- `docs/subsystems/sandbox.md:5-23`: the DSH sandbox describes same-world filesystem effects; it does not confine network, process, device, host filesystem reads, or secrets.
- `apps/cli/reference/README.md:68-76`: the default CLI session uses workspace-write semantics, while reads, network, processes, and credential discovery extend beyond that write policy.
- `examples/jsonrpc-agent/README.md:31-40`: the minimal example includes a persistent shell and unrestricted access mode, so examples are not safe production defaults.
- `packages/mcp/mcp-client/src/transport.ts:33-38`: a stdio MCP transport spawns a child process, so the outer sandbox must enclose auxiliary executable paths and descendants.
- `docs/architecture.md:94`: model-visible state is reconstructed from the DSH session log, but that log is not Pandaloco task, effect, receipt, or business terminal authority.

## Consequence

Epoch 0001 is a source baseline only. It has no tested artifact, resolved production configuration, outer-sandbox evidence, runtime evidence, or promotion authority. The gate matrix keeps every execution phase red until its owner supplies exact evidence.

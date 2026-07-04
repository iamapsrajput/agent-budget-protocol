# Agent Budget Protocol

**A real-time budget decision plane for AI agent runs.** Draft RFC, open for
feedback.

## The problem, in three sentences

AI agents run loops, and every iteration resends the accumulated context, so
cost grows superlinearly with steps. Existing gateway budgets attach to API
keys, users, or teams over monthly windows; a runaway autonomous session can
exhaust a month's budget in an hour before any of them react. The damage unit
for agent spend is the **run**, and no mainstream gateway enforces per-run
ceilings.

## What this RFC proposes

A budget authority that sits in front of provider calls, embeddable as a gateway
hook, sidecar, or SDK middleware:

- **Atomic reservation** before each call (reserve estimated cost → forward →
  commit actuals → refund slack), so parallel tool calls cannot race past a
  ceiling
- **Run-scoped ceilings** with identity binding, alongside user and key scopes
- **Fail-closed pricing**: a model with no known price is unroutable, never free
- **A machine-readable budget protocol** (response headers + RFC 9457 problem
  details) so agents can downshift to cheaper, capability-valid models when
  budget runs low, instead of failing blind
- **Staged adoption**: advisory → downgrade → enforce

Read the full document:
**[RFC.md](https://github.com/iamapsrajput/agent-budget-protocol/blob/main/RFC.md)**

## Status

Draft v3. Actively seeking feedback from platform and infra engineers running
LLM gateways in production. Comment in
[Discussions](https://github.com/iamapsrajput/agent-budget-protocol/discussions/1#discussion-10371306),
especially on the open questions in RFC section 7.

## Relationship to existing tools

This is not a model gateway or provider abstraction and does not compete with
LiteLLM, OpenRouter, or Portkey; the first planned implementation is a LiteLLM
pre-call hook that composes with existing deployments. The design grew out of
[ModelMuxer](https://github.com/iamapsrajput/modelmuxer), an LLM router with
pre-call budget gating that I have built and maintained since 2025; this RFC is
a redesign of its budget layer around what production agent workloads actually
break.

## License

Apache 2.0. The protocol is intended to be implementable by any gateway or agent
framework without restriction.

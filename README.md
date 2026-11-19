# Correctover Findings Registry — Agent Reliability Findings, Numbered

Independent findings across AI agent architectures. Each finding gets a stable ID (`CF-YYYY-NNNN`) and a public paper trail. Scope is deliberately **cross-architecture**: tool-permission trust granularity, cryptographic verification behavior, client key management, session/identity binding — not limited to any single protocol.

Findings are responsibly disclosed upstream first. Links below point to the public upstream records.

## 2026

| ID | Vendor | Repo / Asset | Architecture class | Status | Public record |
|---|---|---|---|---|---|
| CF-2026-0001 | paulmillr (community) | noble-curves | Cryptography — Ed25519 verification accepts small-order public keys (RFC 8032 §8.8) across 16 library versions | Open upstream | [noble-curves#261](https://github.com/paulmillr/noble-curves/issues/261) · reproducible corpus: [Zenodo 229,539-vector conformance set](https://doi.org/10.5281/zenodo.16884613) |
| CF-2026-0002 | OpenAI | openai-agents-python | Tool-permission — server-declared `_meta` overrides application policy precedence | Open upstream | [openai-agents-python#6093](https://github.com/openai/openai-agents-python/issues/6093) |
| CF-2026-0003 | OpenAI | openai-agents-python | Tool-permission — dict-shaped `needs_approval` fails open for unlisted tools on remote-controlled tool lists | Open upstream | [openai-agents-python#6094](https://github.com/openai/openai-agents-python/issues/6094) |
| CF-2026-0004 | Alibaba | QwenLM/qwen-code | Tool-permission — AUTO-mode classifier bypassed entirely by server-level MCP trust; newly added tools execute with zero review | **Vendor-confirmed (P1, category/security, scope/vulnerability)** — official triage bot verified all three code claims, 2026-11-19 | [qwen-code#17854](https://github.com/QwenLM/qwen-code/issues/17854) |
| CF-2026-0005 | Google | google-gemini/gemini-cli | Tool-permission — server-level MCP trust approves all future tools, no first-seen confirmation (origin of the family pattern; affects downstream forks) | Open upstream | [gemini-cli#30369](https://github.com/google-gemini/gemini-cli/issues/30369) |
| CF-2026-0006 | ByteDance | UI-TARS-desktop | Client key management — app-level device-registration signing key compiled into every distributed Electron bundle | Coordinated disclosure (upstream repo archived) | Build-chain evidence documented in CF log; publication pending vendor contact |
| CF-2026-0007 | Amazon (AWS) | amazon-q-developer-cli | Permission boundary — built-in `ExecuteCmd`/`SpawnSubagent` return unconditional Allow (no allowed_tools consult, no approval flow) while third-party MCP tools get an `Ask` fallback; plus Ls gated by fs_write settings | Open upstream | [amazon-q-developer-cli#3936](https://github.com/aws/amazon-q-developer-cli/issues/3936) |

## Family patterns

- **Server-granularity trust antipattern** (CF-2026-0002/0003/0004/0005): four independent implementations — OpenAI, Alibaba, Google (and its fork family) — grant remote-controlled servers blanket approval that outlives the tool list the user actually saw. We track this as one antipattern with per-vendor instances.
- **Client-held signing keys** (CF-2026-0006): attestation keys shipped inside user-extractable bundles invert the trust direction of the protocol they sign.

## Method

Every finding is verified against published source (release wheels/packages or repo `main`) before disclosure; claims without a reproducible chain are not filed. Corrections are welcome and will be logged.

---
Maintained by Correctover — AI Reliability. Independent, vendor-neutral.

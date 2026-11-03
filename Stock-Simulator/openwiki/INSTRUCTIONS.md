# OpenWiki brief — Circles / CXOS (code-level depth)

This brief steers every `openwiki --init` and `--update` run. Prefer **agent-usable, evidence-backed system truths** over marketing overviews. Treat source and tests as authoritative.

## Audience and purpose

Document this repository so a coding or ops agent can:

1. Map a symptom (issue text, stack trace, log line, metric name) to the owning module and call path.
2. Understand runtime behavior, failure modes, and safe change boundaries without reading the entire tree.
3. See how this repo contracts with sibling Circles services.

## Required page coverage

Plan and maintain pages that cover at least:

| Topic | Must include |
| --- | --- |
| **Quickstart for agents** | How to run locally, key entrypoints, “where to look first” for common failure classes |
| **Architecture & ownership** | Top-level packages/modules, who owns what, inbound/outbound dependencies |
| **Request / job lifecycle** | End-to-end paths for primary APIs, workers, agents, or pipelines (sequence or flowchart Mermaid) |
| **Data model & persistence** | Schemas, stores, migrations, idempotency, consistency assumptions |
| **Configuration** | Env vars / config files / feature flags — name, default, where read, effect when wrong |
| **Auth, tenancy, security** | Trust boundaries, authn/z, secrets handling, PII |
| **Errors & observability** | Error types/codes, retry/backoff, timeouts, log fingerprints, trace/metric names agents can match |
| **Integrations** | External APIs, queues, sibling Circles repos, shared packages — contract + failure behavior |
| **Ops & runbooks** | Deploy shape, health checks, common incident signatures → likely files |

Skip fluff: marketing copy, restating the README without new evidence, and enumerating every trivial helper.

## Depth standard (non-negotiable)

For each material component or flow:

- **Behavior**: what it does at runtime, not just its name.
- **Evidence**: Claims must cite `repo://path/to/file#Lstart-Lend` (or equivalent) for each substantive truth.
- **Call / data flow**: who calls whom; inputs/outputs; side effects.
- **Invariants**: ordering, idempotency, concurrency, “must never”.
- **Failure semantics**: what throws/returns/retries; how callers observe it; log strings or codes.
- **Extension boundaries**: safe hooks vs dangerous internals.

Atomic Claims = one falsifiable system truth (not one Claim per symbol). Prefer completeness of material truths over minimizing Claim count; drop trivia and duplicates.

## Cross-repo routing hints

Always document fingerprints that help a multi-repo routing agent:

- Distinct log prefixes, logger names, error class names, HTTP paths, gRPC methods, queue topics, metric names.
- Explicit “owned by this repo” vs “delegates to \<sibling\>” notes when a path crosses services.
- Shared library symbols that appear in stack traces for *callers* of this repo.

## Diagrams

Use Mermaid when it clarifies more than prose: sequence (runtime), flowchart (control), ER (data), state (lifecycles). Keep diagrams grounded in inspected source; omit decorative diagrams.

## Tone and format

- Concise, technical Markdown. Prefer tables for config and error maps.
- Link related wiki pages; keep `quickstart.md` as the agent entry hub.
- Do **not** invent APIs or behaviors absent from source/tests. Mark genuine unknowns as verification gaps.

## Out of scope / de-emphasize

- Generated artifacts, vendored deps, and paths listed in `.openwikiignore`
- One-off scripts with no production ownership unless they explain a real ops path
- UI chrome that has no bearing on backend/agent behavior (unless this *is* a UI-primary repo)

## Update policy

On `--update`, reconcile stale Claims against current evidence. Prefer revising affected pages over rewriting the whole wiki. Preserve user-authored sections of this file forever.

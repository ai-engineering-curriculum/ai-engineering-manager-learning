# Agent / Tool-Use Authorisation at the Product Boundary

A model that reads and writes text is a model. A model that calls tools is a system acting on the product's behalf. Every tool call the model can make is a potential blast radius — the smallest unit of damage that single call can do if the model misfires or an adversary induces the misfire. The manager's job on agentic surfaces is to be the one who has read, and personally signed, the list of tools the agent can call, the scopes those tools hold, and the checkpoints on the actions that matter.

The craft of implementing OAuth flows, scoped service identities, MCP server authorisation, tool-call allow-lists, and blast-radius containment belongs to the `agent-platform-engineer` (level 40) and the `agentic-ai-engineer` (level 30, feature-level work). The manager owns the **boundary** — which tools exist, which actions require a human, what the product promises about what the agent will never do on its own. This is one of the places the manager's judgement is irreducible.

## Motivation — the four agent disasters

Four shapes of agent incident the authorisation boundary is designed to prevent.

- **The tool nobody authorised.** The agent grew a tool by accretion — an engineer wired `send_slack` to help debug, an intern added `create_calendar_event` for a hack-week demo, the production catalogue inherited them silently. The first time a prompt-injection path discovers `send_slack`, the agent is leaking company-confidential customer context into a public channel. Nobody signed on `send_slack`; everybody owns the fallout.
- **The scope that was broader than the product needed.** The OAuth token the agent used for `read_calendar` was issued at a scope that also granted `delete_calendar_event`. The product never intended the agent to delete — but a prompt-injection vector in a retrieved meeting description told the agent to "clean up this calendar," and the model (optimising for helpfulness) complied. The token's scope was the blast radius.
- **The missing HITL.** The agent has a `send_wire_transfer` tool. Someone added it for an internal operations product and did not wire a human confirmation because the first users were trusted. When the product expanded to a broader audience, the HITL remained absent. A user, following a social-engineered instruction, made a request the agent translated into a large transfer; by the time the human in the loop (the user) noticed, the transfer had cleared.
- **The containment that did not contain.** A dev engineer wired a tool-call rate limit and called it blast-radius containment. The attacker learned the limit was per-session, opened ten sessions, and did ten times the damage. The rate limit was not containment; it was a bump in the road for a determined adversary.

Each is preventable with an authorisation boundary review — the artefact this chapter builds.

## Core concept 1 — the five properties of the boundary

Every agent's tool-use authorisation is characterised along five properties. The manager's review of a new tool (or of the existing catalogue) touches each.

### Property 1 — identity

**Which identity is the tool call made as?** Four common answers, each with different implications.

- **User identity** — the agent acts as the user whose session it is in, using credentials (OAuth token, API key, session cookie) bound to that user. Advantages: downstream services see the user, standard authorisation applies, audit logs name the user. Disadvantages: an adversarial prompt in the user's session can do anything the user is authorised to do, which may be too much.
- **Service identity (scoped)** — the agent acts as a dedicated service account with a scoped identity that is *less* privileged than the user. Advantages: the blast radius is explicitly narrowed; the downstream service can enforce service-specific policy. Disadvantages: audit trails need to link the service action back to the originating user for accountability (R15 depth).
- **Service identity (broad)** — the agent acts as a service account with broad privileges across the product's tenancy. Typically wrong on multi-tenant products — a prompt-injection in tenant A's session could induce actions against tenant B's data if the service identity spans tenants. The default for internal single-tenant products; the wrong default everywhere else.
- **Delegated identity** — the agent acts under a delegation grant from the user, often implemented as OAuth with specific scopes requested at session start. Standard pattern when the agent crosses product boundaries (e.g. our agent calls your Google Calendar).

The spec from chapter 02 section 4 names the identity for each tool. The review verifies it is the narrowest identity that still allows the tool to work.

### Property 2 — scope

**What is the identity's scope?** Scopes govern what the identity can do. The review asks, for each tool:

- Is the scope **minimum** — does the identity have exactly the permissions needed and no more? An identity for `read_customer_record` should not include `write_customer_record`.
- Is the scope **durable** — does the scope persist beyond the session, and if so, for how long? A session-scoped token is a smaller blast radius than a persistent token.
- Is the scope **revocable** — can the issuing authority revoke the scope instantly in an incident? Static API keys in a config file are not revocable fast enough to matter in an incident.
- Is the scope **tenant-isolated** — on a multi-tenant product, does the scope bind to the current tenant and refuse to cross tenants? If not, the first prompt-injection crossing tenants exposes the entire architecture.

Standards worth naming in the spec: OAuth 2.0 (and 2.1 draft), OpenID Connect for identity, the scopes pattern as implemented by the specific downstream service. Where the agent calls internal services, the pattern is typically a service-to-service mTLS flow with a scoped JWT, delegated from the user's identity via a token-exchange protocol.

### Property 3 — pre-invocation checks

**What runs before the tool call is dispatched?** Four layers, each a decision in the spec.

- **Input schema validation.** Tool inputs are validated against a JSON Schema (or equivalent) before the call. A tool whose input the agent can shape freely is a tool whose input an attacker can also shape freely.
- **Authorisation check.** The pre-invocation layer verifies the current identity holds the scope required for this tool call. The downstream service is also doing this (defence in depth); the pre-invocation check prevents the call ever reaching the service when it would be refused.
- **Policy check.** A policy engine evaluates the proposed call against product rules — tenant isolation, cost caps, rate limits, feature-flag gates. Example: "this tool is only available on surfaces where severity-tier ≤ MEDIUM"; the policy engine blocks the call on other surfaces.
- **HITL trigger.** If the tool's HITL rule fires (see property 4), the pre-invocation layer routes the call to a human-approval queue instead of dispatching it.

The **Model Context Protocol (MCP)** — Anthropic's open protocol for connecting models to tools and data sources — has become a common shape for the tool-invocation surface across the industry. An MCP server exposes tools under a protocol contract; the pre-invocation checks live either in the host application (client-side) or in the MCP server itself (server-side), depending on the deployment. The manager's review of an MCP-based surface checks that the MCP server's authorisation model is explicit and that the host application does not bypass it.

### Property 4 — human-in-the-loop checkpoints

**Which actions require a human to say yes?** The decision rule: any action with meaningful external side-effects **or** any action over the product's declared blast-radius threshold.

Side-effect categories that typically require HITL:

- **Communication actions** — send email, post to a channel, send an SMS, call an API that notifies third parties.
- **Financial actions** — initiate payment, approve expense, change subscription, apply credit.
- **Data-write actions over a threshold** — delete more than N records, modify PII fields, update account permissions.
- **External-system state changes** — deploy a service, modify infrastructure, run a script with privileged credentials.
- **Content-publishing actions** — publish a post, change a public page, update documentation.
- **Scheduled actions** — any action that fires after the user leaves the session is a meta-side-effect; HITL on the schedule itself may be required.

HITL is not a dialog box pattern — it is a product obligation. The spec names the UX (inline confirmation; out-of-band approval; approve-with-magic-link), the SLA (how long the action pends awaiting approval before being cancelled), and the reviewer's view (what information the human has at approval time; chapter 02 section 5).

Patterns to defeat:

- The **habitual click-through** — a confirmation prompt that fires so often users dismiss it without reading. Reserve HITL for actions it is actually worth stopping on; combine routine actions into a daily digest if the frequency is high.
- The **irrevocable confirmation** — a "yes" that cannot be undone within a reasonable window. Where feasible, every HITL action has a brief cancellation window during which the user can retract.
- The **inherited HITL** — the HITL on the compound action ("book my trip") that bundles many single actions, each of which should have had its own confirmation. If the user said yes to "book my trip," they did not say yes to "and also upgrade to first class."

### Property 5 — blast-radius containment

**What is the worst a single misfire can do?** Blast-radius containment is the discipline of making the worst-case bounded.

- **Rate limits per session, per user, per tenant.** A tool that can be called a thousand times per session is not rate-limited for safety; it is rate-limited for cost.
- **Cost caps per session, per user, per day.** A loop that fires the tool catalogue until budget exhaustion is a known agent failure mode; the budget is the containment.
- **Allow-lists for arguments.** A `send_email` tool whose `to` field is unrestricted is unsafe; one whose `to` field is restricted to domains the user has previously corresponded with, or to a user's own account, is bounded.
- **Allow-lists for tools per surface.** A surface may receive only a subset of the full tool catalogue; even if an injection convinces the model to want to call a dangerous tool, the tool is not in the surface's manifest.
- **Session-level isolation.** Tool calls in one session cannot read or modify state established by another session; a prompt-injection in session A cannot affect session B.
- **Kill-switches.** Per-tool feature flags that let the on-call rotation disable a tool across all sessions within seconds during an incident. The runbook (chapter 05 pin) names the kill-switch for each tool.
- **Immutable audit logs.** Every tool call writes to an append-only log; the log is retained for the compliance window; the format supports investigation (user id, session id, tool, arguments, outcome, timestamp).

## Core concept 2 — the authorisation-boundary review

The manager runs an authorisation-boundary review (a) when a new tool is proposed, (b) when an existing tool's scope changes, (c) on a quarterly cadence against the full catalogue, and (d) triggered by a red-team finding that touches tool authorisation.

The review produces (and updates) a one-to-two-page **boundary document** per surface:

```markdown
# Agent authorisation boundary — <surface name>
Owner: <manager> · Reviewed: <date> · Next review: <date>

## Surface
<one paragraph: which feature, which audience, which severity tier>

## Tool catalogue
| Tool | Purpose | Identity | Scope | HITL rule | Blast-radius bounds |
|---|---|---|---|---|---|
| read_record | fetch customer record for Q&A | user OAuth token | read:customer in current tenant | none | rate-limit 60/min/session |
| update_record | update customer field | user OAuth token | write:customer in current tenant, restricted to {notes, tags} fields | inline confirmation per write | rate-limit 10/min/session |
| send_email | send email on user's behalf | service identity, scoped to current user's sending address | send:email from user_domain to {prior_correspondents} | inline confirmation per send | rate-limit 5/min/session; cost-cap $0.05/session |
| run_report | generate and return PDF | service identity, scoped to current tenant | read:analytics in current tenant | none | cost-cap $2.00/session; exclusive-lock to prevent concurrent costly calls |

## Removed since last review
- `delete_record` — removed 2026-10-15; replaced by HITL request to support team.

## Known risks
- Long-term: `send_email` scope depends on {prior_correspondents} allow-list being current; stale allow-list is a risk. Accepted; refresh cadence weekly.

## Kill-switches
- Feature flag `tool:send_email:enabled` — disables across all sessions in <10s.
- Feature flag `tool:run_report:enabled` — disables across all sessions in <10s.
- Session-global kill `agent:surface:disabled` — disables the entire surface.

## Audit logs
- Written to <log sink> with retention <N years>; indexed by session_id, user_id, tool.
- On-call can replay the last 24h of calls for incident investigation per the chapter 05 runbook.
```

The review decides, on each new or changed tool:

- Does the identity match the narrowest sufficient option?
- Is the scope minimum, durable, revocable, tenant-isolated?
- Are the pre-invocation checks present (schema + authz + policy + HITL trigger where required)?
- Does the HITL rule match the side-effect severity?
- Are the blast-radius bounds (rate limits, cost caps, allow-lists, kill-switches, audit logs) specified?

A tool that fails any of these is not added to the catalogue. A tool already in the catalogue that fails any of these on a review is removed, HITL-gated, or scope-tightened in the next change.

## Core concept 3 — the deferrals

The manager signs the boundary; the platform engineers build it. Three specific deferrals.

### Deferral 1 — OAuth / service-identity implementation

The craft of implementing OAuth 2.0 / 2.1 flows, OpenID Connect token verification, OAuth token-exchange (RFC 8693) for service-to-service delegated identity, mTLS, JWT signing and rotation, and the per-downstream-service scope conventions belongs to the `agent-platform-engineer`. The manager reads the output (which scopes, in which flow, bound to which identity); the platform engineer implements it.

### Deferral 2 — MCP contract depth

The Model Context Protocol (MCP) provides an open standard for how models connect to tools and data sources — the specification evolves, and the ecosystem of MCP servers and clients is growing quickly. At manager depth: know that MCP exists, know that it is the dominant protocol for agent tool access across providers, and require that MCP-based surfaces have an explicit authorisation model (which the MCP server implements, which the host client enforces). The IC craft of writing a well-authored MCP server, handling its concurrency model, and ensuring the authorisation model is correct belongs to the `agent-platform-engineer` or an equivalent IC.

### Deferral 3 — HITL UX craft

The craft of designing a human-in-the-loop UX that users actually read is a product-design problem that belongs to product-design partners (chapter 11 of mod-111 covers the cross-functional partnership). The manager specifies the HITL *where* and the HITL *information content*; the product designer builds the UX.

## Core concept 4 — anti-patterns to name and defeat

- **Scope creep via "just one more tool."** A tool catalogue that grows by one every two weeks without a boundary-review update is a catalogue that will eventually include a tool nobody can justify. The review cadence is the discipline; the manager signs new additions.
- **HITL as the sole mitigation.** HITL is a layer, not the layer. A product that says "the human will approve it" and has no scope restriction, no allow-list, no cost cap is a product that will discover one day that the human-in-the-loop also gets tired, also gets busy, also gets social-engineered. Belt + braces.
- **"We trust the model."** Modern frontier models are safer than they were two years ago; they are still non-deterministic systems operating on adversarial inputs. The boundary is not a slight; it is the product's commitment to its users about what the model will *not* do unilaterally.
- **The silent service identity.** Service identities created in a cloud provider console, stored in a secrets manager, used by the agent, and never reviewed. Audit the service-identity catalogue the same way you audit the tool catalogue; prefer short-lived, workload-bound identities (OIDC-federated, SPIFFE / SPIRE where available).
- **Tool authorisation at the LLM prompt layer.** "The system prompt tells the model not to call delete_record unless the user confirmed it." No. System prompts are advisory. Tool authorisation is enforced at the transport layer and below; the prompt is the UX, not the control.

## What good looks like

- Every agentic surface has a boundary document, dated, signed, and current.
- Any new tool goes through a boundary review before it ships; the manager personally signs the delta.
- Service identities are workload-bound and short-lived; static API keys in config files are an exception documented and justified.
- HITL checkpoints fire on every meaningful side-effect; the HITL UX is co-designed with product design, not the engineer.
- Kill-switches for each tool are tested in a dry-run each quarter.
- Audit logs retain the full tool-call history for the compliance window and are query-able in incident response.

## What bad looks like

- The tool catalogue is "whatever is wired in the code"; nobody can produce a list.
- Scopes are "whatever the token had when it was issued"; no review of minimum scope.
- HITL fires inconsistently across similar actions, or is absent on actions that need it.
- Blast-radius containment is "the rate limit on the provider API," which is the provider's business, not the product's.
- The authorisation of MCP tools is "whatever the MCP server does"; the host client has not been reviewed.

## Summary

- The agent authorisation boundary is a manager-owned artefact: a one-to-two-page document per surface listing tools, identities, scopes, HITL rules, and blast-radius bounds; dated, signed, and reviewed.
- Five properties characterise each tool: identity (user / service-scoped / service-broad / delegated), scope (minimum, durable, revocable, tenant-isolated), pre-invocation checks (schema + authz + policy + HITL trigger), HITL checkpoints (named by side-effect category), and blast-radius containment (rate limits, cost caps, allow-lists, kill-switches, audit logs).
- The manager signs; the `agent-platform-engineer` and `agentic-ai-engineer` build. OAuth / MCP / HITL-UX craft defers; the boundary decision does not.
- The review runs on new-tool proposals, on scope changes, on a quarterly cadence, and triggered by red-team findings. A tool that fails any of the five properties is not shipped as-is.

The next chapter picks up the final reading discipline: the EU AI Act's Articles 14, 15, and 50, and how the manager translates them into engineering deliverables the team ships without replacing legal review.

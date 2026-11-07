# Exercise 04: Agent Tool-Use Authorization Boundary Review

**Estimated effort:** 3 hours · **Output:** (a) a 1–2 page agent authorisation boundary document for one agentic surface, (b) a review memo on a proposed new tool addition (or an existing-tool scope change) that exercises the five-property review, and (c) a half-page EU AI Act Article 14 / 15.C cross-reference showing how the boundary feeds the compliance-mapping memo.

## Objective

Produce the three artefacts chapters 07 and 08 commission together. The boundary document is the per-surface authoritative record of what the agent can do on the product's behalf. The review memo demonstrates the five-property review in action. The Article 14 / 15.C cross-reference closes the loop into the EU AI Act compliance-mapping memo.

A peer AI Engineering Manager, on reading the three artefacts, should (a) be able to answer "what tools can this agent call, under what identity, with what HITL?" in under a minute, and (b) agree with or push back concretely on the review-memo decision.

## Prerequisites

- Chapter 01 at a glance for the manager / platform-engineer split on authorisation craft.
- Chapter 02 section 4 (tool-invocation policy in the guardrails spec) — the spec's tool-policy section feeds this exercise.
- Chapter 07 (`Agent / Tool-Use Authorisation at the Product Boundary`) — the five properties, the boundary-review ritual, the deferrals.
- Chapter 08 (EU AI Act Articles 14 / 15.C) for the compliance cross-reference.
- Exercises 01 and 02 for the feature context (if you use the same feature, the artefacts compose).
- A real agentic surface (agent calls tools) you know enough about to write the boundary honestly. If no agentic surface in the target product has tools, use a composite feature on a composite team — a document-analysis agent with retrieval + annotation tools is a common pattern.

## Problem statement

Three artefacts, in markdown, filed where your agent platform's governance lives.

### Artefact A — the agent authorisation boundary document (1–2 pages, 90 minutes)

The shape from chapter 07 core concept 2. Content:

1. **Surface.** One paragraph on the feature; audience; severity tier (consistent with the spec from exercise 01); which agent framework / platform the surface uses (the host application, the MCP server, the LLM provider's tool-use API).
2. **Tool catalogue.** A table with one row per tool. Columns (five properties from chapter 07 core concept 1):
   - Tool name and purpose (one sentence).
   - Identity (user / service-scoped / service-broad / delegated; name the token flow).
   - Scope (the specific permissions the identity holds — "read:customer in current tenant"; "write:email from user_domain to {prior_correspondents}"; "read:analytics in current tenant").
   - Pre-invocation checks (schema validation; authz check; policy check; HITL trigger — list which apply).
   - HITL rule (none / inline confirmation / out-of-band approval / dedicated review queue; with the one-sentence reason).
   - Blast-radius bounds (rate limit; cost cap; argument allow-lists; kill-switch id; audit log sink).
3. **Removed-since-last-review.** Tools removed, with the date and reason. An empty section is valid; the section exists so removals are not silent.
4. **Known risks and accepted mitigations.** Risks the current configuration carries — e.g. "the `send_email` scope depends on {prior_correspondents} allow-list being current; stale allow-list is a risk" — with mitigations (refresh cadence, review-trigger).
5. **Kill-switches.** The specific feature flags that disable the tools (or the surface); the SLA for propagation (how quickly a toggle takes effect); the runbook page that owns the kill-switch invocation (ties to mod-107).
6. **Audit logs.** Where tool-call audit logs live; the retention window; the query mechanism the on-call rotation uses (chapter 05 pin).
7. **Review metadata.** Owner (manager), last reviewed date, next review date, reviewers (manager + `agent-platform-engineer` + security / risk partner where applicable).

Minimum content: ≥ 3 tools, ≥ 1 tool with HITL required, ≥ 1 tool whose identity is a scoped service identity (not just "user OAuth token for everything"). If the product does not currently have those shapes, use the exercise to design the shapes you would require.

### Artefact B — review memo on a proposed tool change (1 page, 45 minutes)

Pick one of the following:

- A **new tool** being added to the surface (e.g. a `schedule_meeting` tool to a customer-success agent; a `run_sql_query` tool to an analytics agent; a `create_jira_ticket` tool to a support agent).
- An **existing tool's scope change** (e.g. expanding `read:customer` to include a new data class; adding an argument the tool previously did not accept).
- A **tool deprecation** (e.g. removing a tool the surface no longer needs; replacing one tool with a more scoped set).

The memo walks the five-property review and decides. Sections:

1. **Proposal** — one paragraph on what is being proposed and the product motivation.
2. **Identity review** — which identity, why; is it the narrowest sufficient option; what alternatives were considered.
3. **Scope review** — specific permissions requested; minimum / durable / revocable / tenant-isolated checks; one-sentence justification per check.
4. **Pre-invocation checks review** — schema validation (name the schema); authz check (which policy engine); policy check (which product rules apply); HITL trigger (if applicable).
5. **HITL review** — side-effect category; proposed HITL rule; reviewer role and SLA; what the user sees.
6. **Blast-radius review** — rate limits, cost caps, argument allow-lists, kill-switch posture (does this tool get its own switch or does it share the surface-level switch).
7. **Decision** — one of: approve; approve with conditions (list conditions and their resolution date); defer pending (list what is missing); reject (with specific objections and alternatives suggested).
8. **Sign-off** — manager + `agent-platform-engineer` + security / risk where applicable.

The decision should be substantive. "Approve with condition that rate limit be set to 10/min/session before merge" is a decision; "approve pending a chat with the team" is not.

### Artefact C — EU AI Act Article 14 / 15.C cross-reference (0.5 page, 30 minutes)

A half-page addendum that cross-references the boundary document to the EU AI Act Articles 14 and 15.C deliverables (chapter 08 core concepts 1 and 2). Content:

- **14.A intervention points** — which boundary document entries are intervention points (HITL checkpoints; kill-switches; surface-level disable).
- **14.D oversight monitoring** — which audit-log queries and dashboards demonstrate the oversight mechanism is monitored (HITL queue depth; approval SLA adherence).
- **14.E stop capability** — the kill-switch inventory from the boundary document; the SLA; last dry-run date.
- **15.C cybersecurity measures** — the scope minimisation; the pre-invocation authz; the audit trail; references to the red-team engagement (exercise 02) that covers tool-use attack categories.
- **One-sentence statement** per obligation of what evidence links to the engineering artefact, in a form that the compliance-mapping memo (chapter 08 core concept 4) can paste in.

This artefact exists so that when counsel asks "how do we demonstrate Article 14 compliance on the agentic surfaces," the manager can hand over the cross-reference and the counsel can verify in minutes instead of hours.

### Remaining time (15 minutes)

Run the chapter 07 anti-patterns list against all three artefacts. Fix anything the list surfaces. Common fixes: a tool whose scope is broader than needed; a HITL that is "the system prompt instructs the model not to without confirmation" (wrong altitude); a kill-switch described but not tested; an audit log described but not queryable.

## Requirements

- Boundary document carries ≥ 3 tools, ≥ 1 with HITL required, ≥ 1 with scoped service identity.
- Every tool in the boundary document has all five property columns filled (identity, scope, pre-invocation checks, HITL rule, blast-radius bounds).
- Kill-switches are named (feature-flag or equivalent) and their SLA is given.
- Audit log sink, retention, and query mechanism are named.
- Review memo makes a concrete decision (approve / approve-with-conditions / defer / reject) with specific conditions or objections, not vague agreement.
- Review memo's identity-and-scope review names the alternatives considered and the rationale for the chosen option.
- Review memo's blast-radius section specifies numbers (rate limits, cost caps).
- Article 14 / 15.C cross-reference maps each sub-obligation to a specific boundary-document entry or a specific external artefact.
- All three artefacts carry a review date and a next-review date.
- No HITL rule described as "the system prompt will handle it" — HITL is a transport-layer control.

## Starter guidance

- Start with the tool catalogue table. The rest of the boundary document composes around it. A table with three rows and five filled columns is a working artefact; a table with twelve rows and three filled columns is a draft.
- For the identity column, prefer the narrowest option that still works. If the product has been using "user OAuth token" everywhere, the exercise is to find at least one tool where the scope narrows to a service identity scoped below the user's full privileges.
- For the scope column, write the actual permission strings. "Read customer data" is not a scope; "read:customer:*:tenant/{current_tenant}" or your platform's equivalent is.
- For the pre-invocation checks column, list the four checks (schema / authz / policy / HITL trigger) and mark which apply to each tool. A tool with no checks is a flag that something is under-specified.
- For the HITL rule column, be honest. If the current implementation has no HITL on an action that chapter 07 says warrants it, write the current rule and then add a one-line "proposed change" note; the exercise doubles as a gap-finder.
- For the blast-radius bounds column, specific numbers are better than general promises. "10 req/min/session; $0.05 cost cap/session; kill-switch tool:send_email:enabled" beats "rate-limited, cost-capped, kill-switch available."
- The review memo for a hypothetical tool addition can draw on a real tool the team has been discussing, or a synthetic one. The quality of the exercise comes from the *decision* being substantive, not the tool being real.
- For the Article 14 / 15.C cross-reference, keep it tight. Half a page is enough if the mapping is specific; a sprawling two-pager suggests the mapping is not specific enough.
- Resist the temptation to describe implementation details the platform engineer owns. The boundary document describes the control; it does not describe how OAuth flows are implemented. If you find yourself writing about token refresh rotations, you have crossed into craft the manager defers.
- Cross-reference exercise 01 (guardrails spec section 4) and exercise 02 (red-team brief's attack-surface description section 3) explicitly; a boundary that is isolated from the spec and the brief is a boundary that will drift out of date.

## Acceptance criteria

- Boundary document carries all seven sections, with tool catalogue ≥ 3 rows and all five property columns filled.
- Review memo makes a concrete decision with substantive conditions or objections.
- Review memo signatures include manager, `agent-platform-engineer`, and security / risk where applicable.
- Article 14 / 15.C cross-reference maps each sub-obligation to a specific boundary entry or external artefact.
- All three artefacts are dated, named, and cross-referenced to each other and to exercises 01 and 02.
- No HITL rule described as a prompt-layer control.
- Kill-switch SLA and audit-log retention are specific.

## Stretch goals

- **MCP contract review.** If the surface uses MCP, add a half-page review of the MCP server's authorisation model — which tools the server exposes; which authentication the host client presents; how the server enforces scope; whether the host client can bypass the server's authorisation (it should not). Cite the MCP specification version read.
- **Capability-tier integration.** Add a paragraph on how a capability-tier change in the underlying model (chapter 06 — new RSP / Preparedness / FSF tier) would trigger a re-review of the boundary. Name specific tier changes that would warrant tightening the catalogue (e.g. improved cyber capability → narrow the `run_shell_command` tool, or remove it).
- **Cross-tenant stress test.** Write a half-page description of the cross-tenant test suite the boundary enables — can the agent in tenant A read data in tenant B under any prompt-injection path? The description is the brief for an `agent-platform-engineer` to build; the test is theirs to implement.
- **Service-identity audit.** Produce a one-page inventory of every service identity the agent surface uses — dedicated service account, provisioning mechanism (manual / Terraform / OIDC-federated), credentials storage (secrets manager), rotation cadence, revocation path. The inventory is often the thing the SOC 2 auditor asks for first; preparing it here is strictly cheaper than preparing it under audit pressure.
- **Red-team category pair.** Append a short list of red-team categories (from exercise 02) that specifically target the tool-use boundary: prompt-injection to invoke a dangerous tool, scope-confusion via indirect injection, privilege-escalation via chained tool calls, data-exfiltration via tool output re-feeding, cost-blast-radius via loop. Tie each category to the boundary properties that mitigate it.

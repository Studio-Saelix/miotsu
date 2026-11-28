# Miotsu — Product direction and implementation plan

**Studio Saelix · From direction to delivery.**  
Agent-native engineering planning. Decision baseline: 2026-11-26.

## 1. Product charter
Miotsu is a self-hosted, Docker Compose-first engineering planning and intelligence platform, forked from [It's a Plan](https://github.com/croffasia/itsaplan). It replaces Studio Saelix's paid Linear dependency first, then differentiates through visual dependency planning and native mission delegation. Miotsu plans; Bosun builds; Sencho operates; Hato communicates. Each remains independently usable.

**Approved direction:** Miotsu name and monochrome connected-node mark; keep the upstream architecture; migrate the strategic Navigator out of Bosun; first-class bidirectional Graph-to-Mission/Goal integration. The exact protocol and UI below are proposed implementation contracts subject to AST/API verification.

## 2. Brand and upstream stewardship
- Wordmark: MIOTSU; by Studio Saelix; tagline: From direction to delivery.; descriptor: Agent-native engineering planning.
- Monochrome, five-node branching mark with rounded connections, dark/light variants, restrained Helm-inspired visual language. Preserve accessibility, keyboard navigation and reduced motion.
- Keep upstream Git history and remote; record fork baseline, track upstream changes and security releases, minimize divergence in shared core. Do not mass-rename package IDs, environment variables, database tables, OAuth identifiers or image names without compatibility testing.
- Root AGPL-3.0 license and original copyright (2026 Andrii Poluosmak) remain intact; runner is Apache-2.0 per upstream README. Identify modified files/dates as required, offer corresponding source to remote users, retain notices, audit third-party and Pro-only code/assets; no bypass of upstream commercial licensing. Review license obligations before distribution.
- Retain Bun/Turborepo, Elysia, Next.js/React, shadcn/Tailwind, Drizzle/PostgreSQL, better-auth, RustFS and existing agent infrastructure. No Rust rewrite. Baseline is upstream package version 1.13.0 as observed in fork. Use Community Edition and preserve existing behavior.

## 3. Architecture and ownership
| Domain | Authority |
| --- | --- |
| Projects, issues, requirements, milestones, labels, priorities, strategic graph, readiness and roadmap | Miotsu |
| Approved mission lifecycle, execution DAG, tasks, agent/model selection, worktrees, queue, audit/QA, merge and recovery | Bosun |
| Source, commits, PRs, CI results | GitHub / configured forge |
| Deployment and infrastructure | Sencho |
| Publication | Hato |

Miotsu's existing agent runner remains available for standalone use; a Bosun-delegated engineering issue must not also be executed by Miotsu's runner, automatic assignment/schedule or another competing writer. Preserve human approvals and scopes. No compulsory dependency between applications.

## 4. Delivery sequence (three stages)
### Stage 1 — Independence (P0; shortest safe path)
- [ ] Establish fork branding and legal provenance; baseline build, lint, typecheck, tests and clean deployment. Keep upstream docs accessible.
- [ ] Deploy pinned Community stack with Sencho: PostgreSQL, RustFS, API, worker, bot, web and migration service. Confirm HTTPS, persistent volumes, encrypted secrets, backups and actual restore; avoid exposing database/storage directly. Fork-built image tags must be verified, not assumed from upstream Compose.
- [ ] Inventory/export Linear using read-only API, including stable source UUID/identifier, projects, milestones, issue states, labels, descriptions, parent/dependency edges, comments, documents, history and links. Secure archive outside public Git and test restore. Store credentials only in secret storage.
- [ ] Pilot importer on a representative project, reconcile counts/field mappings and failed file downloads. Upstream imports one team/project at a time and preserves source identifier in description; cross-import parents/relations require a separate reconciliation pass. It does not import milestones, project metadata, updates, documents, initiatives, custom views, estimates, SLA, activity history, completion timestamps or archived state. Links are described as text; verify native PR association. Map milestones explicitly (native model if available, otherwise a minimal new model), preserve dates/status/provenance and archive unsupported data. Check user mapping, state mappings, nesting/label limits and attachment availability. Never claim lossless import until validated.
- [ ] Complete [Bosun SEN-1503](https://linear.app/saelix/issue/SEN-1503/bosun-decouple-linear-through-a-provider-neutral-issue-tracker): provider-neutral Rust boundary, Miotsu-compatible adapter, existing Linear/GitHub/none compatibility, one journaled tracker writer, provider identity pinned per run. Reuse existing contracts.
- [ ] Test MCP/REST clients and ChatGPT integration separately; record missing capabilities and fallback for automations. Keep Linear read-only archive/export as needed, cut over project by project after draining or explicitly migrating active runs. No uncontrolled dual writes. Cancel paid subscription after securing data, verifying renewal date and confirming essential workflow continuity.

Exit: migrated issues, critical milestone/graph integrity and one complete Bosun run with no Linear credentials, plus tested backup/rollback and an explicit record of remaining gaps. Do not wait for cosmetic parity to stop paying.

### Stage 2 — Studio Saelix identity and mission exchange (P1)
- [ ] Audit Bosun Navigator (SEN-1195 through related slices) for reusable frontend graph components and strategic semantics, license compatibility, dependency contracts and tests. Use React Flow/Dagre if validated against upstream stack; port design, not unrelated runtime.
- [ ] Add Miotsu Navigator: feature tree, dependency graph, project/milestone scopes, search/filter, typed edges (hard prerequisites vs parent/related/duplicate), missing data, cycle detection, readiness explanations and graph revision. Support large graphs via lazy expansion/virtualization and measured performance.
- [ ] Select a graph, epic, milestone, initiative, or issue; preview dependency closure and external blockers; Send to Bosun as a draft mission/goal. Also pull from Bosun and support scoped MCP delegation.
- [ ] Version the Mission Manifest and capability negotiation; validate stable issue IDs, repository mappings, revision, authorization and size limits. Reuse Bosun Mission/Queue entrypoints; never create a second scheduler. Show an editable, reviewable proposal and require configured approval before executing.
- [ ] Wire signed, replay-safe progress events and authoritative reconciliation: issue/run links, accepted/blocked/running/verified/merged/failed states, PRs, QA evidence, cost/duration and discovered work proposals. Reconcile stale plans without expanding scope silently.
- [ ] Verify parity and export/backup existing Bosun saved Navigator plans before hiding and then retiring the strategic Navigator UI and migrated planning logic. Keep execution graph, dependency enforcement, Queue, Missions and agent intelligence in Bosun. Preserve generic tracker/no-tracker operation and offer a reversible transition.

Exit: end-to-end graph-to-mission across multiple issues (including safe parallel waves and external blockers), restart/replay and change-of-plan tests, no duplicate execution, validated Navigator parity and an explicit retirement decision.

### Stage 3 — Differentiation (P2)
- [ ] Critical-path and unlock/impact analysis, cross-project dependencies, release readiness, feature/architecture maps and engineering intelligence.
- [ ] Aggregate Bosun execution evidence, cost, duration, QA findings and delivery forecasts with uncertainty and provenance.
- [ ] Build native MCP tooling and a ChatGPT plugin/app as supported by current account/platform, test mobile, scheduled automation and distribution separately. Do not promise Linear parity. Optional links to Sencho deploy and Hato release communication.
- [ ] Upstream contribution policy, release artifacts, security reporting, upgrade/rollback and performance budgets.

## 5. Mission Exchange v1 — proposed contract
A versioned manifest describes the objective, not the execution strategy:
- schema version, stable manifest ID and idempotency key, source instance/project IDs, revision/hash, objective and completion criteria;
- selected issues with opaque stable IDs, display IDs, URLs, requirement snapshots/acceptance criteria, status and provenance; repository/project mapping; typed edges including external blockers;
- explicit scope, constraints, priorities, optional budget/time limits, requested approval/autonomy policy (cannot override Bosun policy), callback/subscription reference (never embed secrets).
Bosun validates the manifest, rejects hard dependency cycles or missing required mappings, deduplicates imports, proposes an execution plan and reports acceptance with mission ID. Bosun determines decomposition, safe concurrency and engine/model/effort routing. Parallel independent issues only after dependency/PR/QA gates permit; conflicting files/worktrees or shared resources can force serialization.
Miotsu owns desired scope and graph; Bosun pins accepted revision, owns execution state and emits evidence. On changes, compare revisions, report affected work, preserve completed work and require reapproval for material expansion. No automatic execution on import unless separately authorized. Use scoped service identities, least privilege, authenticated TLS, signed webhooks with timestamp/nonce, bounded retries, idempotent events, audit trail, revocation and polling fallback. Verify exact endpoints with both repositories; avoid new public API surface if existing scoped Bosun contracts suffice.

## 6. Acceptance and non-goals
- Preserve license, upstream sync, security and data ownership. New custom features are additive and testable; no rewrite or commercial-feature circumvention.
- Two connected applications demonstrate push/pull, graph validation, approval, parallel execution, independent audit/QA, merge evidence, callback reconciliation, restart and rollback; standalone modes remain functional.
- No strategic Navigator deletion until migration parity; no duplicate orchestration, hidden paid dependency, speculative architecture rewrite or guarantee of ChatGPT/mobile/automation support.
- Tests: upstream baseline plus import fixtures, counts/edge reconciliation, auth/scope negative cases, duplicate/out-of-order events, crash recovery, stale revision, cycle, missing mapping, partial failures, UI accessibility and large-graph performance.
- Open decisions: final domain/trademark clearance, exact upstream fork SHA and CI/image publication strategy, native milestone representation, actual API mapping and ChatGPT plugin eligibility. Confirm these with code and runtime evidence.
- Related: [Linear importer](https://github.com/croffasia/itsaplan/blob/main/docs/importing-from-linear.md), [self-hosting](https://github.com/croffasia/itsaplan/blob/main/docs/self-hosting.md), [Bosun](https://github.com/Studio-Saelix/bosun), [Miotsu](https://github.com/Studio-Saelix/miotsu).

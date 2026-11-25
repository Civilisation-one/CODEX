# CIV1-HARDENING-001
## Persistence, Operational Readiness & Production Integration

**Document ID:** CV1-PLATFORM-CODEX-002  
**Spec ID:** CIV1-HARDENING-001  
**Organisation:** Civilisation.One  
**Canonical text:** `Civilisation-one/CODEX`; **implemented in:** `Civilisation-one/civ-intelligence-layer`  
**Specification type:** Engineering implementation and acceptance specification  
**Priority:** Critical platform hardening  
**Document status:** PROPOSED — requires developer review and approval  
**Related:** [CV1-PLATFORM-CODEX-003](CV1-PLATFORM-CODEX-003.md), [CV1-PLATFORM-CODEX-001](CV1-PLATFORM-CODEX-001.md) (read 003 → 001 → 002; see [README](README.md))  
**Version:** 1.0  
**Date:** 18 November 2026

---

# 1. Executive Objective

CIV1-HARDENING-001 establishes the reliability and operational foundations required to advance the Civilisation.One Intelligence Layer towards integrated Alpha.

The existing architecture is retained.

The milestone addresses three weaknesses:

| Engineering area | Previous assessment | Target |
|---|---:|---:|
| Persistence | 5/10 | 8.5/10 |
| Operational readiness | 4.5/10 | 8/10 |
| Production integration | 5/10 | 8/10 |

These targets are provisional engineering objectives. They are not achieved scores and cannot be awarded without passing the acceptance criteria in this document.

## 1.1 Core principles

1. VIREÁX coordinates intelligence.
2. Member nodes retain sovereignty.
3. Evidence establishes confidence.
4. AI output is not automatically a validated claim.
5. A validated claim is not automatically an authorised action.
6. All critical state transitions must be auditable.
7. Security failures must fail closed.
8. Recovery must not silently alter the history of an operation.
9. Independent verification must remain possible.
10. Human authority governs merges, releases and deployments.

# 2. Scope

## Included

- Durable database persistence.
- Transactional audit records.
- Model execution receipts.
- Authority and approval persistence.
- Persistent task and job state.
- Federation replay protection.
- Backup and recovery.
- Unified logging and tracing.
- Service health and metrics.
- Alerting and incident handling.
- Integration contracts.
- Three-node federation simulation.
- End-to-end acceptance tests.
- Failure injection and security testing.

## Excluded

- Platform redesign.
- New AI reasoning capabilities.
- Changes to member sovereignty principles.
- Autonomous deployment.
- Automatic merging.
- Production federation activation without a separate security approval.
- Enabling RAG retrieval or citation functions before their required security acceptance.

# 3. Target Architecture

```text
                 MEMBER / MIRRORME
                         |
                         v
                  VIREÁX ORCHESTRATOR
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
      MKONE ROUTER    REASONING       RAG SERVICE
          |          PIPELINE            |
          |         Mirror/Critic         |
          |         Evidence              |
          |         Synthesis              |
          |              |               |
          +--------------+---------------+
                         |
                         v
                 AUTHORITY / POLICY
                         |
                         v
                   MODULE GATEWAY
                         |
                         v
                CIVILISATION MODULES

       SHARED INFRASTRUCTURE
       +----------------------------------+
       | PostgreSQL                       |
       | Persistent Audit Ledger          |
       | Model Execution Receipts         |
       | Task / Job State                 |
       | Authority / Approval Records     |
       | Federation Replay State          |
       | RAG Lifecycle                    |
       | Metrics / Logs / Traces          |
       +----------------------------------+
```

The architecture must preserve the existing separation between orchestration, routing, reasoning, evidence, authority and module execution.

No module may bypass the designated model router or authority gateway.

# 4. Work Package H1 — Durable Persistence

**Priority:** P0  
**Objective:** Critical state survives process failure, restart, concurrency and recovery.

## H1.1 Database foundation

Use PostgreSQL as the durable persistence layer.

Existing RAG-001 architecture uses PostgreSQL with pgvector. This milestone should reuse the established database infrastructure rather than introduce a competing database.

Requirements:

- Versioned database migrations.
- Transactional writes.
- Foreign-key and uniqueness constraints.
- Explicit transaction boundaries.
- Connection pooling.
- Defined retention policies.
- Backup and restore procedures.
- Migration verification.
- Database access separated by service role.

Do not place secrets or unrestricted credentials in repository configuration.

## H1.2 Persistent records

The implementation must support the following logical records:

| Record | Required information |
|---|---|
| AuditEvent | Event ID, sequence, timestamp, actor, event type, payload hash, previous hash |
| ModelReceipt | Provider, model, request ID, policy version, status, usage, latency |
| AuthorityDecision | Actor, action, resource, scope, decision, expiry, policy version |
| TaskState | Task ID, state, owner, idempotency key, timestamps |
| JobState | Job ID, queue state, attempt count, recovery metadata |
| FederationNonce | Node identity, nonce or sequence, expiry, consumption state |
| MemoryTransaction | Proposal, preview, approval, commit and audit references |
| DocumentLifecycle | Document version, owner, node, visibility, validity and supersession |

These are logical requirements, not instructions to create duplicate tables where equivalent existing structures already exist.

Developers must inspect the current schema before implementation.

## H1.3 Audit integrity

Implement an append-only audit model.

Each event must include:

- Unique event ID.
- Monotonic sequence within its defined ledger.
- Timestamp.
- Authenticated actor identity.
- Originating node.
- Event type.
- Event payload or canonical payload hash.
- Previous event hash.
- Current event hash.
- Schema version.
- Correlation or trace ID.

Conceptual hash construction:

```text
event_hash =
SHA256(
    canonical_event_payload
    || previous_event_hash
)
```

Requirements:

1. Canonical serialization must be deterministic.
2. Application service roles must not update or delete committed audit events.
3. Audit writes must be transactionally connected to the state transition they record.
4. Independent checkpoints must be produced periodically.
5. Checkpoint verification must detect alteration or truncation.
6. Recovery must verify the audit chain before accepting the recovered state.
7. Audit failure must block protected state-changing actions.

A hash chain alone is not immutable. An administrator with unrestricted write access may rewrite and rehash the chain. Independent checkpoints and separation of privileges are mandatory for stronger tamper detection.

## H1.4 Idempotency

Every state-changing operation must support an idempotency key.

Expected behaviour:

```text
First request:
    execute operation
    persist result
    return result

Repeated request:
    detect existing idempotency key
    return original result
    do not execute again
```

Requirements:

- Unique idempotency constraints.
- Defined key lifetime.
- Operation scope.
- Request payload fingerprint.
- Conflict handling if the same key is reused with different input.
- Concurrent request tests.

## H1.5 Crash recovery

The system must recover safely from:

- Process termination.
- Database connection loss.
- Interrupted transaction.
- Machine restart.
- Worker termination.
- Network interruption.
- Partial job execution.

An operation whose execution status is uncertain must not be automatically repeated if repetition could cause an irreversible external action.

Use explicit states:

```text
PENDING
RUNNING
COMPLETED
FAILED
CANCELLED
RECOVERY_REQUIRED
```

The implementation may add states where justified.

## H1.6 H1 acceptance criteria

- [ ] Committed critical state survives restart.
- [ ] Interrupted transaction leaves no partial committed state.
- [ ] Duplicate requests do not duplicate actions.
- [ ] Concurrent operations preserve consistency.
- [ ] Audit alteration is detectable.
- [ ] Audit truncation is detectable against independent checkpoints.
- [ ] Backup restoration preserves critical history.
- [ ] Federation replay state survives restart.
- [ ] Uncertain operations enter recovery handling.
- [ ] Database migrations can be verified against a clean installation.

# 5. Work Package H2 — Operational Readiness

**Priority:** P0/P1  
**Objective:** The system can be observed, diagnosed, operated and recovered.

## H2.1 Unified observability

All major components must emit structured logs and share trace context.

Required fields where applicable:

```json
{
  "trace_id": "string",
  "request_id": "string",
  "task_id": "string",
  "node_id": "string",
  "actor_id": "authenticated-principal-reference",
  "component": "string",
  "policy_version": "string",
  "status": "string",
  "error_code": "string",
  "timestamp": "ISO-8601"
}
```

Do not place passwords, API keys, access tokens or private member content in logs.

Sensitive identifiers should be protected through appropriate access controls and redaction.

## H2.2 Trace continuity

Trace propagation must cover:

```text
MirrorME
  -> VIREÁX
  -> MKONE
  -> Model Provider
  -> Mirror
  -> Critic
  -> Evidence
  -> Synthesis
  -> Authority Gate
  -> Module Gateway
  -> Audit
```

A component must preserve incoming trace identity and create linked child spans where necessary.

Every model execution must have a verifiable receipt.

Every protected module action must have an authority decision.

## H2.3 Health endpoints

Provide service health information using clearly differentiated states:

- HEALTHY
- DEGRADED
- UNAVAILABLE

Health checks must distinguish:

1. Process availability.
2. Dependency availability.
3. Readiness to accept new work.
4. Database connectivity.
5. Queue availability.
6. Model provider availability.

A running process must not automatically be considered ready.

## H2.4 Metrics

Required metrics include:

| Metric | Purpose |
|---|---|
| Request count | Throughput |
| Request latency | Performance |
| Error rate | Reliability |
| Timeout rate | Dependency health |
| Queue depth | Backpressure |
| Database connections | Capacity |
| Database transaction failures | Persistence health |
| Model provider failures | Routing health |
| Policy denials | Security visibility |
| Audit write failures | Integrity monitoring |
| Recovery-required tasks | Operational risk |
| Storage growth | Capacity planning |

Metrics must not expose private member data.

## H2.5 Failure handling

| Failure | Required behaviour |
|---|---|
| Provider unavailable | Controlled fallback or fail closed |
| Database unavailable | Block protected state changes |
| RAG unavailable | Report unavailable; do not fabricate evidence |
| Timeout | Preserve uncertain execution state |
| Invalid identity | Deny |
| Policy conflict | Deny and record |
| Queue overload | Apply backpressure |
| Node disconnected | Isolate and recover safely |
| Audit failure | Block protected actions |
| Invalid response | Reject or quarantine |

Fallback routing must obey the same data classification and authorization rules as the original route.

A fallback must never silently move private data to an external provider.

## H2.6 Operational documentation

Create:

- Service runbooks.
- Database recovery runbook.
- Audit verification runbook.
- Provider outage runbook.
- Security incident runbook.
- Federation isolation runbook.
- Deployment checklist.
- Backup verification procedure.
- Incident severity classification.

## H2.7 H2 acceptance criteria

- [ ] Every major service exposes health information.
- [ ] Trace IDs survive the full execution path.
- [ ] Provider calls are traceable to receipts.
- [ ] Critical failures produce structured error events.
- [ ] Alerts identify actionable failures.
- [ ] Sensitive values are excluded from logs.
- [ ] Provider outage simulation passes.
- [ ] Database outage simulation passes.
- [ ] Queue overload is controlled.
- [ ] Recovery procedures are documented and tested.

# 6. Work Package H3 — Production Integration

**Priority:** P1  
**Objective:** Prove the platform operates correctly across component boundaries.

## H3.1 Interface contract verification

The following boundaries must have versioned contracts:

| Boundary | Contract requirements |
|---|---|
| MirrorME → VIREÁX | Authenticated identity, request context, trace |
| VIREÁX → MKONE | Routing policy, classification, budget |
| MKONE → Provider | Authorized route, timeout, receipt |
| Provider → MKONE | Response provenance, status, usage |
| VIREÁX → Critic | Independent review input |
| Critic → Evidence | Claims, objections, requirements |
| Evidence → Synthesis | Verification status and uncertainty |
| VIREÁX → Module Gateway | Action request and authority context |
| Gateway → Module | Capability scope and permission |
| VIREÁX → RAG | Identity and retrieval permissions |
| RAG → VIREÁX | Authorized results and provenance |
| All components → Audit | Durable trace and event references |

Contracts must be tested for:

- Valid inputs.
- Invalid inputs.
- Missing fields.
- Unknown fields.
- Unsupported versions.
- Malformed identities.
- Unauthorized operations.
- Expired authority.
- Policy mismatch.
- Duplicate messages.

## H3.2 Three-node simulation

Create a deterministic simulation with three logical nodes.

**Node A — Origin**

- Initiates a request.
- Submits knowledge.
- Maintains its own state and authority.

**Node B — Collaborator**

- Receives a federated request.
- Provides evidence.
- May challenge a claim.
- Maintains independent state.

**Node C — Independent Verifier**

- Verifies signatures.
- Checks node identity.
- Checks authority.
- Detects replay.
- Evaluates evidence provenance.
- Reports contradictions.

The simulation must not grant a node authority over another node's private state.

## H3.3 Adversarial scenarios

Test:

1. Conflicting evidence.
2. Revoked node credentials.
3. Expired signatures.
4. Replay attacks.
5. Network partitions.
6. Malformed payloads.
7. Duplicate messages.
8. Conflicting document revisions.
9. Node restart during execution.
10. Permission revocation during an active workflow.
11. Invalid policy version.
12. Unauthorized external data transfer.

## H3.4 End-to-end test

The acceptance test must execute:

```text
1. Member submits request.
2. Transport identity is authenticated.
3. VIREÁX establishes task and policy.
4. MKONE selects an eligible model route.
5. Model execution produces a receipt.
6. Mirror/Critic/Evidence stages execute.
7. RAG returns authorized results with provenance.
8. Synthesis separates evidence from hypotheses.
9. Authority gate evaluates requested action.
10. Module Gateway authorizes or denies.
11. Audit transaction commits.
12. Trace can be independently inspected.
```

The test must include both successful and denied actions.

## H3.5 H3 acceptance criteria

- [ ] All integration contracts pass.
- [ ] Three-node simulation passes.
- [ ] Replay attack is rejected.
- [ ] Revoked credentials are rejected.
- [ ] Network partition recovery passes.
- [ ] Permission revocation is enforced.
- [ ] Contradictory evidence remains distinguishable.
- [ ] Missing evidence cannot become an accepted claim.
- [ ] Model agreement cannot authorize an action.
- [ ] Complete end-to-end trace is available.
- [ ] Audit history survives restart.

# 7. Work Package H4 — Security and Trust Boundaries

**Priority:** P0  
**Objective:** Ensure that persistence and integration do not create new authorization vulnerabilities.

## H4.1 Identity

Authenticated identity must originate from trusted transport or authentication infrastructure.

A caller-supplied `actor_id` is not proof of identity.

Requirements:

- Server-side principal resolution.
- Scoped service credentials.
- Member and node isolation.
- Credential expiry.
- Rotation and revocation.
- Authorization checks at the point of action.
- Explicit separation between user, service and model identities.

## H4.2 Authority

A model cannot authorize its own action.

A model cannot create human approval by generating text that claims approval exists.

An approval must be independently verifiable and bound to:

- Authenticated approver.
- Exact action.
- Exact resource.
- Scope.
- Purpose.
- Expiry.
- Policy version.
- Approval identifier.

`AUTHORIZED` means permission has been granted. It does not mean the action has already executed.

## H4.3 Data classification

Data classification must be enforced at the routing boundary.

Requirements:

- Conservative classification.
- No automatic sensitivity downgrade.
- Policy-controlled external egress.
- Redaction where necessary.
- Local-only restrictions cannot be overridden by fallback.
- Record provider selection and disclosure decisions.
- Do not treat caller-declared classification as proof of content sensitivity.

## H4.4 RAG

RAG retrieval and citation operations remain disabled until the H4.4/H4.5 RAG security acceptance is independently demonstrated.

Required tests:

- Tenant isolation.
- Node isolation.
- Owner isolation.
- Visibility enforcement.
- Permission revocation.
- Supersession filtering.
- Lifecycle enforcement.
- Citation provenance.
- Unauthorized retrieval returns zero results.

## H4.5 Security acceptance criteria

- [ ] Changing caller `actor_id` cannot impersonate another member.
- [ ] Unauthorized external egress is blocked.
- [ ] Expired approval is rejected.
- [ ] Scope mismatch is rejected.
- [ ] Policy mismatch fails closed.
- [ ] Revoked permissions cannot retrieve protected data.
- [ ] Federation replay fails across restarts.
- [ ] Model output cannot manufacture approval.
- [ ] Audit history is independently verifiable.

# 8. Proposed Repository Structure

The following is a target logical structure. Developers must compare it with the actual repository and reuse existing packages rather than create duplicate implementations.

> **Illustrative layout, not a file list.** `civ-intelligence-layer` already has a `contracts/src/` tree (for example `audit.ts`, `types.ts`, `rag.ts`, `gate.ts`, `store.ts`). Inspect that tree first; extend existing contracts rather than creating parallel files with the same responsibilities.

```text
civ-intelligence-layer/
│
├── contracts/
│   ├── src/
│   │   ├── audit.ts
│   │   ├── persistence.ts
│   │   ├── receipts.ts
│   │   ├── authority.ts
│   │   ├── operations.ts
│   │   └── integration.ts
│   └── test/
│
├── persistence/
│   ├── src/
│   │   ├── database.ts
│   │   ├── transactions.ts
│   │   ├── audit-ledger.ts
│   │   ├── receipts.ts
│   │   ├── task-store.ts
│   │   ├── recovery.ts
│   │   └── idempotency.ts
│   ├── migrations/
│   └── test/
│
├── observability/
│   ├── src/
│   │   ├── logging.ts
│   │   ├── metrics.ts
│   │   ├── tracing.ts
│   │   ├── health.ts
│   │   └── alerts.ts
│   └── test/
│
├── integration/
│   ├── contracts/
│   ├── end-to-end/
│   ├── federation/
│   ├── failure-injection/
│   └── fixtures/
│
├── security/
│   ├── identity/
│   ├── authority/
│   ├── egress/
│   └── acceptance/
│
├── docs/
│   ├── CIV1-HARDENING-001.md
│   ├── PERSISTENCE-RUNBOOK.md
│   ├── INCIDENT-RESPONSE.md
│   ├── DATABASE-RECOVERY.md
│   ├── AUDIT-VERIFICATION.md
│   └── INTEGRATION-ACCEPTANCE.md
│
└── .github/
    └── workflows/
```

# 9. Development Tickets

## H1 — Persistence

| Ticket | Task | Priority |
|---|---|---|
| H1-001 | Inspect existing persistence and schema | P0 |
| H1-002 | Define persistence contracts | P0 |
| H1-003 | Implement transactional database access | P0 |
| H1-004 | Implement durable audit ledger | P0 |
| H1-005 | Implement model execution receipts | P0 |
| H1-006 | Persist authority decisions | P0 |
| H1-007 | Implement idempotency controls | P0 |
| H1-008 | Implement crash recovery | P0 |
| H1-009 | Implement backup/restore validation | P1 |
| H1-010 | Implement independent audit checkpoints | P1 |

## H2 — Operations

| Ticket | Task | Priority |
|---|---|---|
| H2-001 | Define observability contracts | P1 |
| H2-002 | Implement structured logging | P1 |
| H2-003 | Implement trace propagation | P1 |
| H2-004 | Implement health and readiness endpoints | P1 |
| H2-005 | Implement operational metrics | P1 |
| H2-006 | Define alerting rules | P1 |
| H2-007 | Implement failure handling | P1 |
| H2-008 | Write operational runbooks | P1 |
| H2-009 | Test recovery procedures | P1 |

## H3 — Integration

| Ticket | Task | Priority |
|---|---|---|
| H3-001 | Audit existing integration contracts | P1 |
| H3-002 | Add contract regression tests | P1 |
| H3-003 | Build three-node simulator | P1 |
| H3-004 | Add adversarial federation scenarios | P1 |
| H3-005 | Implement end-to-end trace test | P1 |
| H3-006 | Add failure injection | P1 |
| H3-007 | Validate complete audit continuity | P1 |
| H3-008 | Produce integration acceptance report | P1 |

## H4 — Security

| Ticket | Task | Priority |
|---|---|---|
| H4-001 | Verify authenticated principal resolution | P0 |
| H4-002 | Test member/node authorization boundaries | P0 |
| H4-003 | Validate data classification and egress | P0 |
| H4-004 | Validate approval scope and expiry | P0 |
| H4-005 | Test RAG isolation and revocation | P0 |
| H4-006 | Test federation replay protection | P0 |
| H4-007 | Independent security acceptance review | P0 |

# 10. Sequential CODEX Agent Instructions

Agents must work sequentially, with a human review checkpoint between work packages.

## Global agent rules

1. Inspect the repository before changing files.
2. Read existing CODEX and repository instructions.
3. Produce a plan before implementation.
4. Identify affected packages and dependencies.
5. Do not duplicate existing functionality.
6. Preserve existing contracts unless a documented change is approved.
7. Add tests for every behaviour change.
8. Never claim a test passed unless it was actually executed.
9. Never claim a migration was applied unless verified.
10. Never commit secrets.
11. Never deploy.
12. Never merge.
13. Never enable auto-merge.
14. Create draft pull requests only.
15. Preserve human approval authority.
16. Report incomplete work and known limitations honestly.

## Agent 1 — Repository Auditor

**Assignment:** H1-001, H2-001, H3-001.

Deliver:

- Current package map.
- Existing database and persistence inventory.
- Existing audit and receipt inventory.
- Current logging and tracing inventory.
- Integration dependency map.
- Security gaps.
- Proposed implementation plan.
- Ticket-to-file mapping.

No implementation changes.

## Agent 2 — Persistence Engineer

**Assignment:** H1-002 through H1-010.

Deliver:

- Versioned persistence contracts.
- Database migrations.
- Transactional storage.
- Audit ledger.
- Receipt persistence.
- Idempotency implementation.
- Recovery tests.
- Backup/restore tests.

Do not change authorization policy without an approved contract change.

## Agent 3 — Operations Engineer

**Assignment:** H2-002 through H2-009.

Deliver:

- Structured logging.
- Trace propagation.
- Health endpoints.
- Metrics.
- Alert definitions.
- Failure handling.
- Runbooks.
- Operational tests.

Do not expose private data in logs or metrics.

## Agent 4 — Integration Engineer

**Assignment:** H3-002 through H3-008.

Deliver:

- Contract tests.
- Three-node simulator.
- Failure injection.
- End-to-end tests.
- Integration report.

Do not activate real federation or production provider access without separate approval.

## Agent 5 — Independent Security Reviewer

**Assignment:** H4-001 through H4-007.

This agent must review the implementation independently, not simply approve the implementation agent's claims.

Deliver:

- Threat and boundary assessment.
- Impersonation test results.
- Authorization test results.
- Egress test results.
- Replay test results.
- RAG isolation test results.
- Critical and high-severity findings.
- Final security acceptance recommendation.

## Human acceptance gate

After each agent completes its work:

1. Inspect the proposed diff.
2. Verify tests and evidence.
3. Review security impact.
4. Approve or reject the work package.
5. Authorize the next agent.

No agent may authorize its own production release.

# 11. Release Acceptance Gate

CIV1-HARDENING-001 is not complete merely because the code compiles.

All mandatory acceptance criteria must pass.

| Gate | Required evidence |
|---|---|
| Build | Successful reproducible build |
| Unit tests | Relevant test suite passes |
| Integration | Cross-component tests pass |
| Persistence | Restart, crash and restore tests pass |
| Audit | Independent integrity verification passes |
| Security | Identity and authorization tests pass |
| RAG | Isolation and revocation acceptance passes |
| Federation | Replay and partition tests pass |
| Operations | Health, metrics and alert tests pass |
| Documentation | Runbooks and architecture updated |
| Review | Independent review completed |
| Approval | Human release authorization recorded |

A failed P0 requirement blocks release.

A failed P1 requirement must be documented, risk-assessed and explicitly accepted before proceeding.

# 12. Final Definition of Done

The milestone is complete when:

- Critical state is durable.
- Audit history is verifiable.
- Model calls have persistent receipts.
- Authority decisions survive restarts.
- Duplicate actions are prevented.
- Recovery is tested.
- Services are observable.
- Failures are handled safely.
- Integration contracts are verified.
- Three-node simulation passes.
- End-to-end traces are complete.
- Security boundaries are tested.
- RAG access control is accepted before activation.
- Human authority over merge and deployment remains intact.

# 13. Engineering Decision

**CIV1-HARDENING-001 should become the next critical milestone for `civ-intelligence-layer`.**

The architecture should not be redesigned to address these weaknesses.

The engineering priority is to prove that the existing architecture can:

1. Preserve state.
2. Preserve provenance.
3. Preserve authority boundaries.
4. Recover from failure.
5. Operate under observation.
6. Integrate across components.
7. Resist malicious or malformed inputs.
8. Produce independently verifiable outcomes.

The governing platform principle remains:

**VIREÁX coordinates intelligence. Member nodes retain sovereignty. Evidence establishes confidence.**

**End of specification.**

---

## Document control

- **Document ID:** CV1-PLATFORM-CODEX-002 (spec ID CIV1-HARDENING-001)
- **Version:** 1.0
- **Document status:** PROPOSED
- **Owner:** Civilisation.One platform governance / authorised maintainers
- **Related:** CV1-PLATFORM-CODEX-003 (purpose), CV1-PLATFORM-CODEX-001 (invariants and evidence rules)
- **Review trigger:** material change to persistence, operations, integration or security requirements
- **Change policy:** revise by reviewed, traceable pull request; retain prior versions in repository history
- **Status note:** this specification states requirements and acceptance criteria. It is not evidence that any of them are implemented.

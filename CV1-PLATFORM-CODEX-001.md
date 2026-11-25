# CIVILISATION.ONE — PLATFORM CODEX

**Document ID:** CV1-PLATFORM-CODEX-001  
**Version:** 1.0  
**Date:** 14 November 2026  
**Document status:** PROPOSED (previously labelled “Foundational team reference”; moves to ACCEPTED only by maintainer decision)  
**Related:** [CV1-PLATFORM-CODEX-003](CV1-PLATFORM-CODEX-003.md), [CV1-PLATFORM-CODEX-002](CV1-PLATFORM-CODEX-002.md) (read 003 → 001 → 002; see [README](README.md))  
**Platform:** Civilisation.One / OUUM  
**Repository:** `Civilisation-one/CODEX`

> **VIREÁX coordinates intelligence. Member nodes retain sovereignty. Evidence establishes confidence. CivScore makes progress visible.**

---

## 1. Purpose and authority of this document

This Codex defines the architectural principles, system boundaries, governance expectations, evidence standards and engineering practices for Civilisation.One.

It is a platform-wide reference for engineers, researchers, administrators, reviewers, contributors and AI agents working within the project. It is intended to guide specifications, implementation decisions, code reviews, testing and operational procedures.

This document is an architectural and governance charter. It is not proof that every described component is implemented, deployed, secure or experimentally validated. Implementation status must be reported separately and supported by repository evidence, tests and operational records.

Where a repository-level specification conflicts with this Codex, contributors must identify the conflict and resolve it through an explicit, reviewed decision. They must not silently reinterpret either document.

## 2. Platform identity

Civilisation.One (OUUM) is a hybrid, distributed intelligence and knowledge-coordination platform. It connects people, independent AI systems, research, projects and communities through member-controlled nodes and shared coordination infrastructure.

The platform's purpose is to make distributed intelligence capable of working together while preserving individual and institutional autonomy, traceable evidence, accountable decisions and transparent progress.

Civilisation.One is not merely a social network, a cryptocurrency project, a centralised AI assistant or a single-agent system. Its architecture must support collaboration without making the central coordination layer the owner of members, their data, or the truth.

### Foundational principle

1. VIREÁX coordinates intelligence.
2. Member nodes retain sovereignty.
3. Evidence establishes confidence.
4. Governance authorises consequential actions.
5. CivScore makes measured progress visible.

No agent owns truth. No central node owns its members. No score replaces evidence. No recommendation silently becomes an authorised action.

## 3. Architectural invariants

The following invariants apply across the platform.

### 3.1 Member sovereignty

- A member node controls its own identity-bound data, private memory and permissions.
- Access to member data is purpose-bound, permission-checked and auditable.
- Global coordination does not imply unrestricted access to local data.
- Members must be able to understand, grant, limit and revoke permissions, subject to clearly disclosed legal and operational constraints.
- Data sharing must preserve source identity, scope and provenance.

### 3.2 Separation of intelligence, evidence and authority

The platform must distinguish:

- **Intelligence:** generated analysis, proposals, predictions and recommendations.
- **Evidence:** source material and observations that support or challenge claims.
- **Validation:** a documented assessment of evidence quality and claim status.
- **Authority:** permission to approve or execute a defined action.
- **Execution:** the actual operation and its recorded outcome.

An AI-generated answer is not automatically a validated claim. A validated claim is not automatically an authorised action.

### 3.3 Evidence before confidence

Confidence must be traceable to evidence, methods, assumptions and uncertainty. Popularity, repetition, model agreement or seniority must not independently establish factual truth.

### 3.4 Independent criticism

Material claims and consequential proposals must be open to structured challenge. Criticism must be capable of identifying missing evidence, contradictions, invalid inference, uncertainty and alternative explanations.

### 3.5 Auditability

Important state changes, approvals, permission changes, publication events and consequential actions must leave attributable audit records. Audit records should be append-only or tamper-evident, with correction handled through superseding events rather than silent history rewriting.

### 3.6 Reversibility and bounded action

Where practical, actions should be reversible, scoped and limited in duration and effect. Irreversible or high-impact actions require stronger safeguards and explicit human or delegated authority.

### 3.7 No throne

No component, agent, council member, administrator or score has unlimited authority. Authority must be explicit, scoped, reviewable and constrained by policy. Central coordination is not sovereignty over participants.

## 4. Global architecture

Civilisation.One is a distributed platform with shared coordination services and member-controlled nodes.

The following are logical architectural domains. They are not a claim that every domain is complete or deployed.

### 4.1 Global Hub

The Global Hub provides the shared platform entry point for identity, navigation, collaboration, project coordination and access to platform services. It must not bypass the permission boundaries of member nodes.

### 4.2 VIREÁX coordination layer

VIREÁX is the platform's intelligence coordination and orchestration layer. It routes work, coordinates specialist reasoning, tracks provenance and exposes uncertainty and disagreement.

VIREÁX is not an oracle, an owner of member data, an independent source of truth or an unrestricted execution authority.

A typical reasoning workflow may use the following stages:

1. **Mirror:** faithfully represent the input, context and relevant claims.
2. **Critic:** challenge assumptions, logic, omissions and contradictions.
3. **Evidence:** retrieve and assess supporting and opposing evidence.
4. **Synthesis:** produce a conclusion that preserves uncertainty, provenance and unresolved disagreement.

These stages are a logical workflow. Implementations may distribute them across services or agents, but must preserve stage identity and traceability. A stage must not be reported as having run when it did not.

### 4.3 Coordination boundary

VIREÁX may coordinate, recommend, evaluate and report within its granted permissions. It may not silently:

- change a member's private records;
- expand its own permissions;
- publish a member's work;
- alter CivScore measurements or outcomes;
- override an authorised governance decision;
- execute an external action without the required authority.

All external actions must pass through an explicit policy and authorisation boundary.

## 5. Member nodes and MirrorME

A member node is the member-controlled environment for identity-bound information, personal knowledge, goals, preferences and permitted interactions with platform services.

### 5.1 MirrorME

MirrorME is the member intelligence-node concept and implementation track. Its intended capabilities include:

- member-controlled profile and identity context;
- private memory and knowledge records;
- goals and project context;
- session and conversation continuity;
- model routing under explicit policy;
- permission-aware interaction with global services;
- consent, revocation and audit history.

These capabilities must be assessed individually. A prototype or interface is not evidence of a complete, secure member node.

### 5.2 Hybrid reasoning

The platform direction is hybrid: local and external reasoning may be used under an explicit routing policy.

The routing policy must identify:
- which data may leave the member node;
- which model or provider may receive it;
- the purpose and scope of the request;
- whether data is retained by the provider;
- what provenance and response metadata are recorded;
- what happens when a provider is unavailable or policy denies the route.

Private information storage remains under local/member-controlled policy. External reasoning must not be treated as permission to export the underlying private memory.

### 5.3 Memory transaction

Persistent memory changes should follow a controlled lifecycle:

**Propose → Preview → Approve → Commit → Audit**

The system must distinguish temporary context from durable memory. A model's suggestion to remember something is not itself member approval. The transaction must record the proposed content, approving authority, time, resulting version and audit reference.

## 6. Collective Intelligence Workflow (CIW)

The collective-intelligence workflow is a core platform capability and a primary integration target. It must connect contribution, review, evidence, decision and publication into a traceable lifecycle.

A reference workflow is:

1. **Submission:** a member or authorised agent submits a claim, question, proposal or research item.
2. **Identity and scope:** resolve the submitter, ownership, visibility and applicable permissions.
3. **Classification:** identify the item type, topic, sensitivity and evidence status.
4. **Coordination:** VIREÁX routes appropriate work to relevant agents, reviewers or knowledge sources.
5. **Criticism:** assumptions, reasoning and possible failure modes are challenged.
6. **Evidence retrieval:** supporting and opposing sources are collected with provenance and access controls.
7. **Review:** qualified reviewers assess relevance, reliability, method and uncertainty.
8. **Verification:** applicable checks, calculations, tests or replications are performed and recorded.
9. **Decision:** the authorised human or governance process accepts, rejects, revises or defers the item.
10. **Publication:** publication occurs only after the required approval and explicit confirmation.
11. **Measurement:** outcomes and workflow quality are measured without confusing activity with impact.

Each transition must have a defined actor, precondition, output, failure state and audit event. The workflow must support rejection, withdrawal, revision, expiry and supersession, not only successful completion.

### Publication gate

For controlled publication workflows, the required approval fields include:

- `approved_for_publication: true`
- `confirm_publish: true`

Both must be satisfied by the applicable authorised process. An agent's synthesis or recommendation cannot substitute for these approvals.

## 7. Evidence and knowledge model

Claims and evidence must be represented separately. A source may support one claim, challenge another and be irrelevant to a third.

### 7.1 Evidence classes

Use these labels consistently:

| Label | Meaning |
|---|---|
| **ESTABLISHED** | Strongly established within the stated scope by convergent, reproducible evidence or settled technical/mathematical result. |
| **SUPPORTED** | Credible evidence supports the claim, but important limitations, uncertainty or incomplete replication remain. |
| **HYPOTHESIS** | A testable proposed explanation or mechanism that has not been adequately established. |
| **SPECULATION** | An exploratory idea without sufficient evidence or a sufficiently constrained test. |
| **UNKNOWN** | Available evidence does not justify a conclusion. |
| **REFUTED** | The claim, as stated and within its stated scope, conflicts with sufficiently strong evidence or a decisive valid test. |

A label applies to a specific claim and scope, not to an entire person, project or theory. Claims may change status when new evidence arrives. The history of those changes must remain visible.

### 7.2 Evidence record

A useful evidence record should include, where applicable:

- stable evidence identifier;
- linked claim identifier(s);
- source and source author/publisher;
- source location or content reference;
- collection and publication timestamps;
- method and relevant conditions;
- content hash or immutable version reference;
- evidence class and rationale;
- limitations and uncertainty;
- supporting or opposing relationship;
- reviewer and review timestamp;
- supersession or correction links.

### 7.3 Verification levels

Verification labels must describe what was actually done. A project may use V0–V3 as a local verification scale, but each level must have an explicit definition, acceptance criteria and reproducible record. A label must never be used as decoration or as a substitute for test output.

### 7.4 Contradictions

Contradictory claims must not be silently merged or discarded. The system should preserve:
- the conflicting statements;
- their sources and timestamps;
- differences in definitions, scope or assumptions;
- the conclusions affected;
- the current resolution status and rationale.

An unresolved contradiction is a valid system state.

## 8. RAG-001 — Retrieval architecture

The approved RAG-001 architectural direction is:

- **Storage:** PostgreSQL with pgvector.
- **Embeddings:** generated locally.
- **Retrieval scope:** hybrid retrieval across global, institutional and member knowledge, subject to identity, permissions and member sovereignty.

RAG-001 is a permission-aware knowledge retrieval foundation. Its initial scope is ingestion, content hashing, access-control metadata, retrieval and citation. It must not be treated as a complete collective-intelligence workflow or as proof of validated knowledge.

### 8.1 Access control before ranking

Access control must be applied before retrieval results are ranked or exposed.

- The same access predicate must constrain lexical and semantic retrieval.
- A reranker must never receive a chunk the requesting identity cannot read.
- Filtering after ranking is insufficient if restricted content has already influenced scores, ordering, summaries or model context.
- Access must be evaluated against the requesting identity, tenant/node, purpose and current permission state.

### 8.2 Retrieval and provenance

The design may combine lexical retrieval (for example BM25) and semantic retrieval (vector similarity). Their results must retain source references and access scope. A citation must resolve to the source version actually used.

### 8.3 Document lifecycle and metadata

Documents should be versioned immutably. Updates create new versions and must not silently overwrite the history.

Relevant metadata includes:

- `claim_id`
- `evidence_class`
- `valid_from`
- `valid_to`
- `supersedes_id`
- `content_hash`
- `embedding_model`
- `embedding_version`
- `document_version`

Embedding model and dimension decisions must be explicit. Vector indexes must match the approved vector dimensions; do not create an index whose assumptions are not satisfied by the stored vectors.

### 8.4 Retrieval limitations

Retrieval relevance is not factual validation. A highly ranked passage may be wrong, outdated, incomplete or contextually irrelevant. The consuming workflow must evaluate evidence quality and claim fit separately.

## 9. CivScore — transparent measurement

CivScore is the platform's progress and measurement interface. It must be grounded in measurable statistics and member opinions, with the underlying evidence and aggregated opinion results visible on the Civilisation.One website sphere.

CivScore must not conceal the difference between measured activity, member sentiment and a computed score.

### 9.1 Separate the layers

The interface and data model must distinguish:

1. **Statistics:** observable metrics, their definitions, sources, time windows and limitations.
2. **Member opinions:** survey or feedback responses, participation counts, sampling method and aggregation method.
3. **Score calculation:** the explicit transformation or weighting used to derive a displayed score.
4. **Context:** uncertainty, missing data, confidence limits, caveats and historical changes.

Do not present an opinion as an objective statistic. Do not present an activity count as proof of quality or impact. Do not hide weighting choices.

### 9.2 Transparency requirements

For every displayed score, provide enough information to understand:
- what is measured;
- how it is measured;
- which population or sample is represented;
- the relevant time window;
- how missing or invalid data is handled;
- how member opinions are aggregated;
- how the score is calculated;
- when the data was last updated;
- known limitations and possible biases.

### 9.3 Data freshness labels

Use explicit operational labels:

- **LIVE:** currently connected and updating as defined.
- **SIMULATED:** generated or replayed demonstration data, not live member activity.
- **UNAVAILABLE:** the required data source or service is not available.
- **STALE:** data exists but is older than the stated freshness threshold.

Never display simulated or unavailable data as live.

### 9.4 CivScore Observer

The CivScore Observer is an independent observer under the applicable VIREÁX coordination and governance framework.

Its permitted role is strictly:

- read;
- verify;
- challenge;
- report.

It must not modify CivScore inputs, weights, calculations, displayed scores or member opinions. It must not manipulate results or override the authority responsible for score calculation and publication. Observations and challenges must be recorded with provenance.

## 10. Governance and decision authority

Governance must distribute responsibility, make authority explicit and provide review and appeal paths. No single agent or node may unilaterally acquire unrestricted platform authority.

The current governance concept includes:
- **VIREÁX (1):** coordination and intelligence orchestration;
- **Core Council (3):** core governance and oversight;
- **Development Council (5):** development and technical coordination.

This 1+3+5 arrangement is a proposed governance structure, not a claim that a legally constituted body or fully implemented voting system exists. Its operating rules, membership, quorum, conflicts of interest, terms, removal, appeal and emergency powers must be specified before it is treated as operational governance.

### 10.1 Human approval

High-impact actions require approval by an explicitly authorised human or governance process. Examples include public release, sensitive data sharing, changes to governance policy, financial commitments and irreversible external operations.

### 10.2 Decision record

A consequential decision should record:
- the question and scope;
- options considered;
- evidence and unresolved objections;
- decision and rationale;
- authorised decision-maker(s);
- applicable policy and quorum;
- timestamp and version;
- review or appeal route.

## 11. Security, identity and trust

Security must be designed as a set of enforceable boundaries, not merely a list of cryptographic algorithms.

Required design areas include:
- identity and authentication;
- node and service identity;
- least-privilege authorisation;
- scoped credentials and secrets management;
- consent and permission revocation;
- secure communication;
- signed or otherwise authenticated messages;
- freshness, replay and expiry protection;
- audit integrity;
- key rotation and recovery;
- isolation between member, institutional and global data;
- incident response and recovery.

### 11.1 MK3A security direction

The MK3A workstream has considered a cryptographic pipeline involving ML-KEM-768/1024, ML-DSA-65/87, authenticated encryption (AEAD), and optional QKD integration.

A proposed processing sequence is:

**Parse → Schema → Version → Identity → Signature → Freshness → Transcript → Session → Authority → Policy → State**

This is a design direction. It does not by itself establish that an implementation is secure, formally verified or production-ready. Cryptographic choices require threat modelling, correct library use, key lifecycle design, protocol review and independent testing. QKD must not be represented as a general substitute for sound endpoint security or key management.

### 11.2 Threat modelling

Threat models should include compromised member devices, malicious or misconfigured agents, prompt injection, data exfiltration, privilege escalation, replay, forged identity, poisoned knowledge sources, supply-chain compromise, insider misuse and denial of service.

Security controls must be tested against explicit threats and recorded acceptance criteria.

## 12. Functional domains

The platform's functional map includes the following domains:

1. Global Hub
2. VIREÁX intelligence coordination
3. MirrorME member nodes
4. CivScore
5. Knowledge Network
6. Research and scientific collaboration
7. Evidence and verification
8. Collective Intelligence Workflow
9. Governance
10. Identity and membership
11. Security and trust
12. RAG and retrieval
13. Agent network
14. Model routing
15. Memory and context
16. DePIN and distributed infrastructure
17. Thin Line research
18. Simulation and sandbox
19. Communication and collaboration
20. Project coordination
21. Publication and knowledge release
22. Audit and observability
23. Core infrastructure and operations

This is a functional domain map, not a verified transcription of a canonical source-code module register and not a statement that all 23 domains are implemented.

## 13. Research integrity — Thin Line Theory and related work

Civilisation.One may host exploratory theoretical research, including Thin Line Theory (TLT) and related mathematical or physical models.

The canonical conceptual definition currently used for Thin Line Theory is:

> The Thin Line is the proposed foundational geometrical organisation of physical reality, governing the relational structure, orientation, curvature and continuity of spacetime and its physical evolution. It is not an object within spacetime, but a proposed principle underlying the geometrical structure through which physical reality is described.

This is a conceptual definition, not an established physical law or an experimentally validated theory.

Research published through the platform must distinguish:
- established physics and mathematics;
- supported interpretations;
- proposed mathematical structures;
- hypotheses requiring derivation or experiment;
- speculation;
- unknown or unresolved questions.

A mathematically consistent model is not automatically a physically correct model. Dimensional consistency, limiting behaviour, well-posedness, conservation laws, compatibility with established results, parameter identifiability and falsifiable predictions must be examined. Failed tests and negative results must be preserved.

Do not claim experimental confirmation, peer review, replication or consensus unless supported by actual records.

## 14. Data architecture and lifecycle

Data must carry identity, ownership, scope, permissions, provenance and lifecycle state.

A general lifecycle is:

**Create or ingest → Identify → Classify → Authorise → Validate → Store → Retrieve or process → Review → Publish or share → Retain, supersede or delete under policy**

The platform must distinguish:
- member-private data;
- institution-controlled data;
- global/public data;
- derived data;
- temporary processing context;
- durable memory;
- audit records.

Derived data does not automatically inherit permission to be public. Its source dependencies and applicable restrictions must be considered. Deletion, retention and legal obligations must be defined by the relevant policy and jurisdiction.

## 15. Engineering and development protocol

All contributors, including AI coding agents, must follow a disciplined workflow.

### 15.1 Before implementation

1. Read the relevant Codex, repository instructions and existing specifications.
2. Inspect the current code and tests.
3. Identify the exact requirement and affected boundaries.
4. Record assumptions, unknowns, risks and compatibility constraints.
5. Propose a minimal implementation plan.
6. Define acceptance tests before changing behaviour.

### 15.2 During implementation

- Prefer contracts and explicit interfaces before cross-service behaviour.
- Preserve existing working functionality unless a change is authorised.
- Keep changes scoped and reviewable.
- Do not silently change schemas, public APIs, permissions or data semantics.
- Add tests for success, failure, boundary and permission cases.
- Preserve provenance and trace identifiers across service boundaries.
- Keep secrets and private data out of source control, logs and test fixtures.
- Do not introduce a dependency or infrastructure service without documenting its purpose and operational cost.

### 15.3 Before declaring completion

A contributor must report:
- files and interfaces changed;
- behaviour added or changed;
- tests actually run and their results;
- tests not run and why;
- known limitations;
- security and migration implications;
- remaining work;
- commit, branch and pull-request references where applicable.

A build passing is not equivalent to full acceptance. A unit test passing is not proof of end-to-end behaviour. A merged pull request is not proof of deployment. A deployment is not proof of correctness.

### 15.4 Repository discipline

- Inspect the target repository before writing.
- Use a focused branch for non-trivial changes.
- Do not overwrite existing files without inspecting them.
- Keep commits descriptive and atomic where practical.
- Use pull requests for reviewable changes.
- Do not merge, release or deploy without the required authority.
- Never claim to have changed a repository, run a test, merged a PR or deployed a service unless the action actually succeeded and can be verified.

## 16. Implementation status vocabulary

Every component or capability should use a precise status:

| Status | Meaning |
|---|---|
| **IMPLEMENTED** | Present in the target codebase and supported by relevant tests or operational evidence. |
| **PARTIAL** | Some required behaviour exists; defined requirements remain incomplete. |
| **PROTOTYPE** | Experimental implementation; not accepted as production-ready. |
| **SPECIFIED** | Requirements or design exist; implementation is not established. |
| **PROPOSED** | A candidate direction awaiting decision or specification. |
| **UNVERIFIED** | A claim of implementation or operation has not been independently checked. |

Status must be assigned per capability, not inferred from a product name, user interface, roadmap or presentation.

At the time of this document, the architecture includes active implementation tracks and prototypes. The complete end-to-end collective-intelligence workflow, independently validated evidence lifecycle, full review and publication lifecycle, comprehensive CivScore measurement pipeline and production-grade security must not be assumed complete without current acceptance evidence.

RAG-001 is an approved architectural direction with implementation work in progress. Its approval as an architecture does not certify full retrieval integration, permission testing, benchmark performance or production readiness.

## 17. Development priorities

The recommended order of work is:

1. **Complete the collective-intelligence workflow:** establish a functioning, auditable path from contribution through evidence, review, decision and publication.
2. **Complete knowledge infrastructure:** ingestion, immutable versions, ACL-first retrieval, citations and evaluation.
3. **Protect member sovereignty:** identity, consent, scoped permissions, local memory and explicit model routing.
4. **Make CivScore auditable:** verified statistics, transparent opinion aggregation, explicit calculations and freshness labels.
5. **Strengthen governance and security:** authority contracts, human approval, threat modelling, audit and revocation.
6. **Expand distributed coordination:** additional nodes and agents only after core boundaries and failure handling are reliable.

Priorities may be changed through an explicit decision supported by dependencies, risk and evidence.

## 18. Team operating rules

### Engineers
Build only against reviewed requirements. Preserve boundaries, write tests and report limitations honestly.

### AI agents
Operate as bounded tools. State uncertainty, cite sources where available, preserve provenance and request approval when authority is absent. Never invent tool results, repository actions, test outcomes or permissions.

### Researchers
Separate hypothesis from result. Publish methods, assumptions, limitations and negative findings. Make claims falsifiable where the subject permits.

### Reviewers
Challenge the work rather than the person. Examine evidence, logic, security, reproducibility and scope. Record unresolved objections.

### Administrators and governance participants
Exercise only documented authority. Maintain conflicts-of-interest transparency, reviewability and appeal routes.

### Project leads
Maintain a clear distinction between roadmap, specification, implementation, acceptance and deployment. Do not report planned work as completed work.

## 19. Operational principles

Civilisation.One should optimise for:
- reliable cooperation rather than central control;
- verifiable knowledge rather than persuasive output;
- member agency rather than opaque automation;
- transparent measurement rather than gamified authority;
- accountable governance rather than personality-based power;
- resilient distributed systems rather than a single point of trust.

The platform must remain capable of disagreement. A healthy system does not erase contradictions to produce a convenient consensus. It makes disagreements inspectable and creates a path for evidence, review and revision.

## 20. Canonical platform statement

Civilisation.One is a distributed intelligence and knowledge-coordination platform connecting people, independent AI systems, research and communities through sovereign member nodes, evidence-based knowledge production, transparent governance and measurable collective progress.

**VIREÁX coordinates intelligence.**  
**Member nodes retain sovereignty.**  
**Evidence establishes confidence.**  
**CivScore makes progress visible.**

No agent owns truth. No central node owns its members. No score replaces evidence. No recommendation silently becomes an authorised action.

The objective is not to centralise intelligence. It is to make distributed intelligence capable of working together without sacrificing independence, accountability or truth.

---

## Document control

- **Document ID:** CV1-PLATFORM-CODEX-001
- **Version:** 1.0
- **Document status:** PROPOSED
- **Related:** CV1-PLATFORM-CODEX-003 (purpose), CV1-PLATFORM-CODEX-002 (engineering hardening specification)
- **Owner:** Civilisation.One platform governance / authorised maintainers
- **Review trigger:** material architecture, governance, security or evidence-policy change
- **Change policy:** revise by reviewed, traceable commit; retain prior versions in repository history
- **Status note:** this Codex defines principles and intended architecture. Each implementation claim requires separate verification.

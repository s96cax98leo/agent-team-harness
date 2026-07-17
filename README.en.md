# The Agent Team Harness Rulebook

> 中文版(原文):[README.md](README.md)
>
> **Version**: v0.4 (2026-07-17; v0.3 added §6.5–6.7 MCP mechanics & calling conventions; v0.4 added §13 Runtime — Claude Agent SDK + model neutrality)
> **What this is**: A business-neutral master rulebook for AI agent teams. Picture a company: every agent team is a **module department** — the department's line of business (the §4 slot) is plugged in later, but its management system (the 12 harness modules) never changes.
> **How to use it**: Hand this rulebook plus a description of the business you want to an AI. Following the input template in §14 and the derivation method in §4.2, it produces a **complete team definition** (deliverables in §14.2). Changing the business never changes the system — a new team is just a filled-in configuration.

---

## 1. What This Rulebook Is

### 1.1 In one sentence

Every agent team is a **self-governing department**: it has its own manager (Leader), specialists (Workers), quality control (Evaluator), archive room (memory), regulations (guardrails), progress saves (checkpoints), and acceptance system (DoD / test bank). Departments talk to each other **only through formal correspondence between managers (over MCP)**, and every letter must state the project and the goal explicitly — never assuming the other side remembers anything.

### 1.2 Design invariants (hard constraints for the whole document)

1. Every team can **operate, be founded, fail, and be dissolved independently** — it completes the work within its own charter without depending on any other team.
2. Cross-team communication has **exactly one path**: Leader ↔ Leader, over MCP. Workers have no external-facing channel of any kind.
3. Every agent's and every team's **memory is private property**; all cross-boundary communication **must explicitly declare the project and the goal**, and may never rely on shared implicit context.
4. Business content exists only inside the slot (§4); no other chapter of this rulebook may contain any business-specific assumption.

---

## 2. Design Philosophy (Six Axioms + the Fractal Principle)

### 2.1 The six axioms

1. **The harness is the agent's operating system; its value lives in the remaining 20%.** The model provides intelligence; the harness provides reliability: persistence, replayability, cost control, observability, error recovery. These are *properties of the environment* that models will never internalize. 88% of production incidents stem from infrastructure, not the model; **what wins is not the smartest model but the sturdiest system**.
2. **Separate the brain from the hands (intent–execution decoupling).** The model (a probabilistic brain) only *proposes* actions; the system (deterministic hands) verifies compliance before executing, and every execution leaves a tamper-proof record. Just like a company: employees propose, the system approves, everything is on file.
3. **The dumb loop.** The system only dispatches, transitions, and routes; all thinking is left to the model. The system's intelligence shows in *constraint and recovery*, never in making decisions on the model's behalf.
4. **Deletability and composability (build to delete).** When models improve and a mechanism is no longer needed, it must be cleanly removable; every mechanism is independently replaceable — nothing is locked together.
5. **Contract first (narrow seams).** A department's value concentrates in a few narrow, stable external contracts (service catalog, delegation form, closing report). For any cross-boundary interaction, define the contract before discussing the implementation.
6. **Deterministic and probabilistic controls are complementary.** Hard boundaries (permissions, formats, budgets) are enforced by deterministic rules that cannot be bypassed; semantically fuzzy territory goes to the model's judgment. **The tripwire's trigger is never handed to the model** — a model under attack is often at its most confident.

### 2.2 The fractal principle

The same ideas replay at three scales:

```
Scale 1 (inside an agent):  Model  ──contract──▶ Tool             (brain/hands separation)
Scale 2 (inside a team):    Leader ──contract──▶ Worker           (manager assigns specialist)
Scale 3 (between teams):    Leader ──contract──▶ Peer Leader      (inter-department correspondence)
```

Each scale up, the same problems reappear (information contamination, secondhand distortion, permission sprawl, cost explosions), and the same ideas answer them (isolation, summarized handoffs, least privilege, hard budget stops).

---

## 3. Team Organization

### 3.1 The fixed role skeleton (business-independent)

Every team, whatever its business, consists of three role types:

| Role | Company analogue | Duties | Hard constraints |
|---|---|---|---|
| **Leader** | Department manager | Sole external contact; intake, planning, decomposition, assignment, consolidation, reply | **Never operates business tools personally**; only the Leader holds cross-team communication rights |
| **Worker** (1–N) | Specialist | Executes concrete subtasks; own isolated workspace and tool authorization | No external channel; hands the Leader only a 1–2 page structured summary; never delegates outside the team over the Leader's head |
| **Evaluator** | QC / independent review | Reviews output against acceptance criteria (DoD); adversarial fault-finding | Decoupled from the doers and **never omitted** — self-grading always inflates; independent review brings 2–3× quality |

### 3.2 Every team carries all 12 management systems (the 12 modules)

A team is a complete harness instance; what the 12 modules mean at department scale:

| # | Module | The department-scale system |
|---|---|---|
| 1 | Orchestration loop | The Leader's intake→plan→assign→consolidate heartbeat; each Worker's prepare→execute→self-check cycle |
| 2 | Prompt assembly | The briefing every member receives before work: identity & rules > available tools > relevant knowledge > progress > current task — the priority order is never inverted |
| 3 | Structured output | All assignments and reports use fixed forms, never free prose; reasons written before conclusions |
| 4 | Sub-agent orchestration | The Leader's assignment system: staged acceptance (phase gating); internal division of labor is never disclosed externally |
| 5 | Tool integration | Each Worker holds only the minimum tool authorization for its job; tools run in isolation, never touching the department's core credentials |
| 6 | Memory management | The department archive (§5): private, filed by project, with checkout and destruction rules |
| 7 | Context management | Every member's attention is finite (far below the advertised number); past 80% load, forced condensation — preventing quality collapse and lazy early exits |
| 8 | State & checkpoints | Progress saved continuously; interruptions resume from the last save; history replays for audit; parallel branches for what-if trials |
| 9 | Environment initialization | The team founding process (§4.3): translate the mission into a verifiable checklist and prepare an isolated environment before any work starts |
| 10 | Error handling | Errors routed by *who fixes them*: transient→auto-retry; correctable→fed back for self-correction; high-risk→sign-off; budget blown→hard stop |
| 11 | Guardrails | Department regulations (§7): dangerous-operation list, risk tiers, mandatory sign-off points — enforced by the system, not by member good faith |
| 12 | Verification & feedback | The QC system (§9): independent review + acceptance criteria + pre-launch test bank |

### 3.3 Internal division-of-labor rules

- Assignments use **phase gating**: each stage has explicit inputs/outputs and acceptance criteria; nothing advances until the gate passes.
- The internal working topology (serial / parallel / dependency graph) is the department's **private implementation detail**; only the service catalog and case status are public.
- Delegation-chain depth is hard-capped at ≤3 (including cross-team segments); the system automatically detects and blocks "A delegates to B, B delegates back to A" cycles.

---

## 4. The Business Module Slot

### 4.1 Slot definition

Founding a new business team requires **only this configuration** (zero changes to the system):

| Slot field | Content |
|---|---|
| **Team Charter** | Department name, one-sentence mission, capability list (task types accepted externally) → auto-generates the public service catalog (Agent Card) |
| **Toolset** | The tools/systems this business needs, and how authorization is split across Workers |
| **Skill & SOP seeds** | The business's standard operating procedures, loaded into the archive as initial procedural memory |
| **DoD templates** | Machine-verifiable definition-of-done templates for this business's unit of work |
| **Guardrail policy** | This business's dangerous-operation list, risk tiers (low/medium/high/critical), mandatory sign-off triggers |
| **Model tier mapping** | Which subtask classes use which model tier (high-end for complex reasoning, low-end for mechanical formatting) |
| **Memory retention policy** | Retention periods for business data (preferences 7–30 days / verified facts 90–365 days / SOPs managed manually) |

### 4.2 Business → Team derivation method (five steps)

Once you supply the business description, derive the team and agent configuration like this:

**Step 1 — Deconstruct the business.** What is the *unit of work* (an order / a report / a case)? That decides the project unit and the first draft of the acceptance template. Then map the value stream (input → processing stages → output) to get the process skeleton; for each stage list the *verbs* (capabilities → tool list) and *nouns* (data and systems touched → resource boundaries).

**Step 2 — Split agents along four axes** (never by human job titles):

| Axis | Splitting rule |
|---|---|
| Attention | Exploratory subtasks that flood working memory (bulk retrieval, long documents) become their own agent, returning only summaries |
| Tools | Each agent's tool authorization is small and cohesive (ideally under 10); mutually exclusive tool clusters are separated |
| Risk | Irreversible outward operations (sending, payments, deletion/modification) become a sign-off-controlled agent, physically separated from read-only analysis |
| Cost | Complex reasoning and mechanical work are split so model tiers can be routed |

Plus the fixed skeleton: Leader + Workers + independent Evaluator.

**Step 3 — Brake on splitting.** Minimize agent count. Multi-agent coordination costs run an order of magnitude above a single agent; research shows a single agent matches multi-agent setups on roughly two-thirds of tasks at half the cost. **Every split must name which of the four axes justifies it; if none does, don't split.**

**Step 4 — Choose the working topology.** Stable, predictable flow → plan-then-execute pipeline; exploration needed → reactive Workers; parallelizable subtasks → fan-out and merge. Always with staged acceptance; never "everyone chats freely with everyone" swarm collaboration (17× the errors, 15× the cost).

**Step 5 — Fill the slot + acceptance.** Produce every §4.1 field, and build the business a **Golden Set** — 10–30 test cases judged by mechanically checkable rules (no AI subjective scoring) — as the launch gate and the regression baseline for every future revision.

### 4.3 Standard team founding process

```
Fill in the Team Charter
  → Environment initialization (isolated environment, credential isolation, archive setup;
     translate the mission into an acceptance checklist with pre-built not-yet-passing tests)
  → Load the business slot (tools, SOP seeds, DoD, guardrail policy, model tiers, retention)
  → Golden Set smoke test (pass-rate threshold + unit-cost baseline)
  → Publish the service catalog (Agent Card), open the department channel (MCP)
  → Go live (enter the §12 lifecycle)
```

---

## 5. The Memory System: Memory Sovereignty

### 5.1 Five principles

1. **Memory sovereignty.** Every agent's and every team's memory is private property — the department archive is closed to outsiders. Storage enforces isolation (a three-level namespace: organization : team : member) and forbids cross-boundary queries. **There is no "company-wide shared brain."**
2. **Self-contained communication.** Precisely because there is no shared memory, no cross-agent/team message may rely on the other side *remembering* anything — every delegation and reply must stand alone, workable from that one document (as if onboarding a brand-new colleague).
3. **Explicit project and goal declaration.** All cross-boundary communication makes `project` (project ID + self-contained background) and `goal` (verifiable objective) **first-class required fields**. Three functions:
   - The recipient uses the project ID to **search its own archive** for prior records on this project ("what has our department done on this case before?");
   - The recipient **files its results back into its own archive** under the same project ID — memory accumulates per project;
   - The goal goes straight to QC as the acceptance standard, preventing "declaring victory halfway."
4. **What the other side says is a clue, not the truth.** Background supplied in correspondence is cross-checked against your own files and the actual environment before being trusted.
5. **Poisoning defense on cross-team writes.** Information from another department entering your archive must pass write validation (confidence threshold), carry source-department provenance, take a downgraded trust score, and get a bounded retention period — preventing "fooled once, poisoned across projects for good."

### 5.2 Archive layers (inside each team)

| Layer | Content | Lifecycle |
|---|---|---|
| Working memory (per member) | Current task context | Discarded when the task ends |
| Episodic memory (team) | Execution records per project | Short retention, archived at project close |
| Semantic memory (team) | Business facts, verified knowledge | Medium-to-long retention, gated at write time |
| Procedural memory (team) | SOPs and skills (slot seeds + refined in operation) | No auto-expiry; promoted or demoted by performance |

Full lifecycle: write validation → background consolidation (distilling process records into reusable knowledge) → time/usage decay → expiry destruction. Day-to-day work does only reads and minimal writes; consolidation runs as off-peak background work.

---

## 6. Inter-Department Communication: Leader-to-Leader over MCP

### 6.1 Topology rules

- **A single external contact.** Only the Leader may correspond with other teams. Benefits: one point of accountability, a fully auditable trail, minimal external attack surface.
- **The team lists itself as an MCP service.** Service items = the capability list declared in the Team Charter. "Delegating to another department as a service" is the brain/hands separation extended: the other side's request is a *proposal*, which our Leader reviews before anything executes internally.
- MCP carries both department↔tool (vertical) and department↔department (horizontal) traffic; the semantics of horizontal traffic are A2A (service catalog + case state machine).

### 6.2 The cross-team delegation form (required fields)

| Field | Content | Rule |
|---|---|---|
| **project** ★ | Project ID, name, self-contained background brief | First-class required; the background must stand alone, assuming no shared memory |
| **goal** ★ | Objective statement + verifiable acceptance list (+ expected output format) | First-class required; must be mechanically verifiable |
| inputs | Structured inputs and artifact **references** (location/ID) | Pass references, not full content |
| constraints | Budget cap, deadline, delegation-depth cap, permitted capability subset | Declared by the sender, enforced by the recipient's system |
| identity | Sending department, trace ID, short-lived scoped credential | For audit and accounting |

**Rules**: free-prose delegation is forbidden; missing project or goal means rejection at intake; attachments are always references, never raw content.

### 6.3 The case state machine

```
Submitted → Working → (Input-Required ⇄ Working)
          → Completed | Failed | Canceled
```

- Every state change notifies the sender; Input-Required is the cross-department channel for supplements and sign-offs.
- Every case **receives exactly one closing notice** — without it, the sender never knows whether the case ended.

### 6.4 The closing report (reply fields)

| Field | Content |
|---|---|
| status | success / partial / failed |
| key_findings | Structured highlights (one to two pages) |
| artifacts | References to output locations, **never inlined content** |
| confidence | Confidence score (0–1) |
| cost | Resources and time consumed (for cross-department accounting) |
| next_suggested_action | Suggestion for the sender (optional) |

**Forbidden** in replies: full work logs, raw system responses, internal department conversations — handoffs are summaries, preventing contamination and cost blowups.

### 6.5 MCP mechanics: the department's postal system

MCP's three primitives map onto a team's three external assets:

| MCP primitive | Meaning | Team-scale counterpart |
|---|---|---|
| **Tools** (verbs) | Callable actions | Case-management tools (§6.6) + named capability tools from the service catalog |
| **Resources** (nouns) | Readable data | The service catalog (Agent Card) and reference reads of public artifacts |
| **Prompts** (templates) | Pre-built request templates | Delegation-form templates (business-specific versions of the §6.2 fields, ready for senders) |

Role duality: **every team harness is both an MCP client and an MCP server** —

- **As a client (outbound calls)**: vertical calls to its own business tools (Workers, within their in-team authorization only); horizontal delegation to other teams (Leader only — a Worker's client configuration **contains no other team's endpoint at all**: hard isolation, not discipline).
- **As a server (public listing)**: the whole team is externally **one MCP server**; behind the endpoint sits the Leader — every inbound call passes the §6.8 intake pipeline before touching anything internal.

### 6.6 Calling conventions: the standard toolset (identical for every team)

A team's MCP server exposes two tool groups:

**A. Case-management tools (business-independent, fixed six)**

| Tool | Purpose | Input → Output |
|---|---|---|
| `describe_team` | Read the service catalog: capabilities, state machine, auth requirements, current load | (none) → Agent Card |
| `delegate_task` | Submit a delegation | Delegation form (§6.2) → `task_id` + `Submitted` |
| `get_task_status` | Check case progress | `task_id` → current status + progress summary |
| `provide_input` | Answer an Input-Required | `task_id` + supplement → back to `Working` |
| `cancel_task` | Cancel a case (triggers §8 compensation) | `task_id` → `Canceled` + list of compensations performed |
| `get_task_result` | Fetch the closing report | `task_id` → closing report (§6.4; artifacts as references) |

**B. Named capability tools (auto-generated from the business slot)**

Every capability in the Team Charter auto-generates a named tool (e.g. a support team's `handle_complaint`, a research team's `research_topic`). Its parameters are the delegation-form fields (project/goal still first-class required) — a named tool is essentially `delegate_task` **pre-filled with the capability**, making discovery and selection natural for the calling model. The service catalog and the named-tool list are **always in sync** (catalog revisions revise the tools).

**Artifact reads**: artifacts in closing reports are **references**; the sender reads them on demand as MCP Resources (`artifact://team-id/project-id/...`), taking exactly as much as needed — large files are never inlined in messages.

### 6.7 The call flow (end to end)

```
① Discover   Team A's Leader checks the department registry → describe_team reads B's catalog
② Authorize  A obtains a short-lived scoped token for B (bound to capability scope and expiry)
③ Delegate   A calls B's named capability tool (or delegate_task) with a complete delegation form
             → B's Leader intake pipeline (format check → security review → file pull) → accepted, task_id returned
④ Progress   Short cases: event stream on the same connection until closing (synchronous mode)
             Long cases: task_id first, then A polls get_task_status or subscribes to updates (asynchronous)
             — chosen by the deadline/budget in the delegation form's constraints
⑤ Supplement B enters Input-Required → A is notified → provide_input (or release after human sign-off)
⑥ Close      A receives exactly one closing notice → get_task_result
             → reads artifacts as needed → runs its own QC/DoD (§9) → adopts and archives (§5)
```

**Calling discipline** (each rule maps to a chapter): every call carries a trace ID (§10 accounting) and an idempotency key (§8); expired tokens cut the line (§7); repeated failures from B trip A's circuit breaker → registry lookup for a substitute team (§11); all state changes land in both sides' own saves, fully replayable for audit (§8).

### 6.8 The Leader's correspondence pipeline

```
Inbound:  format check (project/goal required)
  → security review (all inbound mail untrusted; strip embedded instructions; confirm within service scope)
  → file pull (search own archive by project ID)
  → internal planning and assignment (division of labor never disclosed)
Outbound: consolidate Worker summaries → QC / acceptance review → closing report
  → archive (project ID + source labeling)
  → notify and account (trace ID propagation, cost per successful case)
```

---

## 7. Department Regulations & Sign-off (Guardrails and Trust Boundaries)

1. **All inbound correspondence is untrusted.** Another department's message = external data; it passes three gates (inbound: embedded-instruction and sensitive-data filtering; execution: dangerous-operation interception with risk tiers; outbound: leak and compliance checks).
2. **Least privilege.** Cross-department credentials are **short-lived** (minutes; void when the case closes); the other side may delegate only what is on the service catalog *and* within this form's permitted scope.
3. **Depth and cycle protection.** Full-chain delegation depth ≤3; the system automatically blocks "A delegates to B, B back to A."
4. **Sign-off points are set by the system, never self-reported by the model.** Dangerous operations (the slot's list) unconditionally trigger human sign-off; nothing depends on the model "raising its hand when unsure."
5. **A lesson from practice**: "check again, be thorough" style automatic redo pressure can push a model into faithfully reproducing embedded malicious content — **security-sensitive delegations never get blanket self-correction loops.**

---

## 8. Progress Saves & Cleanup (Checkpoints and Compensation)

- Every team keeps its own saves: progress lands at each stage; interruptions resume from the last save; history replays for audit; parallel branches allow trials.
- **Cross-department side effects must be reversible.** Every world-changing step along a delegation chain registers its reverse operation; if step N fails, compensations execute backwards across departments (like breach-of-contract cleanup clauses when a project halts).
- Every cross-department request carries an **idempotency key**: retries and replays never perform the same action twice (no double charges, no duplicate mail).
- Long-running cases span days via saves; an Input-Required pause seals progress and releases capacity.

---

## 9. Quality Management (Verification & Feedback)

- **Independent review before adoption.** A closing report from another department passes your own QC and the acceptance criteria from the original delegation form before merging into your work (trust, but verify).
- **The test-bank system (EDD).** Every business team runs its Golden Set before launch and continuously after — judged by mechanical rules, not AI subjective scoring, reproducible across models.
- **Test the system, not the employee.** Safety cases verify that the dangerous operation *was not executed*, not that the model *never considered it* — members may think; the system must block.
- Reversible errors → show the cause and let the actor self-correct (bounded retries); irreversible operations → mandatory sign-off.

---

## 10. Cost & Performance

- **Trace IDs travel across departments**: status, consumption, duration, and outcome of every delegation are queryable end to end.
- **The accounting unit is cost per successful case** (not cost per token): evidence shows cheap options that fail and retry end up costing the most — "failure is expensive."
- Model routing is built in: complex cases to high-tier models, routine cases to low-tier, routed by the system automatically.
- Three hard caps (turns / budget / depth) enforced by the system; below 10% remaining budget, forced save-and-stop pending instructions.

---

## 11. Multi-Team Collaboration (Company Level)

- Permitted shapes: **hierarchical** (a coordinating department dispatches to others), **pipeline** (departments hand off in sequence), **dependency graph** (parallel where dependencies allow) — always with staged acceptance.
- **Company-wide free-for-all chat is forbidden** (every department talking to every department): unauditable, prone to infinite debate, 15× the cost.
- **Failover = switch departments.** A persistently failing department (breaker open) → find a capability-matched substitute in the catalog; none available → escalate to humans.
- Departments align through contracts and acceptance criteria, not through meetings seeking consensus.

---

## 12. The Team Lifecycle

| Stage | Content |
|---|---|
| **Founding** | The §4.3 standard process (initialization, isolation, slot loading, test-bank acceptance) |
| **Listing** | Publish the service catalog (capabilities, contact, auth requirements); register in the company directory |
| **Operation** | Accept delegations, accumulate per-project files, self-refine SOPs (new skills trial first, promoted on merit, demoted on failure) |
| **Reorganization** | The business slot updates with versioning (the public catalog is a contract; breaking changes require notice and transition); the test bank evolves in step |
| **Dissolution** | Deletability: delist from the directory, revoke credentials, archive or destroy files per retention policy, tear down the environment — no debris left behind |

---

## 13. Runtime: Claude Agent SDK + Model Neutrality

### 13.1 The execution engine = Claude Agent SDK

Every agent (Leader, Worker, Evaluator) runs on the **Claude Agent SDK**. Rulebook concepts map onto SDK capabilities:

| Rulebook concept | SDK capability |
|---|---|
| The Leader's main loop (intake→plan→assign→consolidate) | SDK main agent (session) |
| Workers (isolated workspace, own tool authorization) | SDK **subagents** — natural context isolation with per-agent tool allowlists, matching the §3 assignment system exactly |
| Tool access (§6 vertical traffic) | The SDK's native MCP client (each agent mounts only the MCP servers on its own authorization list) |
| The team's public listing (§6.5 server side) | An MCP server wrapping the team's SDK instances; behind the endpoint sits the Leader's correspondence pipeline |
| Sign-off & guardrails (§7) | SDK hooks (pre-tool-use interception → dangerous-operation sign-off points) + permission settings (allow / ask / deny) |
| Progress saves & resumption (§8) | SDK session resumption, combined with this rulebook's checkpoints and case state machine |

**Principle: the SDK is the engine; the rulebook is the system.** The SDK supplies mechanical capability (loop, subagents, MCP, hooks, sessions); the 12-module system decides how it is used. What the SDK does not cover — memory sovereignty and project filing (§5), cross-team delegation forms and the state machine (§6), cross-department compensation (§8), the test bank (§9), cost-per-successful-case accounting (§10) — the harness layer adds on top.

### 13.2 Model neutrality: every model works

The engine is bound to the SDK; **the model layer is bound to no vendor** — the hourglass's backend seam made real:

1. **Gateway-based model access.** The SDK speaks one API format; a model gateway translates requests to the actual provider — native Anthropic (Claude), **OpenAI and any OpenAI-compatible endpoint**, **local models via Ollama**, or other clouds. Switching models = changing gateway configuration; the team's system and processes change not one word.
2. **Per-role model assignment.** The slot's model-tier mapping (§4.1) resolves to concrete providers and models, **independently per role** — e.g. the Leader on Claude (planning and external correspondence), routine Workers on local Ollama models (cost), the Evaluator on OpenAI (QC on a different vendor's model also reduces same-source bias — "the same model grading itself").
3. **Capability detection and degradation.** Before any model joins, check the minimum capability bar (at least: tool calling and structured output); models below the bar don't take that role, or are downgraded to positions not needing the capability. **No mechanism in this rulebook may depend on any single vendor's exclusive feature** — every system in §5–§9 must run on a model that can only call tools and produce structured output.
4. **Failover.** A provider fails or throttles → circuit breaker → the capability matrix picks the next provider (the model-level version of §11's "switch departments"); only when all fail does it escalate to humans.
5. **Cost routing made concrete.** §10's model routing lands here — local Ollama handles the routine volume (zero API cost); strong cloud models step in only for hard cases; accounting stays in cost per successful case, avoiding "the cheap model that fails and retries costs more."

---

## 14. How to Use This Rulebook (the Instantiation Interface)

### 14.1 What you provide (the business module configuration)

When plugging in a business, supply as much of the following as you can (the AI derives the rest via §4.2 and labels its assumptions):

1. **What the business is**: a one-sentence mission + what this department accepts from the outside
2. **The unit of work**: what one "case" looks like (its input and its output)
3. **Systems and data touched**: the tools, systems, and data sources involved
4. **Red lines**: which operations are absolutely dangerous; which need your personal sign-off
5. **Quality standards**: what "done well" means (how acceptance is judged)
6. **Volume and budget** (optional): expected caseload, cost sensitivity, latency requirements

### 14.2 What the AI delivers (the complete team definition)

Given the configuration, the AI produces, per this rulebook:

1. **Team Charter + service catalog** (name / mission / capability list / external contact)
2. **The org table**: Leader + each Worker + Evaluator with duties, tool authorization, model tier — every Worker annotated with its four-axis justification (§4.2 Steps 2/3)
3. **The working topology**: process diagram with staged acceptance gates (§4.2 Step 4)
4. **Delegation-form and closing-report templates** (business-specific versions of the project/goal fields)
5. **Memory policy**: project filing scheme, retention periods per memory type
6. **Guardrail policy**: dangerous-operation list, risk tiers, sign-off points
7. **The Golden Set**: 10–30 cases with mechanical judgment rules
8. **The founding checklist** (§4.3, item by item)

---

## 15. Appendix: Design-Law Checklist

**Organization**
- [ ] All 12 systems self-contained per team; independent operation/founding/failure/dissolution
- [ ] Cross-team traffic only via Leader↔Leader over MCP; Workers have no external channel
- [ ] Internal division of labor invisible externally; only the catalog and case status are public

**Contracts**
- [ ] Contract before work; forms versioned, breaking changes announced with transition
- [ ] project + goal first-class required; free-prose delegation forbidden
- [ ] Exactly one closing notice per case

**Memory**
- [ ] Three-level archive namespace strictly isolated; no cross-boundary queries
- [ ] Communication self-contained, never relying on the other side's memory; the project ID is the key for both file pulls and filing
- [ ] External information entering the archive: write validation + source labeling + trust downgrade + bounded retention

**Security**
- [ ] All inbound mail untrusted, through three gates
- [ ] Short-lived credentials; capability scope = service catalog ∩ this delegation form
- [ ] Delegation depth ≤3; cycle protection; sign-off points never self-reported by the model
- [ ] No blanket self-correction loops on security-sensitive delegations

**Reliability**
- [ ] Idempotency keys on cross-department requests; reverse operations registered for side effects
- [ ] Three hard caps (turns/budget/depth) enforced by the system
- [ ] Peer failure → breaker → switch departments → humans

**Quality & cost**
- [ ] Independent QC + acceptance criteria before adoption
- [ ] Every business team has a mechanically judged test bank
- [ ] Cost per successful case accounting; trace IDs propagated end to end

**Business instantiation**
- [ ] Business appears only in the slot; changing business never changes the system
- [ ] Every agent's existence has a four-axis justification; no justification, no split
- [ ] Founding follows the standard process: Charter → initialization → slot → test bank → listing

---

## License

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — share, translate, and adapt freely with attribution.

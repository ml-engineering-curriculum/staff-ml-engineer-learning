# The Delegation Contract to Eval and Experimentation Specialists

## Motivation

Chapters 2 through 5 authored four contracts. Every one of them says *what* the ML org's evals, judge deployments, rollouts, and experiments must satisfy, and *who* arbitrates when the answer is unclear. None of them says *how* the underlying work is actually done well at the individual-contributor level: how a specific held-out set is drawn to be adversarial without being unfair, how a judge prompt's rubric is iterated to sit above the bias floor, how a Bayesian sequential test's prior is chosen to defeat the "always-valid" pathology on a genuinely-noisy metric.

That work exists. It is real, it is technically demanding, and it is what the peer specialist tracks own. The Staff engineer's temptation — especially the newly-Staff engineer who did every one of these tasks well at L30 as their team's eval lead — is to keep doing them, badged as "program work" but actually done at specialist altitude. That drift produces the same career failure mode Larson (*Staff Engineer*, 2021) names as the sub-Staff trap: the engineer is technically superb, appears busy, and never actually operates at Staff altitude. The delegation contract is the fix.

This chapter walks the hand-off contract to the two peer specialist tracks — [`model-evaluation-engineer-learning`](https://github.com/ml-engineering-curriculum/model-evaluation-engineer-learning) and [`ai-eval-engineer-learning`](https://github.com/ml-engineering-curriculum/ai-eval-engineer-learning). Its shape mirrors [`mod-404/01-scope-and-hand-off.md`](../mod-404-distributed-training-systems-at-scale/01-scope-and-hand-off.md)'s hand-off treatment: what Staff owns, what specialists own, what flows in each direction across the boundary, and what the escalation path is when the boundary is violated.

## The two peer specialist tracks and what they own

### model-evaluation-engineer-learning

The individual-contributor track for a team's eval-authoring depth on *classical* ML systems — supervised classifiers, ranking, recommendation, forecasting. Owns:

- **Eval-set construction.** How a held-out set is drawn to be i.i.d. with the deployment distribution, how a golden regression set is built and grown, how a slice list is chosen to capture the business-critical cuts.
- **Annotator programs.** Labelling protocols, inter-annotator agreement measurement, adjudication procedures. This is where the ai-eval-engineer track overlaps but the classical-ML lineage is model-evaluation-engineer's.
- **Metric implementation.** The actual code that computes NDCG@10, recall@k, PR-AUC, calibration curves, group-fairness metrics.
- **Regression-test authoring.** The CI-level eval checks that run on every model change; the harness that produces the numbers the chapter-2 gate reads.

### ai-eval-engineer-learning

The individual-contributor track for eval on *generative* AI systems — LLMs, agents, RAG, multimodal. Owns:

- **Judge-model prompt engineering.** The rubric prompt itself; iteration against a calibration set; robustness to prompt injection in the judge's own instructions.
- **Preference-collection protocols.** Chatbot-Arena-shape pairwise-preference collection, rubric-anchored scalar collection, elicitation of subject-matter-expert judgements.
- **Judge bias measurement.** The instrumentation that produces the position / verbosity / self-enhancement / rubric-drift numbers the chapter-3 policy demands.
- **Adversarial and red-team eval sets** for generative systems — jailbreak prompts, prompt-injection payloads, factuality edge cases.

Some orgs merge these into a single "AI Eval" role; other orgs keep them separate. The delegation contract works either way, but the Staff engineer names which shape their org uses in the contract's opening paragraph.

## What Staff owns vs. what the specialists own

**Staff owns.**

- **The org-wide standards** — the four contracts from chapters 2 through 5.
- **The tier classifications** — which use of a judge is Tier A, B, C.
- **The review-body operation** — running the review-gate, arbitrating cross-team interference.
- **The delegation itself** — this document.
- **The exec-audience roll-up** — the one-page summary of "how the eval program is going" for leadership.

**Specialists own.**

- **The eval-set and judge-prompt implementation** — the technical work that satisfies the contracts.
- **The measurement of judge bias** — the numbers the chapter-3 policy reads.
- **The team-level authoring of annotator protocols and adjudication procedures.**
- **The technical depth on statistical eval methodology** — inter-annotator agreement, judge-calibration statistics, adversarial-set construction — that the Staff engineer references but does not authoritatively author.
- **The individual-model quality bar on a specific team's system.**

If the Staff engineer at mod-406 finds themselves iterating a judge prompt, either they are pair-programming with the ai-eval-engineer during a hand-off review — legitimate but time-boxed — or the delegation has broken and the Staff engineer has quietly slid into specialist scope. Common tell: the Staff engineer's calendar starts looking like a specialist's calendar.

## What flows in each direction

**Downward — from Staff to specialists.**

- **The contracts** — the chapter-2 standard, the chapter-3 policy, the chapter-4 rollout contract, the chapter-5 platform contract. Specialists implement to satisfy these.
- **The tier classifications** — which judge deployments are Tier A vs. B vs. C. Specialists implement the calibration and bias-tracking cadence the tier requires.
- **The review-body verdicts** — accept / block / defer / nudge on their team's eval submissions.
- **The metadata schema** — the eval-run metadata that the chapter-2 gate reads. Specialists implement the recording; Staff authors the schema.

**Upward — from specialists to Staff.**

- **The measured judge-bias numbers** — the chapter-3 dashboards.
- **Calibration-vs-human numbers** — the quarterly calibration outputs.
- **New failure-mode reports** — a specialist discovers a novel judge failure mode or a new adversarial vulnerability; the Staff engineer's job is to fold it into the standard.
- **Escalations on infeasible contract clauses** — when a contract clause cannot be implemented at the specialist's altitude (e.g. the required calibration cadence is impossible at the current annotation budget), the specialist escalates and the Staff engineer either revises the contract or defends the resource requirement.

The upward flow is where the Staff engineer's contract stays honest. If nothing ever escalates upward, either the contract is too permissive (specialists are doing whatever they want and calling it in-contract) or the specialists have given up. Chapter 5's review body is the mechanism that catches both.

## The hand-off review cadence

The delegation contract is not a document filed once. It is refreshed on a cadence:

- **Weekly** — the review-body meeting (chapter 5) is where Staff and specialists meet operationally.
- **Monthly** — a longer meeting (60-90 minutes) where the specialist track leads and the Staff engineer walk the recent contract-versus-reality gaps. This is where the "here is what we cannot implement" conversations happen without the pressure of a live experiment on the table.
- **Quarterly** — the contract itself is re-opened. What is in the standard that is not delivering value? What is missing? What does the org's changed model portfolio (a new modality, a new regulated system, a new judge-model vendor) require the standard to add?

The quarterly cadence matters. A yearly cadence lets the contract drift into decoration (mod-402's silent-contract-erosion failure mode); a monthly cadence turns the contract into a document nobody reads because it changes too fast to remember.

## Escalation triggers

Three concrete triggers for escalation up the delegation contract:

1. **A specialist repeatedly cannot implement a contract clause** at the current headcount or budget. Escalates to Staff, then to the peer EM per the mod-401 partnership contract, then to the mod-410 leadership-comms channel. The output is either resource, contract revision, or documented risk acceptance.
2. **A specialist discovers a failure mode the contract does not name** and the failure is showing up in production. Escalates to the review body for immediate incorporation into the standard, and — if the failure has already caused a launch to ship badly — into mod-408's incident chain.
3. **The Staff engineer discovers they are doing specialist work.** Self-escalation. The correct move is to name it in the next weekly review, hand the work back to the specialist explicitly, and refit the Staff engineer's calendar. Larson (*Staff Engineer*, 2021) frames this as the "no-op day" a Staff-plus IC needs — if there is no such day, the delegation is broken.

## What the delegation contract does not cover

- **Individual specialist career development.** That belongs to the EM and to the specialist's own track's coaching mechanisms. Staff can and should coach specialists on scope and altitude — mod-410 chapter 5's L30-coaching content — but the day-to-day career work is not this document's territory.
- **The specialist tracks' internal roadmaps.** How model-evaluation-engineer chooses to prioritise their own capability-build backlog is their business. The delegation contract names *what the Staff engineer's program requires*, not *how the specialist track sequences its own work*.
- **The platform-team boundary.** The delegation to peer platform tracks (ai-infra-mlops for the experimentation platform build, for example) is a *platform-consumer* relationship, walked in chapter 5, not a specialist-delegation relationship. The two are distinct; a common newly-Staff error is to treat them as the same.

## What the specialists' expectations of Staff should be

The delegation is bidirectional. Specialists' reasonable expectations of the Staff engineer:

- **Clear contracts.** No "figure out what the standard should say" ambiguity. The contract is written down, versioned, and refreshed on the cadence above.
- **A working review body.** Chapter 5's review body actually meets, actually arbitrates, and actually protects the specialist's time from cross-team disputes the specialist should not be resolving.
- **Air cover on scope creep.** When a team lead tries to demand specialist work at a scale the contract did not sanction, the Staff engineer defends the contract. Specialists should not have to say no to team leads alone.
- **Accurate exec framing.** When the Staff engineer's exec-audience roll-up on the eval program cites specialist work, it cites specialists by name. Specialists' work should be visible to leadership through the Staff engineer, not laundered as the Staff engineer's own.

## The artifact this chapter produces

A single **delegation-contract document**, one page. Its structure:

1. Which specialist tracks the org uses (model-evaluation-engineer, ai-eval-engineer, or a merged role) and their leads.
2. What Staff owns (five bullets).
3. What specialists own (five bullets).
4. What flows downward and upward (the two lists above).
5. The three cadences (weekly / monthly / quarterly).
6. The three escalation triggers.
7. The specialists' expectations of Staff.

This is not one of the four numbered exercises' primary deliverables, but the mod-402 chapter 5 RFC that the eval-program artifacts roll up into will reference this document in its "delegation" section. The mod-401 chapter 5 hand-off content is the closest peer.

## Summary

- The delegation contract to the peer specialist tracks — **model-evaluation-engineer** and **ai-eval-engineer** — is what keeps the Staff engineer out of specialist scope and in the program work the promotion committee is actually reading them against.
- Staff owns the four contracts, the tier classifications, the review body, and the exec roll-up. Specialists own eval-set construction, judge-prompt engineering, bias measurement, and single-team quality depth.
- The hand-off runs on three cadences — **weekly** at the review body, **monthly** at the specialist-track sync, **quarterly** for contract refresh — and has three escalation triggers.
- The specialists' expectations of the Staff engineer are named explicitly: clear contracts, a working review body, air cover on scope creep, and accurate exec framing.
- A common newly-Staff failure is drifting into specialist work while calling it program work; the "no-op day" self-check catches it early.

This is the last chapter of mod-406. The learner should now hold four documents — the offline-eval standard, the LLM-as-judge policy, the rollout contract, the experimentation-platform consumer contract — plus the review-body charter and this delegation contract. Together they are the systemic fix for the eval-metric-conflict failure mode from [`mod-402/03-portfolio-failure-modes.md`](../mod-402-multi-team-ml-architecture/03-portfolio-failure-modes.md), and the eval-program layer of the portfolio blueprint. The next module to read is [`mod-408-portfolio-reliability-and-incident-command`](../mod-408-portfolio-reliability-and-incident-command/) for the reliability side of the same rollout contract, or [`mod-407-capacity-cost-and-tco`](../mod-407-capacity-cost-and-tco/) for the cost side of the eval-and-judge budget.

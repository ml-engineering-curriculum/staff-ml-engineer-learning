# mod-410-staff-plus-technical-leadership: Staff-Plus Technical Leadership — Roadmaps, Exec Comms, Hiring Calibration, Coaching Seniors

**Estimated effort:** 14 hours

The capstone module of the Staff Machine Learning Engineer track. Closes the loop mod-401 opened by naming what *sustained* staff-plus technical leadership looks like once the learner has authored the artifacts of mod-402 through mod-409 — multi-quarter roadmaps leadership actually commits to, executive-audience one-pagers and RFCs (including Amazon PR/FAQ working-backwards for new programs), staff-plus hiring loops with rubric-based ML interviews and hire / no-hire / senior-vs-staff calibration, coaching plans for L30 senior tech leads that stay on the Staff side of the mod-401 chapter 5 partnership contract with the peer EM, vendor and partnership judgement calls (LLM API vendor mix, foundation-model provider strategy, cloud provider trade-offs, MLOps vendor selection), and the personal sustaining discipline that keeps the archetype picked in mod-401 as an ongoing constraint over multi-quarter tenure.

Builds on [`mod-401-staff-ml-role-scope`](../mod-401-staff-ml-role-scope/) (archetypes, altitude vocabulary, staff/EM partnership contract) and every intermediate module (mod-402 through mod-409, which produced the technical artifacts this module's leadership package wraps around). Feeds forward into [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/) — the leadership-simulation capstone — and into the [`principal-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning) next-up track for the L50 transition. Sits sideways to [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 Engineering Manager track — which authors the mirror image of chapter 5's Staff/EM partnership contract from the EM side.

## Learning objectives

- Author a multi-quarter ML program roadmap that leadership commits to — with defended sequencing, clear non-goals, resource asks, and dependencies on peer platform teams.
- Write executive-audience one-pagers and RFCs — decisions the CTO / CEO / board can read and react to; separate what needs a decision from what needs an FYI.
- Run staff-plus hiring loops — calibrate senior ML engineers, decide the bar between hire / no-hire / senior-vs-staff, and coach hiring committees to run rubric-based ML interviews.
- Coach senior tech leads (L30) to run their reviews, roadmaps, and mentorship at bar; partner with the EM peer on career-ladder conversations without doing the manager's job.
- Make vendor and partnership judgement calls at staff scope — LLM API vendor mix, foundation-model provider strategy, cloud provider trade-offs — and defend them in the room.
- Sustain the technical bar without sliding into IC-only work or into management-only work — stay in the staff archetype the learner picked in mod-401.

## Lecture chapters

1. [`01-what-staff-plus-technical-leadership-means.md`](01-what-staff-plus-technical-leadership-means.md) — What sustained staff-plus technical leadership is as scope of work; distinguished from over-indexing on the Solver archetype and from crossing into the peer EM path; the "am I sustaining staff-plus scope?" heuristic; revisits the four archetypes as ongoing constraints now that the learner has worked mod-402 through mod-409.
2. [`02-multi-quarter-ml-program-roadmap.md`](02-multi-quarter-ml-program-roadmap.md) — The five-column roadmap leadership commits to: diagnosis, programs, sequencing, non-goals, resource asks. Sequencing defended by naming counterfactual breakage; non-goals as first-class outputs (Rumelt); resource asks tied to a program-level delta; peer-platform-team dependencies with a written slip fallback; "no" in writing as a valuable output.
3. [`03-executive-audience-rfcs-and-one-pagers.md`](03-executive-audience-rfcs-and-one-pagers.md) — Separating decision doc / FYI / options paper; the one-page structure with recommendation above the fold (Minto pyramid); Amazon PR/FAQ working-backwards for new-program framing; trade-off framing in the exec's units, reversibility class (Bezos Type 1 / Type 2), and counterfactual cost; when an RFC is the wrong artifact for the audience.
4. [`04-staff-plus-hiring-loop-calibration.md`](04-staff-plus-hiring-loop-calibration.md) — Rubric-based ML interviews (four technical slots — system design, fundamentals depth, hands-on, staff-plus judgement); calibration between hire / lean-hire / no-hire / lean-no-hire with "tie goes to no-hire" (Fournier); senior-vs-staff as a downlevel-only decision; debias practices and hiring-committee coaching; the mod-401 chapter 5 hiring boundary held from the Staff side.
5. [`05-coaching-senior-tech-leads-and-em-partnership.md`](05-coaching-senior-tech-leads-and-em-partnership.md) — Three coaching modes (review don't rewrite / pair then withdraw / sponsor not sponsor-as-shield); where coaching stops and EM career management starts; the weekly Staff/EM 1:1, the quarterly technical-bar read, and real-time signal; the failure mode of taking L30 work back under deadline; the special case of coaching an L30 who is deeper than the staff engineer.
6. [`06-vendor-and-partnership-judgement.md`](06-vendor-and-partnership-judgement.md) — Four vendor decision classes (LLM API mix, foundation-model provider, cloud provider, MLOps vendor); reversibility class as the first move (Type 1 / Type 2 doors); utility-vs-strategic (Fowler / Wardley); the five-question framework; LLM-vendor routing-layer pattern; the non-negotiable written exit contract; the Finance / Legal signatories.
7. [`07-sustaining-the-technical-bar.md`](07-sustaining-the-technical-bar.md) — Capstone. The ten-module arc as one job; the archetype from mod-401 exercise-02 as ongoing sustaining constraint (Tech Lead / Architect / Solver / Right Hand strategies); the two drift patterns (IC-only, management-only); the quarterly personal check-in; the technical bar as ongoing discipline; the artifact set the learner ends the track holding.

## Exercises

- [`exercises/exercise-01-multi-quarter-ml-program-roadmap.md`](exercises/exercise-01-multi-quarter-ml-program-roadmap.md) — Author the multi-quarter roadmap for a real portfolio using the chapter-2 five-column template. 3 hours.
- [`exercises/exercise-02-executive-audience-rfc-portfolio.md`](exercises/exercise-02-executive-audience-rfc-portfolio.md) — Rewrite a portfolio decision as an exec one-pager and pair it with a working-backwards PR/FAQ for one new program. 3 hours.
- [`exercises/exercise-03-staff-plus-hiring-loop-calibration.md`](exercises/exercise-03-staff-plus-hiring-loop-calibration.md) — Design the rubric and slot allocation for a senior or staff ML hiring loop; run one debrief simulation. 3 hours.
- [`exercises/exercise-04-coach-a-senior-tech-lead-plan.md`](exercises/exercise-04-coach-a-senior-tech-lead-plan.md) — Author a coaching plan for a real (or realistically-composed) L30 tech lead against the chapter-5 modes. 3 hours.
- [`exercises/exercise-05-vendor-and-partnership-judgement-doc.md`](exercises/exercise-05-vendor-and-partnership-judgement-doc.md) — Author a vendor-decision doc — LLM API mix, self-host vs. API, cloud provider, or MLOps vendor — with the chapter-6 five-question framework and a written exit contract. 2 hours.

## Structure

```
mod-410-staff-plus-technical-leadership/
├── 01-…md … 07-…md    lecture chapters
├── exercises/          per-exercise prompts (solutions live in the paired solutions repo)
├── labs/               long-form hands-on labs (planned)
├── quizzes/            knowledge checks (planned)
├── resources.md        curated external references
└── README.md           this file
```

## Suggested sequencing

Read chapters 1 and 2 back-to-back — chapter 1 sets the sustaining frame, chapter 2 lands the first concrete artifact (the roadmap). Take exercise-01 immediately after chapter 2. Read chapters 3 and 4 in one sitting — exec comms and hiring are the two most externally-visible staff activities and they reinforce each other on rubric discipline; take exercise-02 after chapter 3 and exercise-03 after chapter 4. Read chapter 5 (coaching) and take exercise-04 with a real (or realistically-composed) L30 in mind — this exercise leans heavily on the mod-401 chapter 5 partnership contract, so re-read that mod-401 chapter before authoring. Read chapter 6 and take exercise-05 against a real vendor decision on the learner's desk if possible — the exit contract is the load-bearing section. Read chapter 7 last, and treat it as a personal reflection more than a lecture — the archetype centre-of-gravity check-in should happen with the mod-401 exercise-02 write-up open in another window.

The five exercises and the mod-401 archetype self-assessment together form the input for the [`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/) capstone project. Learners planning to take project-403 should keep the exercises grounded on one consistent portfolio (typically the same portfolio used in project-401's mod-402 exercises).

## What comes next

- **[`project-403-staff-plus-tech-leadership-simulation`](../../projects/project-403-staff-plus-tech-leadership-simulation/)** — the leadership-simulation capstone that stitches this module's five exercises plus the mod-401 archetype pick into one staff-plus leadership portfolio.
- **[`principal-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning) (L50)** — the next-up role on the ML engineering ladder; org- and industry-wide technical strategy, external influence, keynote-grade authorship. The mod-401 chapter 1 defer-up boundary.
- **Sustained L40 practice.** For most learners the next two years are staying at L40 through several portfolios and technology waves. Chapter 7's quarterly personal check-in and the ten-artifact set from the ten-module arc are the sustaining scaffolding.

## Related tracks

- [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) — the peer L40 Engineering Manager track. Authors the EM-side mirror of the mod-401 chapter 5 partnership contract that this module's chapter 5 leans on for the coaching boundary. The Staff/EM pair signs one document from either side.
- [`senior-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/senior-ml-engineer-learning) (L30) — the prerequisite single-team tech-lead track; the L30 tech leads chapter 5 coaches are exactly the learners of that track.
- [`ai-infra-ml-platform-learning`](https://github.com/ai-infra-curriculum/ai-infra-ml-platform-learning), [`ai-infra-mlops-learning`](https://github.com/ai-infra-curriculum/ai-infra-mlops-learning), [`ai-infra-performance-learning`](https://github.com/ai-infra-curriculum/ai-infra-performance-learning) — the peer platform tracks whose paved-road roadmap this module's chapter 2 depends on and whose vendor-selection conversation this module's chapter 6 partners with.
- Peer specialist tracks — `training-pipeline-engineer`, `fine-tuning-engineer`, `model-evaluation-engineer`, `ai-eval-engineer`, `nlp-engineer`, `rag-engineer`, `llm-application-developer`, `applied-ai-engineer` — the IC-depth specialists chapter 5's coaching mode 2 (pair then withdraw) applies to when they are on a portfolio team.

# The Four Staff-Plus Archetypes, Applied to ML

## Motivation

The previous chapter defined *staff-plus altitude* — the object-of-the-verb shift from one system to a portfolio, program, or platform contract. That is a necessary condition, but it does not tell the engineer *which shape of staff role* they are actually in. Two Staff ML Engineers at the same company at the same level can spend their week on genuinely different work — one spends it embedded in a modeling team defending its architecture, one spends it roaming from firefight to firefight across the ML org, one spends it as the Director's second brain.

Will Larson's *[Staff Engineer](https://staffeng.com/book)* names four archetypes that cover almost every staff-plus IC job in industry: **Tech Lead**, **Architect**, **Solver**, and **Right Hand**. Reading the archetypes literally — before you ever apply them to yourself — is how you avoid the two most common Staff-scope traps: (1) accepting a role labelled "Staff Engineer" that is actually a different archetype than the one you signed up for, and (2) trying to grow into all four archetypes at once and clearing the bar for none.

This chapter defines each archetype in Larson's own vocabulary, then translates each into concrete ML-flavoured artifacts and failure modes. The self-assessment exercise (exercise-02) later asks the learner to place themselves against these four, so the definitions must be precise.

## Where the archetypes come from

Larson developed the four-archetype taxonomy by interviewing staff-plus engineers across Stripe, Slack, Google, Netflix, Fastly, Squarespace, and others. The interview corpus is public at [StaffEng.com/stories](https://staffeng.com/stories/) and is the recommended background reading for this chapter. The archetypes are described in chapter 1 of the book, "Overview of the Staff Engineer Role."

A few caveats before we translate them to ML:

- **The archetypes are not exclusive.** Most staff engineers spend most of their time in one archetype and sometimes work in an adjacent one. The self-assessment is about *centre of gravity*, not about a single label.
- **The archetypes are not levels.** A Right Hand is not "more senior" than a Tech Lead; they are the same L40 bar in a different shape.
- **The archetypes are not roles you interview for.** Job postings say "Staff Machine Learning Engineer" — they rarely say "Staff Solver". The archetype is a lens for reading what the posting is *actually asking for* under the title. Chapter 3 drills into that reading skill.

## Archetype 1 — Tech Lead

### Larson's definition

The **Tech Lead** archetype pairs with an engineering manager to guide the execution of a single team (or a small cluster of closely-related teams). They are the most common staff archetype and the one closest to the Senior-tech-lead role the engineer was likely promoted from — but their scope is wider and their time horizon longer than the Senior's.

### The ML translation

A Staff ML Tech Lead sits inside one modeling team (or a tightly-coupled cluster: "search" = ranking + retrieval + query understanding, or "personalization" = recsys + user embedding + feature layer) and owns:

- The **modeling roadmap** for that team over two-to-four quarters, defensible to the peer L40 EM and to the Director.
- The **architecture RFCs** for the team's biggest bets, and the code-review bar that keeps those RFCs from drifting.
- The **hiring loop** for that team's senior ML hires — calibrating the bar with the EM, coaching the interviewers.
- The **partnership contract** with the platform teams the modeling team consumes from and the specialist teams it delegates to (mod-405, chapter 4).

**Working example.** A Staff ML Tech Lead on the marketplace-search team spends their week: (1) reviewing the new retrieval architecture RFC and pushing back on the eval plan; (2) running the roadmap review with the EM and the Director; (3) sitting in three onsite loops for a senior retrieval hire; (4) pairing with a mid-level engineer on the LLM-judge integration; (5) reading the last quarter's postmortems for the recommender team next door because their retrieval regressions leaked into search's ranking metric.

### The failure mode

The Tech Lead archetype is the easiest place to *stall* at Staff altitude, because the day-to-day work looks almost identical to Senior tech-lead work. The stall signal: the engineer is still primarily judged on *this quarter's model quality* rather than on *the two-quarter modeling program*. The fix is not to work harder — it is to widen the artifact set (roadmap, hiring calibration, cross-team contract) until the L30 work becomes a sub-set of the L40 work.

## Archetype 2 — Architect

### Larson's definition

The **Architect** archetype is responsible for the direction, quality, and long-term technical approach within a specific critical area of the business. They typically work across many teams within that area rather than embedding in one. They are recognised as the durable technical authority on the area — engineers three levels below know to escalate to them.

### The ML translation

A Staff ML Architect owns *a technical area that spans several ML teams* and is the durable authority on it. The area is usually named after a technical substrate rather than a product team:

- Ranking and retrieval across every surface that runs a ranker.
- The org's foundation-model training program (mod-403 / project-402).
- The org's cross-team eval program (mod-406).
- The distributed-training strategy that every team's training run inherits (mod-404).
- The ML platform consumer contract (mod-405).
- Portfolio-scope AI risk and safety (mod-409).

The Architect's dominant artifact is the **multi-team architecture blueprint or RFC** (mod-402). They are the person who can say "these three ranking systems can share one retrieval substrate; this fourth one needs its own because of latency" and defend it against pushback from three peer team leads. They are the person the CTO asks *"why do we have three model registries?"* and gets a real answer from.

**Working example.** A Staff ML Architect for the ranking area spends their week: (1) reading the last month's ranker RFCs from the four ranking teams and writing a two-page consolidation memo; (2) authoring the shared retrieval substrate RFC and running the review with the four team leads and the ML platform EM; (3) partnering with the peer L40 EM (for the ML platform team) on the delivery plan; (4) publishing the ranker eval standard so every ranking team ships against the same metrics; (5) sitting in a director staff meeting to defend the substrate consolidation cost.

### The failure mode

The Architect archetype's failure mode is **losing the connection to running code**. An Architect who has not read a training run's logs in six months, and whose RFCs are "correct in principle but impossible to build", loses credibility fast. Larson names this as the reason staff engineers should keep at least one hand on execution — a small implementation stake in one of the systems they architect keeps the RFCs honest. The counter-failure is running to overcorrect and becoming an over-scoped Tech Lead: still shipping code, no longer authoring the area.

## Archetype 3 — Solver

### Larson's definition

The **Solver** archetype takes on the arbitrarily complex problem the organisation is stuck on, digs in, and produces a real answer. They roam. They are the person the Director assigns to *"the pretraining loss is diverging and none of the three teams that touch it can figure out why."* Their team, this quarter, is whichever problem needs them most.

### The ML translation

A Staff ML Solver is on-call for the org's expensive, ambiguous problems. Common ML-flavoured shapes:

- The pretraining program's loss diverged at step 40k and none of the ablations reproduce the divergence in isolation.
- The recommender's revenue metric is regressing over months in a way no single experiment can identify.
- Two production models produce contradictory scores on the same input and the ambiguity has become a customer-facing bug.
- A model regulator (EU AI Act, US state actuarial, financial fair-lending) has raised a specific question and no one on the ML org owns the answer.
- The GPU capacity plan for the next quarter is wrong by an unclear amount and the FinOps + Infra + ML views disagree.

The Solver's dominant artifact is the **investigation writeup**: a report that a director, the peer teams, and the future Solver can read and act on. It looks like a very good postmortem, except the "incident" is a multi-team ambiguity rather than a page.

**Working example.** A Staff ML Solver spends their quarter: (1) six weeks investigating why the mixture-of-experts pretraining run's loss stalls at a specific step across three cluster generations; (2) two weeks pairing with the training-pipeline peer specialist to reproduce a data-corruption failure mode; (3) two weeks authoring the investigation writeup, the fix RFC, and the failure-budget update; (4) two weeks embedded with the retrieval team on an unrelated regression the Director escalated.

### The failure mode

The Solver archetype's failure mode is **staying too long on any one problem**. A Solver who accidentally becomes the Tech Lead of the team that owns the problem they were dispatched to solve has lost the archetype. The mitigation is a hand-off ritual: every Solver engagement ends with a written hand-off contract to the team that will own the solution long-term.

## Archetype 4 — Right Hand

### Larson's definition

The **Right Hand** archetype extends an executive's attention. They borrow the executive's authority to operate at a scale a normal IC could not, and they return decision-quality analysis to that executive. They are effectively the executive's *technical second brain* on the ML organisation.

### The ML translation

A Staff ML Right Hand is paired with the Director of ML (or in smaller companies, the VP or CTO) and owns:

- Reading, condensing, and pushing back on every consequential technical RFC in the ML org before it reaches the executive.
- Authoring the executive's one-page framing of a technical decision for the leadership team.
- Sitting in staff meetings as the technical voice when the Director is elsewhere.
- Owning the cross-team investigations the Director does not have time to run.
- Standing in for the Director on the technical parts of external partnerships (vendor calls, regulator conversations, board-level ML risk framing).

The Right Hand's dominant artifact is the **executive-audience one-pager** — a two-page synthesis that lets a director make a $10M capacity call or a merge/split decision without personally reading the ten-page underlying RFC. It is the most compressed, highest-stakes writing on the ML ladder.

**Working example.** A Staff ML Right Hand to the Director of ML spends their week: (1) synthesising the six pretraining-program RFCs into one two-page briefing before Monday's exec review; (2) sitting in the peer L40 EM staff meeting when the Director is on the road; (3) drafting the ML org's next-quarter capacity ask for the CFO's team; (4) running a private investigation on a compliance question the Director does not want to escalate until it is resolved; (5) coaching two Tech-Lead-archetype Staff engineers on the roadmap the Director has committed to leadership.

### The failure mode

The Right Hand archetype's failure mode is **losing the technical bar**. A Right Hand who becomes a scheduling assistant or a comms surrogate for the Director has drifted into chief-of-staff work, which is a real and valuable job — but is not a staff-plus IC job on an ML engineering ladder. The countermeasure is the same as for the Architect: keep some execution stake, keep reading the RFCs at technical depth, keep the code-review bar.

## Which archetype does your organisation actually hire?

Not every organisation hires all four archetypes. Larson observes that:

- **Tech Lead** is the most common archetype at every company size.
- **Architect** is common in mature ML orgs with three or more modeling teams sharing substrates.
- **Solver** is common in orgs with expensive ambiguous programs — foundation-model pretraining, autonomy stacks, fintech fraud — and rare in smaller applied-ML teams.
- **Right Hand** is common in AI-native labs and in larger big-tech ML orgs with executive layers deep enough to need one; rare in fintech and small applied-ML teams.

Chapter 3 shows how to read a specific posting for its archetype. Exercise-02 asks the learner to place their current role, their target role, and their organisation against the four.

## Cross-checking with other frameworks

Two industry frameworks converge with Larson's four archetypes and are worth reading as triangulation:

- **[Engineering Ladders](https://www.engineeringladders.com/)** describes staff+ engineers along five axes (technology, system, people, process, influence). The four archetypes map roughly onto different weightings of those axes: the Tech Lead is people/system-heavy, the Architect is technology/system-heavy, the Solver is technology-heavy with situational people, the Right Hand is influence/process-heavy.
- **[progression.fyi](https://www.progression.fyi/)** aggregates public engineering-career frameworks from Meta, Rent the Runway, Monzo, Buffer, and others. Every framework past L40 differentiates staff+ engineers by scope-shape rather than by seniority alone — the archetype vocabulary is a specific case of that pattern.

Neither framework is the *canonical* one; the whole point of publishing multiple is that org-specific ladders differ. Read Larson first, then read either one to check that your understanding is not company-specific.

## Common mistakes when applying the archetypes

- **Treating them as a career ladder.** They are not. A Solver is not "more advanced" than a Tech Lead. Most Staff engineers *rotate through* archetypes over a career rather than climbing a ladder of them.
- **Trying to hold two at once at full weight.** A Staff engineer who is trying to be both the Architect for the whole ranking area *and* the Tech Lead of one ranking team full-time will burn out or drop one silently. Pick one centre of gravity; keep an eye on the second.
- **Confusing archetype with title.** Every archetype fits under the title "Staff ML Engineer". The title on a posting is not evidence of which archetype the org actually wants. Chapter 3 reads for the archetype behind the title.
- **Confusing Right Hand with chief of staff.** A Right Hand is a technical IC. If the day-to-day is calendaring, comms, and administrative execution, it is chief-of-staff work — a real job, but not a Staff ML Engineer job.

## Summary

- Will Larson's four archetypes — **Tech Lead, Architect, Solver, Right Hand** — cover almost every staff-plus IC role in industry. All four are L40 altitude; they differ in *shape*, not in level.
- **Tech Lead** — embeds with one modeling team, owns the two-to-four-quarter roadmap and the hiring bar. Failure mode: staying at Senior scope.
- **Architect** — owns a durable technical area across teams (ranking, training program, eval, platform contract, portfolio-scope safety). Failure mode: losing touch with running code.
- **Solver** — roams into arbitrarily complex ML problems (loss divergence, revenue regressions, compliance questions, capacity ambiguity) and returns a defensible investigation. Failure mode: staying too long on one problem.
- **Right Hand** — extends the Director's technical attention through executive-audience synthesis, exec-meeting presence, and cross-team investigations. Failure mode: drifting into chief-of-staff work.
- Most Staff engineers have one dominant archetype and one adjacent one. Trying to hold all four at full weight is a burnout trap.

The next chapter takes these four archetypes off the page and shows how to spot them in the wild — how to read a real job posting for the staff-defining verbs, and how to distinguish them from Senior verbs and from EM verbs.

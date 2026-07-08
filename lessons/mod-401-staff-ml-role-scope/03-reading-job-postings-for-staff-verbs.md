# Reading Job Postings for the Staff-Defining Verbs

## Motivation

Job postings are the most-abused primary source in the promotion-and-hiring conversation. Almost every posting titled "Staff Machine Learning Engineer" contains a mix of Senior verbs, Staff verbs, and EM verbs — because postings are written by hiring managers who are working from templates, from a peer's posting they liked, and from the latest exec-briefing on what they want the ML org to do. A posting can be titled "Staff" and read as Senior once the verbs are extracted; another can be titled "Senior Staff" or "Machine Learning Lead" and read as a Right Hand.

This chapter teaches one specific skill: **extract the verbs and their objects from a posting, and classify each verb-object pair as Senior, Staff, or EM.** After this chapter, a real posting takes about fifteen minutes to score. Exercise-01 puts the skill on live postings.

## Why "the verb and its object" is the right unit of analysis

Job postings say what work will happen and who will do it. Every sentence in the requirements section has that shape — an implicit or explicit verb, plus what the verb acts on:

> *"You will [own] [the ranking model that powers marketplace search]."*
>
> — verb: **own**; object: **the ranking model**. Object scope: **one system on one team**. Verdict: **Senior tech-lead**.

> *"You will [architect] [the retrieval, ranking, and personalization substrate across the four consumer surfaces teams]."*
>
> — verb: **architect**; object: **a substrate across four teams**. Object scope: **portfolio**. Verdict: **Staff Architect**.

> *"You will [grow and manage] [a team of 8-12 ML engineers]."*
>
> — verb: **grow and manage**; object: **people**. Verdict: **Engineering Manager**.

The title of the posting does not settle any of these. The verbs settle them. Job-posting analysis for level fit is *entirely* a mechanical extraction exercise, and the payoff of doing it mechanically is you stop being fooled by inflated titles.

## The Senior / Staff / EM verb classes

Below is the working taxonomy the module uses. It is not exhaustive — postings invent phrasings — but every real phrasing lands cleanly in one of the three columns when the object is checked.

### Senior tech-lead verbs (L30 — single-team scope)

| Verb | Typical object | Why this is L30, not L40 |
|---|---|---|
| own | *the ranking model* / *the fraud model* / *the recommender* | Single system on a single team. Scope is not multi-team. |
| lead | *the ranking team's modelling roadmap* | Single-team roadmap. Signal is missing the multi-team qualifier. |
| author | *the model card* / *the training pipeline RFC* | Artifact for one system. |
| run | *the eval harness* / *the retraining pipeline* | Operates on one team's system. |
| mentor | *mid-level engineers on the team* | Mentorship scope is one team. |
| ship | *the next iteration of the model* | Sprint-to-quarter time horizon; single artifact. |
| debug | *the training-serving skew* on the model | Single-system diagnostic. |

If a posting is 90% these verbs against these objects, it is a Senior tech-lead posting. That is not a bad posting — it just is not L40.

### Staff-defining verbs (L40 — multi-team, program, or platform-contract scope)

| Verb | Typical object | Why this is L40 |
|---|---|---|
| architect across teams | *a shared substrate for retrieval and ranking across surfaces* | Object crosses team boundaries. Architect archetype. |
| set (multi-quarter) direction | *the modelling roadmap for the next three quarters that leadership commits to* | Time horizon is two-to-four quarters. |
| own the paved-road contract | *the consumer contract between ML modeling teams and the ML platform team* | Contract between org units. Right Hand or Architect archetype. |
| sponsor the RFC | *the org-scope build-vs-adopt decision on the eval platform* | Sponsorship — not just authorship — of a decision that binds multiple teams. |
| calibrate senior engineers | *the L30 hiring loop across the modelling org* | Multi-team hiring bar. Tech-Lead or Architect archetype in the hiring dimension. |
| partner with executives | *the Director of ML, VP of Product, and CFO on quarterly capacity and program framing* | Executive-audience scope. Right Hand archetype. |
| shape the roadmap | *the multi-quarter ML program roadmap* | Program-level scope, multi-quarter horizon. |
| unblock the org | *cross-team ambiguities the Director escalates* | Solver archetype. |
| define the standard | *the org-wide LLM-as-judge evaluation policy* | Standard binds multiple teams. |
| coach senior tech leads | *the four L30 tech leads across the modelling org* | Coaching operates *above* mid-level scope. |
| author the risk register | *the portfolio-scope AI risk register that the CISO signs off on* | Portfolio scope, executive-audience artifact. |
| defend the TCO | *the ML org's cluster budget in front of the CFO* | Executive-audience decision. |
| lead multi-team incident command | *the ranking-plus-retrieval regression incident* | Multi-team ops. |

The Staff verbs share three properties: their object is *multi-team, portfolio, program, or platform-contract*; their time horizon is *two-to-four quarters*; and their consumer surface is *peer teams, peer platform teams, the peer L40 EM, and executives* — not just the engineer's own team.

### Engineering Manager verbs (peer L40, people-leadership path)

| Verb | Typical object | Why this is EM, not IC-Staff |
|---|---|---|
| hire | *N ML engineers this quarter* | Headcount ownership, not calibration. |
| grow | *a team of X engineers* | Direct reports. |
| retain | *the team* | Retention is HR/people accountability. |
| manage | *N direct reports* | 1:1s, performance management. |
| do performance reviews | *for the team* | HR-owned people process. |
| own headcount | *for the modelling team* | Budgeted people slots. |
| set OKRs / commit for the team | *quarterly goals* | Manager cadence. |
| resolve interpersonal conflict | *between engineers* | People-manager accountability. |
| coach / develop | *individual engineers on their career* | This is EM career-development — distinct from the Staff verb "coach senior tech leads on technical direction". |

If a posting contains a material fraction of these verbs, it is either an EM posting mislabelled as Staff, or a hybrid role that will trap the incumbent. The peer L40 EM track is [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning); that is where those verbs belong.

## Watch for the modifier — the same verb can be Senior or Staff

Several verbs — **own**, **lead**, **author**, **run** — appear at all three levels. The *modifier that widens the object* is what settles the level. Read each verb with its object, not in isolation.

| Verb | Senior object | Staff object |
|---|---|---|
| own | *the ranking model* | *the ranking, retrieval, and personalization portfolio* |
| lead | *the fraud team's retraining pipeline* | *the ML org's build-vs-adopt call on the eval platform* |
| author | *the model card for the recommender* | *the AI risk register the CISO signs off on* |
| run | *the eval harness for the search team* | *the cross-team experiment-review body* |
| debug | *training-serving skew on the recommender* | *the cross-team loss-divergence investigation the Director escalated* |

A posting that keeps its verbs at the Senior-object side of this table is a Senior posting, whatever the title says.

## Two more diagnostics: time horizon and consumer

Two additional signals disambiguate borderline verbs.

### Time-horizon signal

Any phrase in the posting that talks about *"this quarter"*, *"the next sprint"*, or *"the current model iteration"* pushes toward Senior. Any phrase that talks about *"multi-quarter direction"*, *"the two-to-four-quarter roadmap"*, or *"the ML program leadership committed to"* pushes toward Staff. If neither shows up, the posting is under-specified and the recruiter is the next place to check.

### Consumer signal

Who reads the artifacts the engineer will produce? If the answer is *"the team"* and *"the EM"*, it is Senior. If the answer includes *"the Director"*, *"peer L40 EMs"*, *"the CFO on capacity"*, *"the CISO on AI risk"*, *"the peer platform teams' consumer contract"*, it is Staff. If the answer is dominated by *"HR"*, *"direct reports"*, and *"headcount"*, it is EM.

## Worked example — decomposing a synthetic posting

The following is a synthetic posting written for this chapter. It is not from any specific company. It exists to demonstrate the extraction procedure; real postings should be scored in exercise-01.

> **Staff Machine Learning Engineer, Ranking Platform**
>
> - Own the ranking model for the marketplace search team.
> - Lead the cross-surface retrieval strategy across marketplace search, feed, and notifications.
> - Author the shared eval standard that every ranking team ships against.
> - Partner with the ML platform team to define the model-registry consumer contract.
> - Coach two senior ML engineers on their team's roadmap.
> - Mentor mid-level engineers on the ranking team.
> - Grow the ranking team's headcount by four in the next fiscal year.
> - Sit on the L30 hiring loop and calibrate the bar.

Extraction:

| Verb | Object | Class |
|---|---|---|
| own | *the ranking model for the marketplace search team* | **Senior** — single system, single team. |
| lead | *the cross-surface retrieval strategy across marketplace search, feed, and notifications* | **Staff (Architect)** — multi-team object. |
| author | *the shared eval standard that every ranking team ships against* | **Staff (Architect)** — org-standard artifact. |
| partner with | *the ML platform team* to *define the model-registry consumer contract* | **Staff (Architect / Tech Lead consumer contract)**. |
| coach | *two senior ML engineers* | **Staff (Tech Lead / Architect coaching)** — coaching L30s specifically. |
| mentor | *mid-level engineers on the ranking team* | **Senior** — mid-level mentorship inside one team. |
| grow | *the ranking team's headcount by four* | **EM** — headcount ownership. |
| calibrate | *the bar* on the L30 hiring loop | **Staff** — bar-setting for peer L30 hires. |

The score: 5 Staff verbs, 2 Senior verbs, 1 EM verb. The Senior verbs are compatible with the Staff Tech-Lead archetype (Larson explicitly notes that Tech-Lead-archetype staff engineers keep an execution stake). The single EM verb — *grow headcount by four* — is the red flag. It is not a Staff verb; if it stays in the posting it will either eat the incumbent's week or reveal itself as a hybrid Staff/EM role that will not clear either bar. This is the specific question to raise with the recruiter before signing.

Verdict: **this posting is a legitimate Staff Tech-Lead-archetype role with one EM contamination**. Fit is realistic if the EM verb is either removed or explicitly split with a peer EM.

## Common posting anti-patterns to recognise

Once you have scored a dozen postings, patterns emerge. Four to name explicitly:

- **The "Senior-plus-plus" posting.** Titled Staff, verbs all L30, object of every verb is a single system. Common at smaller companies that use Staff as a comp-band adjustment. Fine if the comp lands, but does not clear the Staff bar for future promotions elsewhere.
- **The Staff/EM hybrid posting.** Contains a full Staff verb set *and* a full EM verb set. Common at growing companies that did not want to split the headcount. High burnout risk; the incumbent will drop one side silently.
- **The "we want a Right Hand but didn't say so" posting.** Reports to the VP or Director directly (rare below Staff), mentions "exec briefings", "quarterly capacity plan for the CFO", "external partnership calls". Read as Right Hand.
- **The "we want a Solver but didn't say so" posting.** Vague on scope, heavy on words like *"complex, ambiguous problems"*, *"multi-team investigations"*, *"deep dives across the ML org"*. Read as Solver.

## Reading beyond the requirements section

The requirements bullets are the highest-signal section, but three other sections carry information the verb list does not:

- **Reports to.** A Staff engineer who reports to a Senior EM is almost certainly Tech-Lead-archetype. One who reports to the Director or VP is Right-Hand-adjacent.
- **You will work with.** A Staff engineer whose "you will work with" list includes only their own team's members is scoped narrower than a Staff posting should be. A list that includes peer L40 EMs, the ML platform team, and executives is scoped correctly.
- **What you have done.** Look at *"you have shipped"*, *"you have architected"*, *"you have led"* on the *past* side. If the past-tense requirements are all single-system and single-team, the posting is a Senior posting whose title was inflated. Real Staff postings require past cross-team, program-scope, or platform-contract work.

## Summary

- The extraction procedure: pull every verb and its object from the posting's requirements, then classify each pair as Senior, Staff, or EM.
- Senior verbs act on *one system on one team*. Staff verbs act on *a portfolio, program, or platform contract across teams* and have a *two-to-four-quarter horizon*. EM verbs act on *people and headcount*.
- Several verbs — own, lead, author, run, debug — appear at all levels. The object and the time horizon settle the level; the title does not.
- Time-horizon and consumer signals disambiguate borderline verbs.
- Anti-patterns to name explicitly: Senior-plus-plus, Staff/EM hybrid, unspoken Right Hand, unspoken Solver. Recognising them protects the engineer from signing up for a role that will not clear the Staff bar.

Exercise-01 puts this extraction procedure on live postings. The next chapter — on hand-off contracts to peer specialist and peer platform tracks — assumes the learner can now name the scope a posting is asking for and turns to what the Staff engineer *keeps* versus what they *hand off*.

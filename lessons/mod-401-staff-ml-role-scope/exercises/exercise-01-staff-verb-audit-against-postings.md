# exercise-01: Staff Verb Audit Against Postings

**Estimated effort:** 2 hours

## Objective

Sharpen the ability to read a job posting for its *actual* scope by extracting every verb-and-object pair, classifying each pair as Senior (L30), Staff (L40 IC), or EM (L40 people-leadership), and defending the overall verdict. After this exercise the learner should be able to score any Staff-titled posting in about fifteen minutes and identify which of Larson's four archetypes the posting is really asking for.

## Prerequisites

- Chapter 03 — Reading Job Postings for the Staff-Defining Verbs.
- Chapter 02 — The Four Staff-Plus Archetypes (for the archetype classification step).
- Chapter 01 — What "Staff-Plus" Altitude Actually Means (for the L30/L40/L50 altitude reference).

## Materials to collect

Collect **five real, in-window (past 90 days) job postings**. Every posting must be a *current* public listing that the learner can link to directly. Use the ATS aggregators listed in [`../../JOB_REQUIREMENTS.md`](../../JOB_REQUIREMENTS.md) — `job-boards.greenhouse.io`, `jobs.lever.co`, `jobs.ashbyhq.com` — plus the careers pages of the employer coverage set enumerated there.

Aim for the following distribution across the five, in order to make the classification skill work harder:

1. **One posting titled "Staff Machine Learning Engineer"** that is unambiguously Staff-scoped (the "easy positive"). Bigger, established ML orgs are the most reliable source.
2. **One posting titled "Staff Machine Learning Engineer"** that is *actually* Senior-scoped once the verbs are extracted (the "Senior-plus-plus" anti-pattern from chapter 3). Growing-stage companies often publish these.
3. **One posting titled "Senior" or "Machine Learning Lead"** that is *actually* Staff-scoped (the "unspoken Staff" case).
4. **One posting titled "Staff Machine Learning Engineer"** with a material EM contamination — headcount, direct reports, or 1:1s explicit in the requirements.
5. **One posting for a peer L40 EM role** — titled "Engineering Manager, Machine Learning" or "Machine Learning Manager" — as the counter-example that helps the classifier calibrate the EM verb list.

Do **not** use Research Scientist, Applied Scientist, or Staff Data Scientist postings — those belong to peer research tracks.

## Steps

1. **Collect and archive.** For each of the five postings, capture: employer, exact title, URL, date observed, location(s), and a full copy of the requirements + preferred-qualifications sections. Save each posting into your working notes.
2. **Extract.** For each requirements bullet, extract the *verb* and the *object* the verb acts on. If a single bullet contains multiple verb-object pairs, split it into rows. Aim for 6-12 rows per posting.
3. **Classify each verb-object pair** as one of: `L30-senior`, `L40-staff`, `L40-em`, or `ambiguous`. Use the taxonomy from chapter 3 as the primary reference and cite the row of the taxonomy each classification maps to.
4. **Score each posting.** Report the ratio of Staff:Senior:EM rows. State the verdict (which shape the posting *is*, regardless of the title). If Staff-scoped, name the most likely archetype using chapter 2's four (Tech Lead / Architect / Solver / Right Hand).
5. **Note the anti-patterns.** For each posting where the verdict does not match the title, name the anti-pattern (Senior-plus-plus, Staff/EM hybrid, unspoken Staff, unspoken Right Hand, unspoken Solver).
6. **Write the summary comparison.** In one page, compare the five postings side by side. Which title-to-scope alignments were surprising? What common verb templates showed up across postings? Which anti-patterns are most common in your slice?

## Deliverable

A single document (markdown, 4-6 pages) with:

- **Section 1** — a header table listing the five postings (employer, title, URL, date observed, location).
- **Section 2** — for each posting, a verb-object extraction table with the classification and archetype tag, plus a 2-3 sentence verdict.
- **Section 3** — a one-page comparative summary.
- **Section 4** — three sentences of personal reflection: given what these five postings actually ask for, what does this change about how you would target your next role?

Do not include any candidate personal information from the postings themselves and do not include the postings' verbatim text beyond the requirements bullets you are analysing.

## Starter guidance

- **Extract mechanically before classifying.** If you start classifying rows before you have finished extraction, you will implicitly wash the object of the verb toward whatever level you already expected. Extract every verb-object pair first, then classify.
- **Watch modifiers, not verbs.** As chapter 3 stresses, verbs like "own", "lead", and "author" appear at every level. The object modifier — "the ranking model" vs. "the ranking, retrieval, and personalization portfolio" — is what settles it.
- **Do not over-count "ambiguous".** More than 20% of rows landing in the `ambiguous` bucket is a signal you have not disambiguated aggressively enough. Force yourself onto one of the three level buckets whenever the object is present in the text.
- **Distinguish "coach" verbs carefully.** "Coach senior tech leads on technical direction" is a Staff verb. "Coach individual engineers on career growth" is an EM verb. The direct object matters.
- **Keep the sample honest.** If four out of your five postings turn out to be Senior in disguise, do not add a sixth to pad the "clean Staff" side. The point of the exercise is to *see* the misalignment between titles and scope.

## Acceptance criteria

- Five real, in-window postings collected and archived, each with URL and date observed.
- Every requirements bullet from every posting is decomposed into at least one verb-object row (usually more).
- Each row carries an explicit classification and, where applicable, an archetype tag.
- Each posting has an explicit verdict stating (a) whether the title matches the scope, (b) which archetype the posting is asking for, and (c) which anti-pattern (if any) it exhibits.
- The comparative summary identifies at least one Senior-plus-plus posting *or* one unspoken Staff posting *or* one Staff/EM hybrid — that is, at least one title-to-scope misalignment is identified and defended.
- The personal reflection makes at least one commitment about how the learner will now target their next role differently.

## Stretch goals

- Score **ten postings** rather than five, and produce a rough frequency histogram over the staff-defining verbs in your slice. Which verbs appear most often? Which archetypes dominate your target market?
- Contrast your slice against a slice from a different segment (AI-native labs vs. big tech vs. consumer tech vs. fintech, per the coverage set in `JOB_REQUIREMENTS.md`). Do the archetype distributions differ? If so, what does that say about which segment fits your target archetype from exercise-02?
- Author a **one-page hiring-manager rubric** the *reverse* way — given the verb taxonomy, draft the requirements bullets you would write if you were hiring for each of the four archetypes. This is the input to a Staff-plus hiring rubric in mod-410.
- Score the *reports-to* line and the *you will work with* line for each posting (chapter 3, "Reading beyond the requirements section") and compare their signal to the verb extraction. Which section was the highest-signal for your slice?

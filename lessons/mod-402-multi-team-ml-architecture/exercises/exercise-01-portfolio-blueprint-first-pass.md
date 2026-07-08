# exercise-01: Portfolio Blueprint First Pass

**Estimated effort:** 3 hours

## Objective

Draw an honest, current-state blueprint of a real three-to-five-system ML portfolio using the chapter-2 template. After this exercise the learner should hold a portfolio × substrate matrix, a cross-system dependency diagram, and a list of the cells where consolidation, sharing, or splitting is materially painful — the input to exercises 02, 03, and 04.

The exercise is deliberately about *current state*. Do not draw a target-state architecture and do not propose fixes. Chapters 3-6 and exercises 02-04 handle that.

## Prerequisites

- Chapter 01 — From One System to a Portfolio (portfolio scope, the four substrates, the "am I doing portfolio work?" heuristic).
- Chapter 02 — The Portfolio Blueprint (the matrix template, cell labels, seven-question interview, worked example).
- Chapter 03 skim — the four failure modes, so that the pain-list at the end of the exercise carries the right vocabulary.

## Choose your portfolio

Pick one of the following, in preference order:

1. **Your current employer.** Three-to-five ML systems in one product line whose team leads you can interview. This is the highest-signal option and is required if you plan to feed this exercise into project-401.
2. **A prior employer, less than 18 months ago.** Same constraint on three-to-five systems. The interviews become memory + former-colleague slack messages rather than fresh interviews.
3. **A public reference architecture.** Uber's Michelangelo posts, Netflix's Metaflow-plus-Data-Platform posts, Airbnb's Bighead / Zipline posts, LinkedIn's ProML posts, or a similar corpus. This is the fallback if you are between employers or bound by NDA. If you use this option, cite the source of each claim in the blueprint's notes column so the assumption trail is visible.

Do not fabricate a fictional company. The exercise's value is in the messiness of a real portfolio; a fictional company will produce a suspiciously clean matrix that teaches nothing.

## Steps

1. **Name the systems.** Write down the three-to-five ML systems. For each: business object (what it optimises for), owning team, on-call rotation, upstream data source class, downstream consumer. One paragraph per system. Do not proceed until this list is stable — swapping systems in later will re-do most of the work.
2. **Run the seven-question interview per team.** If your portfolio choice is the public-reference option, extract the equivalent answers from the reference documents and mark unknowns as `unknown` (not `TBD`). The seven questions are enumerated in chapter 2. Take notes; you will reference them again in exercise 02.
3. **Draw the portfolio × substrate matrix.** Rows are systems; columns are the four substrate layers (data planes, feature layer, model registry, eval contract). Each cell records the concrete implementation. Aim for two-to-three sentences per cell.
4. **Label every cell.** Assign each cell one of `shared`, `duplicated`, `forked`, `unique-by-design`. Do not use `TBD` or `mixed`; if you need `mixed`, split the cell's contents into per-property labels in the notes column.
5. **Draw the cross-system dependency diagram.** Rectangles for systems, ovals for shared substrate nodes, edges labelled `writes` / `reads` / `co-owns`. Aim for a single-page diagram — Excalidraw, graphviz, or a hand-drawn scan.
6. **List the painful cells.** Pull the `duplicated` and `forked` cells into a table, one row each, with columns: system pair, substrate layer, one-sentence description of the pain (drift observed, ownership unclear, tooling mismatch, migration blocker).
7. **Write the ownership-ambiguity note.** In one paragraph, name every substrate node in the dependency diagram that has more than one writer or unclear ownership. This paragraph is the bridge to exercise 02.

## Deliverable

A single document, 4-6 pages plus the diagram, containing:

- **Section 1** — Portfolio overview (one paragraph per system).
- **Section 2** — Per-team interview notes (one sub-section per team, seven-question format).
- **Section 3** — The portfolio × substrate matrix.
- **Section 4** — The cell-label table with the notes column.
- **Section 5** — The cross-system dependency diagram (embedded or linked).
- **Section 6** — The painful-cells table.
- **Section 7** — The ownership-ambiguity paragraph.
- **Section 8** — Three-sentence personal reflection: what surprised you between the interviews and the picture you had of the portfolio before the exercise?

Do not include any confidential per-team code, credentials, or PII. Blueprint claims should sit at the substrate-layer level, not at the per-model-artefact level.

## Starter guidance

- **Interview before drawing.** Learners are tempted to sketch the matrix from memory and then confirm with the teams. That inverts the point. The interviews reliably contradict the memory picture; draw only after.
- **Be strict about the difference between `shared` and `parallel`.** Two teams consuming the same event stream but each running their own filtered projection are *parallel*, not shared. `shared` means one materialised implementation both teams depend on. Chapter 2 is precise on this.
- **Do not sanitise the messy cells.** If Team B's model-registry answer is "it's in a spreadsheet", write that down. The blueprint's value comes from the honest picture, not the flattering picture. Nobody's promotion depends on this document being clean.
- **Do not fix as you go.** If exercise-01 produces "and here is how we should merge these", you have started exercise-03 early and short-circuited the diagnostic in exercise-02. Keep the blueprint current-state until the exercise is done.
- **Time-box the interviews.** 45 minutes per team, no more. Longer interviews wander and produce lower-signal notes than a tighter 45-minute conversation.

## Acceptance criteria

- Three-to-five ML systems named, each with business object, owning team, and data-source class recorded.
- Seven-question interview run against each team (or extracted against the public reference), with notes preserved.
- Matrix drawn with a specific implementation named in every cell — not left as `TBD`.
- Every cell carries exactly one of the four labels; no `mixed` and no `TBD`.
- Cross-system dependency diagram drawn, single page, edges labelled with direction, substrate nodes and system nodes visually distinct.
- Painful-cells table has at least two entries; more is normal for a real portfolio.
- Ownership-ambiguity paragraph names every substrate node with more than one writer or an unclear owner. If it names zero, the paragraph is wrong — go back and check.
- Personal reflection makes at least one specific observation about how the interview picture differed from the pre-interview picture.

## Stretch goals

- Extend the matrix to **five substrate columns** by adding the *serving-side* substrate — the online-inference stack each system runs against (Triton / TorchServe / Ray Serve / homegrown). Note whether the serving layer is shared or duplicated. This is often the first place platform-team peers want to consolidate; catching it here makes mod-405 smoother.
- Author a **one-page portfolio one-pager** — the exec-audience summary of the blueprint. What are the three-to-five systems, what is their combined business impact, and what are the two-to-three highest-pain substrate cells you plan to author RFCs against. This is a draft of the executive brief that pairs with the RFC in exercise 04.
- **Diff your blueprint against a public reference architecture.** Pick the reference closest to your portfolio (Uber Michelangelo, Airbnb Bighead, Meta AI infrastructure posts) and note the three biggest differences. Which are because your org is smaller? Which are because your org made a different substrate call? Which look like historical accident that a merge or share would clean up? This is preparation for the failure-mode diagnosis in exercise 02.

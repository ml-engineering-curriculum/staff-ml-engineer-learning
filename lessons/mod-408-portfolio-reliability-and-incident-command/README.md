# mod-408-portfolio-reliability-and-incident-command: Portfolio-Scope ML Reliability, SLOs, and Multi-Team Incident Command

> Scaffolded by `aicg org execute-plan`. Lecture chapters and exercise content are authored on subsequent autonomous cycles.

**Estimated effort:** 12 hours

## Learning objectives

- Standardise ML-specific SLI/SLO vocabulary across a portfolio — prediction quality drift, retraining SLA, feature-freshness, calibration decay — and set the required SLIs every team's model must ship with
- Author the severity matrix for ML incidents at portfolio scope; distinguish silent-failure incidents (quality drift no one paged for) from user-visible incidents
- Run multi-team incident command using Google SRE incident-management structure — Incident Commander, Ops Lead, Comms Lead — adapted to ML failure classes (bad predictions, silent drift, feature-pipeline breakage, retraining-loop stall)
- Author the postmortem template the ML org adopts, including a portfolio-lessons section that the next incident should not repeat
- Set the retraining-as-deploy hygiene the platform enforces — canary path, rollback, offline replay — as a contract every team's model registry entry must satisfy

## Structure

- `01-…md` … `0N-…md`: lecture chapters.
- `exercises/`: per-exercise prompts.
- `labs/`: long-form hands-on labs.
- `quizzes/`: knowledge checks.
- `resources.md`: external references.

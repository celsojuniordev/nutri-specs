# Design

## Context

This is the first change in a greenfield specs repository (`nutri-specs`). There is no existing codebase, backend, or frontend yet — these specs are the source that will later drive separate frontend and backend implementation specs/repos. See proposal.md - Why for the motivation.

The four capabilities (`nutritionist-auth`, `patient-management`, `diet-prescription`, `physical-assessment`) share one domain model and are being specified together because they depend on each other end to end: a patient belongs to a nutritionist, and both diet plans and physical assessments belong to a patient.

## Goals / Non-Goals

**Goals:**
- Establish a single, consistent domain model (Nutritionist, Patient, Diet Plan, Physical Assessment) that later frontend/backend specs can build on without re-deriving domain rules.
- Keep every capability's data scoped to the owning nutritionist (multi-tenant by nutritionist).
- Keep the physical assessment protocol concrete (7-site Pollock skinfolds + a fixed set of circumferences) so downstream specs have an unambiguous data shape to implement against.

**Non-Goals:**
- Not specifying calculated body-composition metrics (e.g., body fat percentage formulas) — only raw measurement capture and comparison are in scope for this change.
- Not specifying the PDF's visual layout/branding — only its required content (patient, nutritionist, meals, food items, quantities).
- Not specifying password reset, email verification, or multi-factor authentication flows — only registration, login, logout, and per-nutritionist data isolation.
- Not specifying frontend or backend implementation technology choices — that belongs to the downstream frontend/backend specs this repo will generate later.

## Decisions

- **Ownership model**: Every Patient record is owned by exactly one Nutritionist; every Diet Plan and Physical Assessment is owned by exactly one Patient (and transitively by that patient's nutritionist). This keeps the authorization rule in `nutritionist-auth` simple and uniform across capabilities: a nutritionist may only read/write records that chain back to their own account.
- **Assessment protocol fixed to 7-site Pollock + standard circumferences**: chosen (per user decision) over an open/flexible field list so the spec gives downstream implementations a concrete, testable data shape instead of an arbitrary key-value structure. Alternative considered: fully flexible user-defined measurement fields — rejected for this change because it would push the data-modeling decision downstream and make comparison behavior harder to specify precisely.
- **Deactivation instead of deletion for patients**: patients are soft-deactivated (per `patient-management`) rather than deleted, so a patient's diet and assessment history is never lost even if the nutritionist stops actively treating them.
- **Diet plan history is append-only from the patient's perspective**: updating a diet plan edits it in place (per `diet-prescription`); creating a new prescription over time is what produces history, mirroring how physical assessments accumulate history. This keeps the two capabilities' history semantics consistent.

## Risks / Trade-offs

- [Fixed assessment protocol may not match every nutritionist's practice] → Accepted for this version per explicit user decision; a future change can widen the model if needed.
- [PDF export requirements are content-only, not visual] → Downstream frontend/backend specs will need to add layout/branding decisions; flagged as a non-goal here rather than left implicit.
- [No password-recovery/account-security flows specified yet] → Acceptable for an initial core-features change; should be scoped explicitly as a follow-up change before production use.

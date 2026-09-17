# Tasks

## 1. Specification Validation

- [ ] 1.1 Run `openspec validate add-nutritionist-core-features --strict` and verify it reports no errors
- [ ] 1.2 Cross-review the four spec deltas (`nutritionist-auth`, `patient-management`, `diet-prescription`, `physical-assessment`) together and verify no contradicting requirements exist between them (e.g., ownership rules stated in `nutritionist-auth` match how `patient-management`, `diet-prescription`, and `physical-assessment` describe access to their records)
- [ ] 1.3 Verify every requirement in all four spec deltas has at least one success-path scenario and at least one rejection/error-path scenario, and add any missing scenario found

## 2. Domain Model Consolidation

- [ ] 2.1 Document the consolidated domain model (Nutritionist, Patient, Diet Plan, Meal, Food Item, Physical Assessment) and the ownership chain between them, and verify every field named in the document traces back to a specific requirement in one of the four spec deltas
- [ ] 2.2 Document the required fields and validation rules for each entity (patient required fields; assessment required weight and positive-value rule for weight/skinfolds/circumferences; diet plan's meal/food-item structure and PDF content requirements), and verify each rule cites the requirement it comes from

## 3. Downstream Handoff Readiness

- [ ] 3.1 Run `openspec status --change add-nutritionist-core-features` and verify `proposal`, `specs`, `design`, and `tasks` all report status `done`
- [ ] 3.2 Confirm this change's proposal, specs, and design are sufficient to start generating the downstream frontend and backend specs (per the domain model and validation rules documented in section 2) and record any gap found for a follow-up change

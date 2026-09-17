# Spec Delta

## Purpose

Allow a nutritionist to prescribe a diet plan ("receita") for a patient, made up of meals and food items, and to hand that plan to the patient as an exported PDF document.

## ADDED Requirements

### Requirement: Create Diet Plan
The system SHALL allow an authenticated nutritionist to create a diet plan for a patient they own, composed of one or more meals, each containing one or more food items with a quantity and unit of measure.

#### Scenario: Successful diet plan creation
- **WHEN** an authenticated nutritionist submits a new diet plan for a patient they own, including at least one meal with at least one food item
- **THEN** the system creates the diet plan associated with that patient and confirms creation

#### Scenario: Diet plan without meals rejected
- **WHEN** an authenticated nutritionist submits a new diet plan with no meals
- **THEN** the system rejects the request and indicates that at least one meal is required

### Requirement: Update Diet Plan
The system SHALL allow an authenticated nutritionist to update a diet plan they created, including adding, editing, or removing meals and food items.

#### Scenario: Successful diet plan update
- **WHEN** an authenticated nutritionist submits changes to a diet plan they own
- **THEN** the system saves the updated meals and food items and returns the updated diet plan

### Requirement: Diet Plan History per Patient
The system SHALL allow an authenticated nutritionist to view the list of diet plans previously created for a patient they own, ordered by creation date.

#### Scenario: View diet plan history
- **WHEN** an authenticated nutritionist requests the diet plans for a patient they own
- **THEN** the system returns all diet plans created for that patient, ordered from most recent to oldest

### Requirement: Export Diet Plan as PDF
The system SHALL allow an authenticated nutritionist to export a diet plan they own as a PDF document containing the patient's name, the nutritionist's name, and every meal with its food items and quantities.

#### Scenario: Successful PDF export
- **WHEN** an authenticated nutritionist requests the PDF export of a diet plan they own
- **THEN** the system generates a PDF document listing the patient's name, the nutritionist's name, and all meals with their food items and quantities, and makes it available for download

#### Scenario: Export of another nutritionist's diet plan denied
- **WHEN** an authenticated nutritionist requests the PDF export of a diet plan owned by a different nutritionist
- **THEN** the system denies the request

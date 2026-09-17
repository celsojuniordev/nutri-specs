# Spec Delta

## Purpose

Allow a nutritionist to record a patient's physical assessment measurements over time and compare assessments against each other to evidence the patient's progress.

## ADDED Requirements

### Requirement: Record Physical Assessment
The system SHALL allow an authenticated nutritionist to record a dated physical assessment for a patient they own, capturing the patient's weight, the seven skinfold measurements of the Pollock protocol (triceps, subscapular, midaxillary, chest/pectoral, suprailiac, abdominal, and thigh), and body segment circumferences (arm, forearm, chest, waist, abdomen, hip, thigh, and calf).

#### Scenario: Successful assessment recording
- **WHEN** an authenticated nutritionist submits a new physical assessment for a patient they own, including the assessment date and weight
- **THEN** the system creates the assessment record with all submitted measurements associated with that patient

#### Scenario: Missing weight rejected
- **WHEN** an authenticated nutritionist submits a new physical assessment without a weight value
- **THEN** the system rejects the request and indicates that weight is required

#### Scenario: Negative or zero measurement rejected
- **WHEN** an authenticated nutritionist submits a physical assessment containing a negative or zero value for weight, a skinfold, or a circumference
- **THEN** the system rejects the request and indicates which measurement is invalid

### Requirement: Assessment History per Patient
The system SHALL allow an authenticated nutritionist to view the full chronological history of physical assessments recorded for a patient they own.

#### Scenario: View assessment history
- **WHEN** an authenticated nutritionist requests the physical assessment history for a patient they own
- **THEN** the system returns all recorded assessments for that patient, ordered from oldest to most recent

### Requirement: Compare Two Assessments
The system SHALL allow an authenticated nutritionist to select two physical assessments of the same patient and view the difference between them for weight, each skinfold, and each circumference.

#### Scenario: Successful comparison
- **WHEN** an authenticated nutritionist selects two physical assessments belonging to the same patient they own
- **THEN** the system returns, for weight, each skinfold, and each circumference, the value from each selected assessment and the difference between them

#### Scenario: Comparison across different patients denied
- **WHEN** an authenticated nutritionist attempts to compare two physical assessments that do not belong to the same patient
- **THEN** the system rejects the request

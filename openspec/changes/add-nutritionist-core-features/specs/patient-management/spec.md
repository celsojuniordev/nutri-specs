# Spec Delta

## Purpose

Allow a nutritionist to register and manage the patients under their own care, keeping each patient's basic and contact information up to date and searchable.

## ADDED Requirements

### Requirement: Patient Registration
The system SHALL allow an authenticated nutritionist to register a new patient with at minimum a full name, birth date, and sex, associating the patient with that nutritionist.

#### Scenario: Successful patient registration
- **WHEN** an authenticated nutritionist submits a new patient's full name, birth date, and sex
- **THEN** the system creates the patient record owned by that nutritionist and confirms creation

#### Scenario: Missing required field rejected
- **WHEN** an authenticated nutritionist submits a new patient without a required field (full name, birth date, or sex)
- **THEN** the system rejects the request and indicates which field is missing

### Requirement: Patient Listing and Viewing
The system SHALL allow an authenticated nutritionist to list and view the details of the patients they own.

#### Scenario: List own active patients
- **WHEN** an authenticated nutritionist requests their patient list
- **THEN** the system returns only active patients owned by that nutritionist

#### Scenario: View patient detail
- **WHEN** an authenticated nutritionist requests the detail of a patient they own
- **THEN** the system returns that patient's registered information

### Requirement: Patient Update
The system SHALL allow an authenticated nutritionist to update the registered information of a patient they own.

#### Scenario: Successful update
- **WHEN** an authenticated nutritionist submits updated information for a patient they own
- **THEN** the system saves the updated information and returns the updated patient record

### Requirement: Patient Deactivation
The system SHALL allow an authenticated nutritionist to deactivate a patient they own without deleting the patient's historical data, and SHALL exclude deactivated patients from the default active patient list.

#### Scenario: Deactivate a patient
- **WHEN** an authenticated nutritionist deactivates a patient they own
- **THEN** the system marks the patient as inactive, removes them from the default active patient list, and retains the patient's diet plans and physical assessment history

#### Scenario: Reactivate a patient
- **WHEN** an authenticated nutritionist reactivates a previously deactivated patient they own
- **THEN** the system marks the patient as active again and includes them in the default active patient list

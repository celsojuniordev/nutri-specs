# Spec Delta

## Purpose

Provide nutritionist account registration and authentication so each nutritionist can securely access the system and only ever see and manage their own patients and data.

## ADDED Requirements

### Requirement: Nutritionist Registration
The system SHALL allow a new nutritionist to create an account by providing at minimum a full name, a unique email address, and a password.

#### Scenario: Successful registration
- **WHEN** a visitor submits a registration form with a full name, an email not already in use, and a valid password
- **THEN** the system creates a nutritionist account and confirms the account was created

#### Scenario: Duplicate email rejected
- **WHEN** a visitor submits a registration with an email address that already belongs to an existing nutritionist account
- **THEN** the system rejects the registration and returns an error indicating the email is already in use

### Requirement: Nutritionist Login
The system SHALL allow a registered nutritionist to authenticate using their email and password and receive an authenticated session.

#### Scenario: Successful login
- **WHEN** a nutritionist submits their correct email and password
- **THEN** the system grants an authenticated session and allows access to the nutritionist's own data

#### Scenario: Invalid credentials rejected
- **WHEN** a nutritionist submits an email/password combination that does not match a registered account
- **THEN** the system rejects the login attempt and does not create a session

### Requirement: Nutritionist Logout
The system SHALL allow an authenticated nutritionist to end their session.

#### Scenario: Successful logout
- **WHEN** an authenticated nutritionist requests to log out
- **THEN** the system invalidates the current session so it can no longer be used to access data

### Requirement: Per-Nutritionist Data Isolation
The system SHALL scope every patient, diet plan, and physical assessment to the nutritionist who created it, and SHALL only allow an authenticated nutritionist to read or modify records they own.

#### Scenario: Nutritionist cannot access another nutritionist's patient
- **WHEN** an authenticated nutritionist requests a patient, diet plan, or physical assessment owned by a different nutritionist
- **THEN** the system denies access and does not return that record's data

#### Scenario: Unauthenticated access denied
- **WHEN** a request to view or modify a patient, diet plan, or physical assessment is made without a valid authenticated session
- **THEN** the system denies the request

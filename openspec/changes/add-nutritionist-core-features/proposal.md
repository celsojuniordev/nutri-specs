# Proposal

## Why

Nutritionists currently manage patient records, diet plans, and physical assessment history through disconnected tools (spreadsheets, paper forms, generic documents). This change defines the core system specs needed to give a nutritionist a single system to register patients, prescribe diets that can be handed to patients as a PDF, and track physical assessment measurements over time to evidence results. These specs are the foundation this repo will later use to generate frontend and backend implementation specs in other repositories.

## What Changes

- Add nutritionist accounts with authentication; each nutritionist can only see and manage their own patients (multi-tenant).
- Add patient registration and management (create, view, update, deactivate patients) scoped to the owning nutritionist.
- Add diet prescription: a nutritionist can create a diet/meal plan ("receita") for a patient, composed of meals and food items, and export it as a PDF document.
- Add physical assessment tracking: record weight, skinfold measurements (7-site Pollock protocol), and body segment circumferences for a patient, keep a full history of assessments, and compare any two assessments to surface change over time.

## Capabilities

### New Capabilities
- `nutritionist-auth`: Nutritionist account registration, login/authentication, and session-based access control so each nutritionist only accesses their own data.
- `patient-management`: Registering, viewing, updating, and deactivating patients under a specific nutritionist.
- `diet-prescription`: Creating, updating, and PDF-exporting a diet/meal plan ("receita") for a patient.
- `physical-assessment`: Recording physical assessment measurements (weight, 7-site skinfolds, circumferences) per patient, retaining history, and comparing assessments over time.

### Modified Capabilities
None — this is the first change in the project; no existing specs to modify.

## Impact

- Establishes the initial domain model for the system: Nutritionist, Patient, Diet Plan (Receita), Meal/Food Item, Physical Assessment.
- No existing code, APIs, or systems are affected — this is a greenfield specification effort that will later drive frontend and backend implementation in downstream repositories.

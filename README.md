# THREAD Prototype

This repository contains a working web prototype of THREAD, anchored at BFI. The prototype is implemented as two connected single-page HTML applications that share a browser-side data model and audit log:

- `index.html` - Agency Portal
- `bfi-system.html` - BFI Internal Control Center

Together, the two applications demonstrate the full THREAD protocol flow: agency onboarding, classification negotiation, Scope of Exchange execution, dataset transfer, internal BFI ingestion, cataloging, pipeline tracking, AI sandbox governance, and audit review.

## Purpose

This is not a production-ready system. It is a research artifact intended to:

- Demonstrate how THREAD's architecture, governance roles, approval gates, and data lifecycle stages can be represented in an operational interface.
- Surface protocol design decisions that are easier to evaluate through interaction, including classification discussion threads, approval gates, audit organization, and Scope of Exchange artifacts.
- Support stakeholder review by giving BFI staff and leadership a concrete system to inspect, critique, and iterate against.

## Applications

### Agency Portal

The Agency Portal is the protocol entry point for a disclosing agency. It guides an agency through a dataset onboarding workflow:

1. Organizational information
2. Dataset metadata and sensitivity inputs
3. Classification tier negotiation
4. Scope of Exchange review and signature
5. Data transfer

The classification screen is the central workflow. The portal computes an initial tier recommendation from the dataset sensitivity inputs, but the agency can accept or override the recommendation. If there is disagreement, the agency Data Owner and BFI Data Steward use a structured discussion thread. Messages, proposed tier changes, disputes, final determinations, and agency overrides are retained as part of the dataset audit trail.

The Scope of Exchange is generated as a structured document with sections for parties, dataset identification, classification, authorized purpose, data controls, AI/ML governance where applicable, governance roles, breach notification, retention, termination, amendment, and federal compliance where applicable. Sections can be reviewed, edited, approved, signed, and registered into the shared control-plane state.

### BFI Internal Control Center

The BFI Internal Control Center is the operational interface for BFI staff. It contains six THREAD-aligned views:

- Dashboard - active Scopes of Exchange, pending gates, recent activity, and alerts.
- Ingestion - onboarding state, classification review, Scope of Exchange status, signatures, transfer status, and chain-of-custody receipts.
- Catalogue - registered datasets with classification, metadata, roles, controls, and Scope of Exchange links.
- Pipeline - validation reports, transformation logs, labeling status, storage, and stage advancement.
- AI Sandbox - training authorization records, model status, linked datasets, and evaluation/governance metadata.
- Audit Log - a unified event timeline and artifact view organized across agency-facing, control-plane, data-plane, AI sandbox, and system layers.

## Shared Data Model

The two applications communicate through browser `localStorage`. The BFI Control Center seeds the demo state, and the Agency Portal writes onboarding, classification, Scope of Exchange, and audit artifacts into the same `thread_*` storage keys.

Important shared keys include:

- `thread_datasets`
- `thread_audit_events`
- `thread_soe_edits`
- `thread_classification_threads`
- `thread_custody_receipts`
- `thread_validation_reports`
- `thread_transformation_ops`
- `thread_training_authorizations`
- `thread_models`
- `thread_onboarding_progress`

Because this state is browser-local, the prototype is best evaluated in a single browser profile and origin.

## Seed Data

The prototype includes realistic San Antonio agency contexts across all three classification tiers:

- Public Works: `SA Public Works Service Calls 2020-2025`, Tier 2, active, AI/ML authorized.
- Information Technology: `311 Service Requests Q1-Q4 2025`, Tier 1, active, AI/ML authorized.
- Human Services: `Housing Assistance & Income Verification Records`, Tier 3, active, no AI/ML authorization.
- Planning & Community Development: `Development Permits & Capital Investment Activity 2019-2025`, Tier 2 draft with an in-progress classification disagreement.

The repository also includes `sa_development_permits_2019_2025_sample.csv`, a sample dataset corresponding to the Planning & Community Development scenario.

## How To Run

No build step is required. The prototype is static HTML, CSS, and JavaScript.

For the most reliable shared `localStorage` behavior, serve the directory from a local web server:

```sh
python3 -m http.server 8000
```

Then open:

- Agency Portal: `http://localhost:8000/index.html`
- BFI Control Center: `http://localhost:8000/bfi-system.html`

The two apps include navigation controls for moving between the Agency Portal and BFI Control Center during the demo flow.

## Suggested Demo Flow

1. Open `bfi-system.html` first to seed the THREAD demo data.
2. Open `index.html` and use the Agency Portal to inspect or resume the Planning & Community Development draft.
3. Review the auto-suggested classification and the structured classification discussion.
4. Switch to the BFI Control Center and inspect the same classification thread from Ingestion.
5. Continue through Scope of Exchange review, signature, and data transfer.
6. Return to the Control Center to inspect the resulting dataset, custody records, pipeline status, AI sandbox records, and audit artifacts.

## Resetting The Demo

Both applications include a reset control that clears the browser-local THREAD state and restores the original seeded data. Resetting removes wizard drafts, signatures, audit events, generated receipts, and other local changes.

If needed, state can also be cleared manually through the browser developer tools by removing the `thread_*` `localStorage` keys.

## Repository Files

- `index.html` - Agency-facing data partnership portal.
- `bfi-system.html` - BFI internal THREAD Control Center.
- `audit_trail_artifacts.html` - supporting audit artifact prototype.
- `sa_development_permits_2019_2025_sample.csv` - sample Planning & Community Development dataset.



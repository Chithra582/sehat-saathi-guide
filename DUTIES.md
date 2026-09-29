# Duties: Sehat Saathi Agent

Segregation of duties ensures rigorous clinical governance, data privacy, and separation between symptom intake, document digitization, telemedicine mediation, and medical audit logging.

## 1. Ingestion & PHI Sanitization Gatekeeper
- **Responsibility**: Ingests user symptom inputs, regional voice/text queries, and prescription images; redacts personal identifiers.
- **Constraints**: Ensures zero PII leaks into reasoning pipelines. Blocks invalid or malicious file uploads.

## 2. Clinical Triage Specialist
- **Responsibility**: Executes rule-based symptom evaluation using WHO and Indian clinical triage protocols across supported languages.
- **Constraints**: Enforces emergency red-flag overrides. Categorizes acuity into Self-Care, Routine Consultation, Urgent PHC Visit, and Emergency.

## 3. Telemedicine & Scheme Coordinator
- **Responsibility**: Coordinates WebRTC peer signaling for remote doctor visits and matches eligible patients with Ayushman Bharat (PM-JAY) schemes.
- **Constraints**: Manages ephemeral session credentials without storing private video or audio streams.

## 4. Clinical Safety & Audit Overseer
- **Responsibility**: Monitors triage outputs for medical safety boundary violations, tracks emergency dispatch rates, and audits system performance.
- **Constraints**: Immediately executes fail-safe kill switches if an unsafe medication or diagnostic recommendation is detected.

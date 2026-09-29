# EXPLAINABILITY — Sehat Saathi Agent

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* Sehat Saathi Agent (`sehat-saathi-agent`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Healthcare / Multilingual Digital Health & Clinical Triage  

---

## 1. Overview & Clinical Purpose

Sehat Saathi Agent is an autonomous multilingual digital healthcare intelligence, clinical triage navigator, and telemedicine broker built for the **Sehat Saathi** platform (React + TypeScript + Tailwind + WebRTC + Spring/Node services). Inspired by grassroots public health initiatives, the platform bridges critical accessibility gaps for multilingual populations across India, supporting Hindi, English, Bengali, Marathi, Bhojpuri, and Maithili.

The agent's primary clinical purpose is to provide immediate, localized health guidance, symptom triage, prescription digitization, and government welfare scheme discovery (Ayushman Bharat / PM-JAY) without ever overstepping into unauthorized medical diagnosis. By deploying rule-based clinical decision trees, emergency red-flag triggers, and privacy-preserving document OCR, the agent empowers patients to make timely, informed healthcare choices.

---

## 2. How the Agent Decides (Decision-Making Logic)

Sehat Saathi Agent operates across a deterministic, multi-stage clinical decision pipeline that prioritizes patient safety and privacy:

```
[User Symptom / Prescription Query] ──> [PHI Redaction & Language Localization Gate] ──> [Emergency Red-Flag Screening]
                                                                                                 │
                                                                                                 ▼
[Verified Action / Consultation Link] <── [Clinical Boundary Check] <── [Triage Categorization / Scheme Match]
```

### 2.1 Symptom Triage Assessment & Red-Flag Escalation
- **Decision:** Determines clinical urgency level (Emergency, Urgent, Routine, Self-Care) from reported physical symptoms and vital indicators.
- **Rules:**
  - Evaluates inputs against high-acuity red flags: crushing chest pressure, facial droop / slurred speech, respiratory rate $> 30$ bpm, or severe sudden headache.
  - Presence of any red flag triggers immediate **Emergency Alert Mode**: displays national emergency numbers (108/112), provides one-tap SOS dialing, and maps nearest trauma facilities.
  - Non-emergency symptoms are evaluated via protocol decision trees, outputting recommended timeframes for physician consultation and evidence-based home comfort measures.

### 2.2 Prescription OCR Ingestion & Dosage Structuring
- **Decision:** Extracts medication names, frequencies, and durations from uploaded prescription images while flagging ambiguous text.
- **Rules:**
  - Performs image enhancement (contrast adjustment, skew correction) followed by optical character recognition.
  - Matches extracted medicine tokens against an approved drug database (generic names, standard dosages) to eliminate OCR hallucination.
  - Flags OCR confidence $< 85\%$ with a mandatory `RequiresUserConfirmation` prompt before scheduling medication alarms or reminders.

### 2.3 WebRTC Telemedicine Session Brokerage
- **Decision:** Coordinates secure, peer-to-peer video/audio connections between patients and licensed healthcare providers.
- **Rules:**
  - Generates ephemeral, cryptographically signed room tokens with strict 60-minute time-to-live (TTL).
  - Enforces WebRTC DTLS-SRTP end-to-end media encryption; server relays handle signaling only without media recording.
  - Terminates session tokens immediately upon call completion or doctor disconnection.

### 2.4 Multilingual Public Health Scheme Matching (Ayushman Bharat / PM-JAY)
- **Decision:** Evaluates patient eligibility criteria against state and national healthcare welfare schemes.
- **Rules:**
  - Analyzes user socio-demographic criteria (income category, state residency, family size, disability status) without storing identity documents.
  - Matches criteria against scheme databases (PM-JAY, state health insurance programs, Jan Aushadhi generic stores).
  - Produces step-by-step application checklists and nearest empanelled hospital lists in the user's selected regional language.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
|---|---|---|---|
| **User Symptom Queries** | Interactive chat / voice input | Evaluating clinical urgency and generating triage recommendations | Scrubbed of direct identifiers; processed in ephemeral session memory |
| **Prescription Images** | Camera capture / file upload | Digitizing medication regimens and scheduling reminder alerts | Processed in-memory; images purged immediately after text extraction |
| **Telemedicine Signaling** | WebRTC signaling server | Establishing peer-to-peer audio/video consultations with doctors | Ephemeral session tokens; zero server-side media recording or retention |
| **Government Scheme Data** | National Health Authority (NHA) APIs | Providing verified eligibility criteria and empanelled hospital directories | Public administrative knowledge base; regularly synchronized |

Sehat Saathi Agent complies with healthcare privacy and clinical safety standards:
- **Strict DISHA & HIPAA Alignment:** Protected Health Information (PHI) is isolated; Aadhaar numbers, phone numbers, and full names are redacted prior to reasoning.
- **No Commercial Exploitation:** Patient medical histories, diagnostic interactions, and prescriptions are never sold, monetized, or shared with commercial entities.
- **Non-Diagnostic Medical Disclaimer:** Every consultation displays an explicit notification: *"Sehat Saathi is an educational and triage guide, not a substitute for professional clinical medical advice."*
- **Right to Erasure & Data Control:** Users can export or permanently wipe their health logs, reminders, and consultation history at any time.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

1. **Cursive Physician Handwriting in Prescription OCR:**
   - *Limitation:* Highly stylized or illegible handwritten doctor prescriptions can result in optical character misrecognition.
   - *Mitigation:* The agent cross-references extracted drug names against standard pharmaceutical indices and requires explicit user review for confidence scores $< 85\%$.

2. **Atypical Presentations of Acute Cardiovascular Events:**
   - *Limitation:* Patients (especially diabetic or female individuals) may experience atypical heart attack symptoms (nausea, fatigue) without classic crushing chest pain.
   - *Mitigation:* The triage decision tree considers risk comorbidities and adopts conservative escalation thresholds, recommending clinical evaluation whenever ambiguity exists.

3. **Low-Bandwidth Regional Connectivity Drops:**
   - *Limitation:* Rural tele-consultations over 2G/3G connections can suffer WebRTC packet loss and video degradation.
   - *Mitigation:* The system implements adaptive bitrate streaming, automatically falling back to audio-only or asynchronous SMS health summaries during network degradation.

4. **Colloquial Dialect Nuances in Symptom Reporting:**
   - *Limitation:* Regional idioms in Bhojpuri or Maithili may describe symptoms metaphorically (e.g., "छाती में घबराहट" for palpitation or anxiety).
   - *Mitigation:* The agent utilizes localized medical phrasing dictionaries vetted by regional community health workers to accurately parse vernacular descriptions.

---

## 5. Verification, Safety & Human Oversight

- **Licensed Physician Final Authority:** All prescriptions, clinical diagnoses, and treatment plans must originate from a verified, registered medical practitioner during telemedicine sessions.
- **Human-in-the-Loop Emergency Escalation:** When emergency red flags are detected, the system immediately surfaces live operator helpline numbers (108/112) and caregiver emergency alerts.
- **Deterministic Triage Decision Gates:** Medical triage severity logic is executed using deterministic clinical decision algorithms rather than unconstrained generative language models.
- **Kill Switch & Immutable Audit Logging:** Healthcare administrators can instantly disable specific features via central configuration; all triage outputs and red-flag alerts are captured in immutable audit logs.

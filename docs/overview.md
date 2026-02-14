# Laboratory Analysis System — Use Case Overview

## Purpose

This document describes an operator-focused laboratory workflow managed by FlowLab, covering the full lifecycle from patient registration and sample handling to exam validation, report generation, and secure delivery. All entities and examples are fictional.

---

## Actors

- Reception staff — patient registration and sample collection
- Triage staff — routes samples to the appropriate sectors and handles final disposal
- Sector technical staff — exam verification and release
- Lab manager — final approval and signature
- Interface workstation — device listener and data translator
- FlowLab server — processing, storage, workflow orchestration
- External hospital system — clinical system integration
- Patient — report recipient

---

## Key Concepts

### Interface

The Interface is workstation software directly connected to diagnostic devices. It continuously listens to device output, receives low-level machine messages, translates them into structured records, and inserts them into the FlowLab database (modeled as a MySQL-style environment).

This component bridges laboratory hardware and modern data systems.

---

### Patient ID and Sample Protocol

FlowLab separates patient identity from sample identity:

- **Patient ID** — persistent identifier tied to the individual  
  (public health number, military registry, insurance ID, etc.)
- **Sample Protocol** — unique identifier for each laboratory visit

When a returning patient provides their ID, the system retrieves existing records automatically, accelerating registration. The protocol links the collected sample to the patient and drives all downstream processing.

---

### Status Lifecycle

```
Pending → Released → Signed → Report Generated
```

- **Pending** — exam received from device
- **Released** — validated by sector staff
- **Signed** — approved by lab manager
- **Report Generated** — consolidated PDF created

---

### Sector

A functional laboratory group (Hematology, Biochemistry, Microbiology, etc.) responsible for validating specific exams.

---

## Preconditions

- Diagnostic devices are physically connected to dedicated interface workstations.
- Workstations are connected to the internal network and communicate with the FlowLab server.
- Interface software is configured for device communication and database submission.
- Users have appropriate credentials and roles.

---

## Primary Workflow (Happy Path)

1. Reception registers the patient or retrieves existing data using Patient ID.
2. A new sample protocol is created and linked to the patient record.
3. The collected sample is sent to triage.
4. Triage routes the sample to required diagnostic sectors.
5. Devices perform exams within their sectors.
6. Interface software captures device output and stores exam data as **Pending**.
7. Sector staff review and validate results → status becomes **Released**.
8. Once all required sectors complete validation, the lab manager signs results → **Signed**.
9. FlowLab merges sector outputs into a unified PDF report.
10. The report is published to the patient portal.
11. Exam data is optionally transmitted via REST integration with an external Hospital Main Information System.
12. After processing is complete, the sample returns to triage for disposal.

---

## Alternate Flows and Exceptions

- **Device parsing failure** — Interface flags data for manual review.
- **Out-of-range results** — staff follow lab procedure rules: annotate, repeat tests, or correct entries.
- **Incomplete sector validation** — manager approval is blocked until all required exams are released.

---

## Core Data Elements

### Patient Record

- Patient ID
- Name
- Sample protocol
- Collection timestamp
- Personal data bundle (administrative and demographic information)

### Exam Record

- Device ID
- Test code
- Measurement values
- Units and reference ranges
- Validation comments
- Timestamps

### Audit Trail

- User action history
- Modification timestamps
- Original device payload

---

## Non-Functional Considerations

- Secure transport (TLS)
- Role-based access control
- Reliable device-to-server communication
- Retry queues for transmission failures
- Full auditability and traceability

---

## Deployment Notes

- Interface workstations remain colocated with diagnostic devices.
- Monitoring should track device connectivity, queue backlog, and processing errors.
- Network reliability is critical to workflow continuity.

---

## Living Document Notice

This overview represents a generalized laboratory workflow. Sector definitions, device types, and policies should be adapted to local operational standards.

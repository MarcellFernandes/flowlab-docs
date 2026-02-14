```mermaid
flowchart LR

Devices[Diagnostic Devices]
Interface[Interface Workstation]
Server[FlowLab Server]
Database[(Central Database)]
Portal[Patient Portal]
Hospital[External Hospital System]

Devices --> Interface
Interface --> Server
Server --> Database
Server --> Portal
Server --> Hospital
```

# FlowLab System Diagrams

This section visualizes the FlowLab laboratory workflow using Mermaid diagrams.  
They reflect the operational flow described in the Overview and are designed for clarity, portfolio presentation, and technical documentation.

---

## 1 — System Data Architecture

This diagram shows how laboratory devices communicate with the FlowLab ecosystem.

```mermaid
flowchart LR

Devices[Diagnostic Devices]
Interface[Interface Workstation]
Server[FlowLab Server]
Database[(Central Database)]
Portal[Patient Portal]
Hospital[External Hospital System]

Devices --> Interface
Interface --> Server
Server --> Database
Server --> Portal
Server --> Hospital
```

**Conceptual flow**

- Devices generate exam data.
- Interface translates machine output into structured records.
- FlowLab processes, stores, and distributes results.
- External systems may receive data through REST integration.

---

## 2 — End-to-End Laboratory Workflow

This diagram represents the operational lifecycle of a patient sample.

```mermaid
flowchart TD

Reception[Reception Registration]
Protocol[Protocol Creation]
Triage[Triage Routing]
Device[Diagnostic Sector Device]
Interface[Interface Capture]
Pending[Status: Pending]
Review[Sector Staff Verification]
Released[Status: Released]
Manager[Lab Manager Approval]
Signed[Status: Signed]
Report[PDF Merge & Generation]
Portal[Patient Portal Delivery]
Hospital[Hospital System Integration]
Discard[Triage Disposal]

Reception --> Protocol
Protocol --> Triage
Triage --> Device
Device --> Interface
Interface --> Pending
Pending --> Review
Review --> Released
Released --> Manager
Manager --> Signed
Signed --> Report
Report --> Portal
Report --> Hospital
Report --> Discard
```

**Status lifecycle**

```
Pending → Released → Signed → Report Generated
```

---

## 3 — Sector Consolidation Flow

This diagram illustrates how multiple laboratory sectors converge into a single patient report.

```mermaid
flowchart TB

Hematology[Hematology Release]
Biochemistry[Biochemistry Release]
Microbiology[Microbiology Release]

Manager[Lab Manager Signature]
Merge[PDF Consolidation]
Portal[Patient Portal]

Hematology --> Manager
Biochemistry --> Manager
Microbiology --> Manager

Manager --> Merge
Merge --> Portal
```

**Operational logic**

- Each sector validates only its own exams.
- Manager approval ensures cross-sector accountability.
- A single unified report is generated per patient protocol.

---

## Diagram Philosophy

These diagrams prioritize:

- operational clarity
- real workflow representation
- technical communication
- portfolio readability

They are intentionally abstracted to protect sensitive implementation details while preserving architectural meaning.

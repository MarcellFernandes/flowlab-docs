# FlowLab Quickstart Guide

This quickstart provides a practical checklist to deploy FlowLab in a laboratory environment and validate the complete sample lifecycle — from patient registration to report delivery and external system integration.

The goal is to confirm that devices, Interface software, verification workflow, and report distribution operate correctly.

---

## 1 — Environment Requirements

Prepare the following components before deployment:

- A server or VM for the FlowLab backend (API + database).
- Network-connected workstations for each laboratory sector.
- Diagnostic devices connected to workstations (serial or network).
- Interface software package for device communication.
- User accounts with appropriate roles:
  - Reception staff
  - Sector technical staff
  - Lab manager
- Network access to the patient portal and optional hospital integration endpoint.

---

## 2 — Deploy the FlowLab Server

- Install the FlowLab backend on the central server.
- Configure database connectivity (assume MySQL-compatible environment).
- Enable TLS encryption for API communication.
- Configure logging, monitoring, and database backup routines.
- Verify API accessibility from laboratory workstations.

---

## 3 — Configure Interface Workstations

For each workstation connected to diagnostic devices:

- Install Interface software.
- Configure device parsers for each supported model.
- Define FlowLab API endpoint and credentials.
- Enable local retry queue and audit logging.
- Validate device connectivity and parser output.

The Interface must successfully translate device messages into structured exam records.

---

## 4 — Register a Test Patient and Protocol

- At Reception, create a test patient record.
- Assign or retrieve a patient identifier (existing or new).
- Generate a protocol/sample ID.
- Confirm patient data is linked to the protocol.

This validates patient identity mapping and sample tracking.

---

## 5 — Execute a Full Sample Workflow Test

Simulate or run a real test exam:

1. Send the sample to triage.
2. Route the sample to a diagnostic device sector.
3. Allow the device to produce exam output.
4. Confirm the Interface submits a record with status `Pending`.

Then validate workflow progression:

- Sector technical staff review and mark result `Released`.
- Lab manager signs the result (`Signed`).
- FlowLab generates or merges the patient PDF report.
- Confirm report availability in the patient portal.
- Confirm REST transmission to the hospital system (if enabled).
- Verify sample lifecycle closure at triage.

---

## 6 — Validate Status Lifecycle

Ensure the exam transitions correctly:

```
Pending → Released → Signed → Report Generated
```

Any deviation indicates workflow or permission misconfiguration.

---

## 7 — Troubleshooting Checklist

If issues occur:

**No device data received**

- Verify serial/network connections.
- Inspect Interface logs.
- Confirm parser configuration.

**Parsing or ingestion failures**

- Validate device output format.
- Check Interface retry queue.
- Review FlowLab ingestion logs.

**Workflow block**

- Confirm user role permissions.
- Ensure required sector validation is complete.

**Report issues**

- Verify PDF generation logs.
- Confirm portal connectivity.

---

## 8 — Integration Validation (Optional)

If hospital system integration is enabled:

- Monitor REST payload transmission.
- Confirm receipt in the hospital information system.
- Validate patient record linkage.

---

## 9 — Recommended Practices

- Use synthetic test data — never real patient data.
- Capture device payload samples for parser verification.
- Monitor Interface queues for unexpected backlog.
- Document configuration changes for traceability.

---

## Outcome

A successful quickstart confirms:

- Device → Interface communication
- Correct status lifecycle execution
- Cross-sector validation workflow
- PDF report generation and merge
- Portal publication
- Optional hospital system integration

This establishes a verified baseline deployment for FlowLab in a laboratory environment.

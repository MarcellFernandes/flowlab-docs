# FlowLab — Use Case: Complete Example

This document is a portfolio-ready, end-to-end use case that shows how a laboratory exam flows from device capture to PDF report publication. It includes a synthetic device payload, API ingestion example, verification checks, a short audit query, and the recommended evidence to attach to a portfolio.

## Scenario

- Patient: John Silva
- Patient ID: PT-000123
- Protocol / Sample ID: PROT-20260214-0001
- Collection timestamp: 2026-02-14T09:12:00Z

Goal: validate the happy-path workflow from device output ingestion through sector validation, manager approval, PDF generation, and portal delivery.

---

## 1) Example device payload (synthetic)

```json
{
  "device_id": "DEV-HEM-001",
  "protocol": "PROT-20260214-0001",
  "patient_id": "PT-000123",
  "timestamp": "2026-02-14T09:13:21Z",
  "tests": [
    { "code": "HGB", "value": 13.6, "unit": "g/dL", "range": "12-16" },
    { "code": "WBC", "value": 6.2, "unit": "10^3/uL", "range": "4-11" }
  ]
}
```

---

## 2) Ingestion example (cURL)

Simulate the `Interface` posting an exam to the ingestion endpoint:

```bash
curl -X POST https://flowlab.internal/api/exams \
  -H "Authorization: Bearer <API_KEY>" \
  -H "Content-Type: application/json" \
  -d @device-payload.json
```

Expected response (200 OK):

```json
{
  "exam_id": "EXAM-20260214-0001",
  "status": "Pending"
}
```

---

## 3) Expected transitions and verification timeline

- `2026-02-14T09:13:21Z` — Ingested: `Pending` (verify `status` in API response)
- `2026-02-14T09:20:00Z` — Sector staff validate → `Released` (check audit trail)
- `2026-02-14T09:30:00Z` — Manager approves → `Signed`
- `2026-02-14T09:31:00Z` — PDF generated → `Report Generated` and published to portal

Quick verification steps:

- Confirm exam via `GET /api/exams/{exam_id}`
- Inspect `Interface` logs for `device_id` and `protocol`
- Verify report presence via `GET /api/reports/{protocol}`

Verification query example:

```sql
SELECT protocol, status, updated_at, changed_by
FROM exams
WHERE protocol = 'PROT-20260214-0001';
```

---

## 4) Summary

"This use case simulates the end-to-end laboratory exam lifecycle: from device capture through ingestion, sector validation, manager approval, and PDF publication. I modeled the device payload, defined state transitions, and documented verification steps including API examples and audit queries. The artifact shows how operational lab processes can be converted into clear, testable documentation suitable for system integrators and operations teams."

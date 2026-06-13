# C-005 - Personal HR metrics and escalation tickets

status: done
deps: C-004

## Scope

Implement the safe mock HRIS read adapter, `GET /api/v1/me/hr-metrics`, explicit escalation creation, and HR admin ticket list/update endpoints. Chat should call the HR metrics function for leave/insurance/reward-status questions and create an escalation when the request is outside AI authority.

## Allowed files

- `src/api/routes.py`
- `src/models/schemas.py`
- `src/services/hris.py`
- `src/services/tickets.py`
- `src/services/auth.py`
- `src/agents/**`
- `tests/test_api/test_hr_metrics.py`
- `tests/test_api/test_tickets.py`
- `tests/test_agents/test_function_calling.py`
- `tests/fixtures/**`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_hr_metrics.py tests/test_api/test_tickets.py tests/test_agents/test_function_calling.py`
- [x] Employee token gets only their own `PersonalHrMetrics`.
- [x] Employee cannot pass an arbitrary employee id to read another user's metrics.
- [x] Sensitive or complex chat request creates a ticket id and returns it in `escalated_ticket_id`.
- [x] HR admin can list and patch ticket status.
- [x] `/docs` shows all ticket and HR metrics endpoints with schemas.

## Done-evidence (world-state proof, named BEFORE building)

Paste metric lookup output, escalation chat output, admin ticket list output, and updated ticket output.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_hr_metrics.py tests/test_api/test_tickets.py tests/test_agents/test_function_calling.py
```

Output:

```text
tests\test_api\test_hr_metrics.py ... [ 30%]
tests\test_api\test_tickets.py .... [ 70%]
tests\test_agents\test_function_calling.py ... [100%]
10 passed in 0.24s
```

Full regression suite:

```text
36 passed in 0.78s
```

Local app verification:

```json
{
  "metrics_lookup": {
    "employee_id": "emp-001",
    "leave_days_remaining": 8.5,
    "insurance_status": "active",
    "reward_review_status": "in_review",
    "as_of_date": "2026-06-13"
  },
  "chat_escalation": {
    "escalated_ticket_id": "ticket-c06a6ec6-cad3-43e1-bbb0-8878501b1dc8",
    "refusal_reason": "sensitive",
    "actions": [
      {
        "type": "escalation_created",
        "label": "Escalation ticket created",
        "data": {
          "ticket_id": "ticket-c06a6ec6-cad3-43e1-bbb0-8878501b1dc8",
          "status": "open"
        }
      }
    ]
  },
  "admin_ticket_list": {
    "tickets": [
      {
        "id": "ticket-c06a6ec6-cad3-43e1-bbb0-8878501b1dc8",
        "requester_id": "emp-001",
        "status": "open",
        "priority": "high",
        "reason": "sensitive",
        "summary": "Cho toi xem luong cua Nguyen Van B"
      }
    ]
  },
  "updated_ticket": {
    "id": "ticket-c06a6ec6-cad3-43e1-bbb0-8878501b1dc8",
    "status": "in_progress",
    "assignee_id": "hr-001"
  },
  "docs_paths_present": [
    "/api/v1/me/hr-metrics",
    "/api/v1/escalations",
    "/api/v1/admin/tickets",
    "/api/v1/admin/tickets/{ticket_id}"
  ]
}
```

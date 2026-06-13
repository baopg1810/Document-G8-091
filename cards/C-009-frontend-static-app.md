# C-009 - Frontend chat and HR admin app

status: done
deps: C-008

## Scope

Implement the approved UI mock as a static frontend served by FastAPI. It consumes the contract endpoints for login, chat, citations, HR metrics through chat, escalation tickets, trend pins, document upload/listing, ticket update, and feedback.

## Allowed files

- `src/static/**`
- `src/main.py`
- `src/api/routes.py`
- `tests/test_api/test_static_app.py`
- `flow/05-contract.md`
- `buildflow/DESIGN.md`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_static_app.py`
- [x] Open the served frontend locally.
- [x] Login as demo employee, ask one policy question, and see cited answer.
- [x] Ask for leave balance and see personal metrics in the chat answer.
- [x] Login as HR admin, view tickets, trend pins, and document list.
- [x] Submit feedback from the UI and see a success state.
- [x] Review UI against `buildflow/DESIGN.md`.

## Done-evidence (world-state proof, named BEFORE building)

Paste the local frontend URL plus successful browser-observed outputs for employee chat and HR admin dashboard.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_static_app.py
```

Output:

```text
tests\test_api\test_static_app.py ... [100%]
3 passed in 0.17s
```

Full regression suite:

```text
48 passed in 1.48s
```

Served frontend verification:

```json
{
  "pid": 13288,
  "app_url": "http://127.0.0.1:8000/app",
  "static_status": 200,
  "policy_citation_count": 1,
  "metrics_action": "hr_metric_lookup",
  "feedback_ok": true
}
```

Browser open command succeeded for `http://127.0.0.1:8000/app`.

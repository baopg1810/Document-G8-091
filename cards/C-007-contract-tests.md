# C-007 - Contract tests for every endpoint

status: done
deps: C-006

## Scope

Create a contract test suite that asserts every endpoint in `flow/05-contract.md` exists in `/openapi.json`, has the expected auth behavior, and returns contract-shaped responses for happy and key failure paths.

## Allowed files

- `tests/test_api/test_contract.py`
- `tests/conftest.py`
- `tests/fixtures/**`
- `flow/05-contract.md`
- `src/models/schemas.py`
- `src/api/routes.py`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_contract.py`
- [x] Test fails if a contract endpoint is missing from `/openapi.json`.
- [x] Test fails if `ChatResponse`, `Ticket`, `TrendPin`, or `PersonalHrMetrics` shape drifts.
- [x] Test covers unauthorized, employee, and admin access for protected endpoints.

## Done-evidence (world-state proof, named BEFORE building)

Paste the full contract test command output and the local `/openapi.json` URL.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_contract.py
```

Output:

```text
tests\test_api\test_contract.py .... [100%]
4 passed in 0.26s
```

Full regression suite:

```text
45 passed in 1.56s
```

Local OpenAPI verification:

```json
{
  "title": "HR Helpdesk AI",
  "openapi_url": "http://127.0.0.1:8000/openapi.json",
  "docs_url": "http://127.0.0.1:8000/docs",
  "contract_paths_present": [
    "/health",
    "/api/v1/chat",
    "/api/v1/me/hr-metrics",
    "/api/v1/admin/tickets",
    "/api/v1/admin/trending/run",
    "/api/v1/feedback"
  ]
}
```

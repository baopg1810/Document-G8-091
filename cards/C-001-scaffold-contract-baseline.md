# C-001 - Scaffold HR Helpdesk API baseline

status: done
deps: none

## Scope

Rename/configure the existing FastAPI starter as HR Helpdesk AI and make the baseline app contract-ready: `/health`, `/docs`, `/openapi.json`, app settings, dependency list, and smoke tests. This card does not implement business endpoints beyond keeping current `/api/v1/status` and `/api/v1/chat` reachable as temporary scaffold behavior.

## Allowed files

- `src/main.py`
- `src/config.py`
- `src/api/routes.py`
- `src/models/schemas.py`
- `requirements.txt`
- `README.md`
- `ARCHITECTURE.md`
- `tests/test_api/test_routes.py`
- `tests/conftest.py`
- `Dockerfile`
- `docker-compose.yml`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_routes.py`
- [x] Start the app locally and open `http://localhost:8000/docs`.
- [x] `GET http://localhost:8000/health` returns JSON with `status: "ok"`.
- [x] `GET http://localhost:8000/openapi.json` includes the app title `HR Helpdesk AI`.

## Done-evidence (world-state proof, named BEFORE building)

Paste the `/health` JSON output and the local `/docs` URL showing the HR Helpdesk API title.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_routes.py
```

Output:

```text
tests\test_api\test_routes.py .... [100%]
4 passed in 0.04s
```

Local app verification:

```json
{
  "health": {
    "status": "ok",
    "env": "development"
  },
  "title": "HR Helpdesk AI",
  "docs_url": "http://127.0.0.1:8000/docs"
}
```

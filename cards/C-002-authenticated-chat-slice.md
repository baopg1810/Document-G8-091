# C-002 - Authenticated chat vertical slice

status: done
deps: C-001

## Scope

Build the first thin vertical slice: demo JWT login, `/api/v1/me`, contract-shaped `/api/v1/chat`, and one plain static page that logs in and sends a chat message to the API. The chat answer may be a deterministic stub, but it must return the `ChatResponse` shape from `flow/05-contract.md`.

## Allowed files

- `src/api/routes.py`
- `src/models/schemas.py`
- `src/services/auth.py`
- `src/services/demo_users.py`
- `src/main.py`
- `src/static/slice.html`
- `tests/test_api/test_auth.py`
- `tests/test_api/test_chat_slice.py`
- `.env.example`
- `requirements.txt`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_auth.py tests/test_api/test_chat_slice.py`
- [x] `POST /api/v1/auth/login` with a demo employee returns `access_token` and `user.role`.
- [x] `GET /api/v1/me` with the token returns the same user.
- [x] `POST /api/v1/chat` with the token returns `message_id`, `session_id`, `answer`, `citations`, `actions`, `escalated_ticket_id`, and `refusal_reason`.
- [x] Open the static slice page and send one successful chat message.

## Done-evidence (world-state proof, named BEFORE building)

Paste a successful login curl output, a chat curl output, and the local static page URL.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_auth.py tests/test_api/test_chat_slice.py
```

Output:

```text
tests\test_api\test_auth.py ... [ 50%]
tests\test_api\test_chat_slice.py ... [100%]
6 passed in 0.05s
```

Local app verification:

```json
{
  "login": {
    "token_type": "bearer",
    "user": {
      "id": "emp-001",
      "email": "employee@example.com",
      "full_name": "Nguyen Van An",
      "role": "employee",
      "department_id": "dept-hr-demo"
    }
  },
  "me": {
    "id": "emp-001",
    "email": "employee@example.com",
    "full_name": "Nguyen Van An",
    "role": "employee",
    "department_id": "dept-hr-demo"
  },
  "chat": {
    "message_id": "msg-61a5f758-d34c-4c2a-aa03-8bb3871b1ae5",
    "session_id": "session-50b5d80c-c26f-4433-9c62-40f39f5640a0",
    "answer": "Xin chao Nguyen Van An. Day la phan hoi scaffold cua HR Helpdesk AI; RAG va trich dan se duoc them o C-003.",
    "citations": [],
    "actions": [
      {
        "type": "none",
        "label": "No action required",
        "data": null
      }
    ],
    "escalated_ticket_id": null,
    "refusal_reason": null
  },
  "static_url": "http://127.0.0.1:8000/static/slice.html",
  "static_status": 200
}
```

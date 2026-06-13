# C-006 - Trending pins and answer feedback

status: done
deps: C-005

## Scope

Log chat queries, implement answer feedback, and implement threshold-based trending summary pins. HR admin can run `/api/v1/admin/trending/run`; employees can read `/api/v1/trending/pins`.

## Allowed files

- `src/api/routes.py`
- `src/models/schemas.py`
- `src/services/trending.py`
- `src/services/feedback.py`
- `src/services/llm.py`
- `src/agents/**`
- `tests/test_api/test_trending.py`
- `tests/test_api/test_feedback.py`
- `tests/fixtures/**`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_trending.py tests/test_api/test_feedback.py`
- [x] Five similar chat queries create one trend pin when admin runs trending.
- [x] Four similar chat queries do not create a trend pin at threshold five.
- [x] Employee token can read trend pins but cannot run trending.
- [x] `POST /api/v1/feedback` accepts `up` and `down` ratings for a valid message id.

## Done-evidence (world-state proof, named BEFORE building)

Paste a trend run response with one created pin, a trend pins response, and a feedback response.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_trending.py tests/test_api/test_feedback.py
```

Output:

```text
tests\test_api\test_trending.py ... [ 60%]
tests\test_api\test_feedback.py .. [100%]
5 passed in 0.10s
```

Full regression suite:

```text
41 passed in 1.26s
```

Local app verification:

```json
{
  "trend_run": {
    "created_pins": [
      {
        "id": "pin-212d6063-8aa9-48f6-b147-b1bac0678c10",
        "title": "Nghi phep",
        "summary": "Co 5 cau hoi gan day ve nghi phep. HR nen ghim cau tra loi chung cho chu de nay.",
        "source_query_count": 5,
        "citations": [],
        "expires_at": null
      }
    ],
    "skipped_topics": []
  },
  "trend_pins": {
    "pins": [
      {
        "title": "Nghi phep",
        "source_query_count": 5
      }
    ]
  },
  "feedback_up": {
    "ok": true
  },
  "feedback_down": {
    "ok": true
  }
}
```

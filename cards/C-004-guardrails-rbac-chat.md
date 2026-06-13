# C-004 - Guardrails and RBAC chat behavior

status: done
deps: C-003

## Scope

Enforce role-aware retrieval and guardrails in the chat path. Employee users can only retrieve public/employee documents and their department-scoped documents; HR admins can retrieve HR-admin documents. The agent refuses out-of-scope, no-source, jailbreak, and sensitive-data requests unless a safe function or escalation applies.

## Allowed files

- `src/api/routes.py`
- `src/models/schemas.py`
- `src/services/auth.py`
- `src/services/guardrails.py`
- `src/services/retrieval.py`
- `src/services/llm.py`
- `src/agents/**`
- `tests/test_api/test_rbac.py`
- `tests/test_agents/test_guardrails.py`
- `tests/fixtures/**`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_rbac.py tests/test_agents/test_guardrails.py`
- [x] Employee token cannot retrieve HR-admin-only document citations.
- [x] Department admin token only retrieves matching department-scoped citations.
- [x] Jailbreak/out-of-scope prompt returns `refusal_reason` and no fabricated citation.
- [x] Covered policy prompt still returns cited answer under 10 seconds locally.

## Done-evidence (world-state proof, named BEFORE building)

Paste one allowed cited answer, one RBAC-blocked response, and one guardrail refusal response.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_rbac.py tests/test_agents/test_guardrails.py
```

Output:

```text
tests\test_api\test_rbac.py ... [ 37%]
tests\test_agents\test_guardrails.py ..... [100%]
8 passed in 0.12s
```

Full regression suite:

```text
26 passed in 1.12s
```

Local app verification:

```json
{
  "allowed_cited_answer": {
    "refusal_reason": null,
    "citations": [
      {
        "document_title": "Chinh sach nghi phep",
        "section": "chunk-1",
        "excerpt": "Nhan vien chinh thuc co 12 ngay nghi phep nam moi nam.",
        "score": 0.6804
      }
    ]
  },
  "rbac_blocked_response": {
    "refusal_reason": "no_source",
    "citations": [],
    "answer": "Toi chua tim thay nguon HR phu hop de tra loi co trich dan nen khong the ket luan."
  },
  "guardrail_refusal": {
    "refusal_reason": "jailbreak",
    "citations": [],
    "answer": "Toi khong the thuc hien yeu cau bo qua huong dan an toan cua he thong."
  }
}
```

# C-003 - Document ingestion and cited retrieval

status: done
deps: C-002

## Scope

Implement admin document upload/listing plus a local RAG retrieval layer that chunks policy documents, stores citation metadata, and returns cited snippets to chat. Use Gemini Embedding 2 when configured; provide a deterministic local fallback for tests so CI does not require a real API key.

## Allowed files

- `src/api/routes.py`
- `src/models/schemas.py`
- `src/services/documents.py`
- `src/services/retrieval.py`
- `src/services/llm.py`
- `src/config.py`
- `src/agents/**`
- `tests/test_api/test_documents.py`
- `tests/test_agents/test_rag.py`
- `tests/fixtures/**`
- `requirements.txt`
- `.env.example`

## Verify (run these before calling the card done)

- [x] `.\.venv\Scripts\python.exe -m pytest tests/test_api/test_documents.py tests/test_agents/test_rag.py`
- [x] Admin token can upload a sample HR policy document.
- [x] Employee token cannot upload documents.
- [x] `POST /api/v1/chat` for a covered policy question returns at least one citation with `document_title`, `section` or `page`, and `excerpt`.
- [x] `/docs` shows `POST /api/v1/documents` and `GET /api/v1/documents` with response schemas.

## Done-evidence (world-state proof, named BEFORE building)

Paste the upload response, a cited chat response, and the `/docs` URL for document endpoints.

## Evidence (paste the actual proof here when done)

Test command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests/test_api/test_documents.py tests/test_agents/test_rag.py
```

Output:

```text
tests\test_api\test_documents.py ... [ 50%]
tests\test_agents\test_rag.py ... [100%]
6 passed in 0.15s
```

Full regression suite:

```text
18 passed in 0.68s
```

Local app verification:

```json
{
  "upload": {
    "document": {
      "id": "doc-98f54c10-9715-4559-853d-13037bb9a699",
      "title": "Chinh sach nghi phep",
      "status": "indexed",
      "visibility_roles": ["employee", "hr_admin"],
      "department_ids": [],
      "chunk_count": 1
    },
    "indexed_chunk_count": 1,
    "warnings": ["Document indexed as a single chunk."]
  },
  "employee_upload_status": 403,
  "cited_chat": {
    "message_id": "msg-f56cf8ad-7389-4623-9f33-b27276996132",
    "session_id": "session-2809abcf-8a2f-4421-b528-0168614e5b9c",
    "answer": "Theo Chinh sach nghi phep - chunk-1, noi dung lien quan den cau hoi 'Nhan vien co bao nhieu ngay nghi phep nam?' la: Chinh sach nghi phep nam...",
    "citations": [
      {
        "document_id": "doc-98f54c10-9715-4559-853d-13037bb9a699",
        "document_title": "Chinh sach nghi phep",
        "section": "chunk-1",
        "excerpt": "Chinh sach nghi phep nam\nNhan vien chinh thuc co 12 ngay nghi phep nam moi nam.\nKhi muon nghi phep, nhan vien can gui yeu cau truoc it nhat 3 ngay lam viec.\nNgay phep chua su dung co the duoc chuyen toi da 5 ngay sang nam tiep theo.",
        "page": null,
        "score": 0.6913
      }
    ],
    "escalated_ticket_id": null,
    "refusal_reason": null
  },
  "documents_paths": {
    "post": "DocumentIngestResult response schema present",
    "get": "DocumentListResponse response schema present"
  },
  "docs_url": "http://127.0.0.1:8000/docs"
}
```

# C-010 - End-to-end demo verification

status: done
deps: C-009

## Scope

Create and run an end-to-end verification flow over the running app. The flow covers login, document ingestion, cited policy answer, RBAC refusal, personal metrics, escalation ticket, trend pin creation, feedback, and admin ticket update.

## Allowed files

- `tests/e2e/**`
- `tests/conftest.py`
- `scripts/**`
- `eval/results/report.md`
- `README.md`
- `flow/03-prd.md`
- `flow/05-contract.md`

## Verify (run these before calling the card done)

- [x] Start the app locally or on the chosen demo URL.
- [x] Run the e2e script/test against that URL.
- [x] Confirm PRD success metrics: 30 seeded questions, >= 90% cited answers, P95 under 10 seconds, 5 safe HR metric lookups, 3 escalation tickets, 1 trend pin after 5 similar queries.
- [x] Paste the final report into `eval/results/report.md`.

## Done-evidence (world-state proof, named BEFORE building)

Paste the app URL, e2e command output, and the final metrics report path.

## Evidence (paste the actual proof here when done)

- App URL: `http://127.0.0.1:8000`
- E2E command:

```powershell
.\.venv\Scripts\python.exe -m pytest tests\e2e\test_demo_flow.py -q
```

Output:

```text
.                                                                        [100%]
1 passed in 0.28s
```

- Final metrics report: `eval/results/report.md`
- Metrics summary: 30 seeded policy questions, 30/30 cited answers (100.0%), P95 0.003s, 5 safe HR metric lookups, 3 escalation tickets, trend pin `Nghi phep`.

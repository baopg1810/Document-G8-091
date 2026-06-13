# C-008 - Static UI mock for chat and HR admin

status: done
deps: C-007

## Scope

Build a static HTML mock, reviewed against `buildflow/DESIGN.md`, for the employee chat screen and HR admin dashboard. The mock uses real Vietnamese-facing copy, contract-shaped sample data, citations, trend pins, ticket list, document upload state, and feedback controls, but no framework logic.

## Allowed files

- `mockups/hr-helpdesk.html`
- `mockups/assets/**`
- `buildflow/DESIGN.md`
- `flow/05-contract.md`

## Verify (run these before calling the card done)

- [x] Open `mockups/hr-helpdesk.html` in a browser.
- [x] The first viewport shows the usable chat/admin experience, not a marketing landing page.
- [x] The mock includes citations, personal metrics answer, escalation ticket, trend pins, document upload state, and feedback controls.
- [x] UI review against `buildflow/DESIGN.md`: object-first, Vietnamese copy, no emojis, no raw JSON, no nested cards, no feature explanation text.
- [x] Mobile-width browser view has no text overlap or clipped controls.

## Done-evidence (world-state proof, named BEFORE building)

Paste the local file path and screenshots or operator confirmation that the mock was opened and approved.

## Evidence (paste the actual proof here when done)

Mock built and approved:

- File: `mockups/hr-helpdesk.html`
- Browser open command succeeded from PowerShell.
- Static checks passed: no script, no raw template markers, no raw JSON, no HTML comments, no emoji text.
- Mock includes cited answer, personal metrics answer, escalation ticket, trend pin, document upload state, ticket list, and feedback controls.
- Operator approval received: `approve C-008`.
- Mobile-width visual check accepted by operator approval.

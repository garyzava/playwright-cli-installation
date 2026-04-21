# Playwright CLI Demo

Run 3 parallel sub-agents to test the contact form from different angles — exactly like the video demo.

## Prerequisites

- `playwright-cli` installed globally (`npm install -g @anthropic-ai/playwright-cli`)
- Chromium installed (`npx playwright install chromium`)
- Playwright CLI skill installed (`playwright-cli install --skills`)
- `uv` installed ([docs](https://docs.astral.sh/uv/))

## Start the server

```bash
uv run python -m http.server 8080
```

The form is now at http://localhost:8080.

## Run the demo

Paste this into Claude Code:

> Use the playwright-cli skill. Run 3 sub-agents in parallel, each testing the form at http://localhost:8080 with headed browsers and named sessions. Report results when all 3 complete.
>
> **Sub-agent 1 — Happy Path** (session: `-s=happy`):
> Fill every field with valid data (name: "Jane Smith", email: "jane@example.com", phone: "555-123-4567", subject: "General Inquiry", message: "This is a test message to verify the form works correctly end to end.", check both checkboxes). Submit and verify the success message shows "Thank you, Jane Smith!". Click "Send another message" and verify the form resets.
>
> **Sub-agent 2 — Validation** (session: `-s=validation`):
> Test each validation rule: submit with all fields empty and verify all error messages appear. Enter a 1-character name and verify the min-length error. Enter "not-an-email" and verify the email format error. Enter "abc" in phone and verify the phone format error. Enter a 5-character message and verify the min-length error. Fill all fields validly but leave Terms unchecked and verify the submit button is disabled.
>
> **Sub-agent 3 — Edge Cases** (session: `-s=edge`):
> Fill the message with exactly 500 characters and verify the counter shows "500 / 500". Try typing beyond 500 chars and verify the field enforces the max. Enter a unicode name like "Maria Garcia-Lopez" and verify the success message renders it correctly. Toggle the Terms checkbox on and off and verify the submit button enables/disables. Submit a valid form, click "Send another", and verify all fields are cleared.

## What to expect

Three browser windows open simultaneously. Each fills out and tests the form from a different angle. Results stream back as each sub-agent finishes.

## Test Results — All 21 checks PASSED

### Sub-agent 1 — Happy Path

| Check | Result |
|---|---|
| Fill all fields with valid data | PASS |
| Submit button enabled after Terms checked | PASS |
| Success message: "Thank you, Jane Smith! Your message has been sent." | PASS |
| "Send another" resets all fields | PASS |

### Sub-agent 2 — Validation

| Check | Result |
|---|---|
| Empty submit shows all required-field errors | PASS |
| 1-char name: "Name must be at least 2 characters." | PASS |
| Invalid email: "Please enter a valid email address." | PASS |
| Invalid phone: "Phone may only contain digits, dashes, parentheses, and spaces." | PASS |
| 5-char message: "Message must be at least 10 characters." | PASS |
| Terms unchecked: submit button disabled | PASS |

### Sub-agent 3 — Edge Cases

| Check | Result |
|---|---|
| 500-char message: counter shows "500 / 500" | PASS |
| Typing beyond 500 chars blocked by maxlength | PASS |
| Unicode name "María García-López" renders correctly in success message | PASS |
| Terms checkbox toggles submit button enabled/disabled | PASS |
| "Send another" clears all fields after submit | PASS |

### Screenshots

- `happy-path-result.png`
- `validation-result.png`
- `edge-cases-result.png`

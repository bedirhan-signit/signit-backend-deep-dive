# Backend engineer — technical assignment

Welcome. This repository is the briefing packet for your backend deep-dive interview.

---

## Assignment

Build a small signature request service.

A user can:

1. Upload one or more documents into a single signature request. Each document is typed as either **primary** or **supplement**.
2. Define the signatories on the request. A signatory is just a name and an email address. Signatories are unique within a request by email address.
3. For each supplement document, specify which signatories are allowed to see it. Primary documents are always visible to every signatory.

Creating a signature request returns its `id` and a creation timestamp.

## Signing

- When a signature request is created, a signing **token** is generated for each signatory. The token carries enough information to identify both the request and the signatory it belongs to.
- Tokens are written to the application log or to a local file. There is no email delivery.
- A mock signing endpoint accepts a POST request with a token. If the token is valid, the signatory is marked as signed.
- After every signatory has signed, the signature request is automatically marked as completed.

## Validation errors

The service must reject these at creation time with a clear error:

- Two signatories sharing the same email address on the same request.
- A visibility list passed on a primary document — primary documents are always visible to all signatories, so a visibility list on one is meaningless.

## Optional extension — vault and summary

If the core build is working with time to spare:

1. Move completed signature requests into a **vault** record.
2. The vault record carries a short summary describing the request: document name, participants, creation date, and a brief content summary. Any approach for generating the summary is acceptable — a stub is fine, we care about the shape of the integration.
3. Flag the signature request as vaulted.

## Rules

- **Language:** Python.
- **Framework:** FastAPI is the suggested framework. Anything else is fine if you're faster in it.
- **Storage:** SQLite, JSON files on disk, or anything equivalent. A real database is fine but not expected.
- **Files:** Save them to a local path inside the project.
- **AI tools, documentation, internet:** All allowed.
- **Tests:** At least one for the visibility rule. More is better.
- **Run-book:** Keep a short run-book below in this README so the service can be started and exercised together during the session.

## Out of scope

- Authentication on request creation and management endpoints.
- PDF rendering and real signing cryptography.
- Email or SMS delivery. Tokens are logged, not sent.
- A user interface is optional. Postman or `curl` is completely fine. Build a minimal UI only if it helps you demonstrate the flow.

## How we'll work in the session

- You drive. The interviewer watches and asks short questions.
- Think out loud.
- Take as much time as you need to read this brief and ask clarifying questions before you start.

---

## Run-book

> Fill this section in as you build.

```
# how to install dependencies

# how to start the service

# how to exercise it end to end

# how to run the tests
```

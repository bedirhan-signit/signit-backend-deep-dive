# Backend engineer — technical assignment

Welcome. This repository is the briefing packet for your backend deep-dive interview.

The assignment is split into **Phase 1** and **Phase 2**. Phase 1 is the required build. Phase 2 is an extension — attempt it only if Phase 1 is working end to end with time to spare.

---

## Phase 1 — signature request service

Build a small backend service that models the core signature request flow.

### The flow

1. **Upload documents.**
   The sender uploads one or more files. For each file, they provide its **type** — either `primary` or `supplement`.
   - Primary documents are included in every signatory's bundle.
   - Supplement documents are only included for the signatories the sender explicitly allows.

2. **Create the signature request.**
   The sender creates a signature request that references the uploaded documents and provides the **signatories**. A signatory is identified by a **name** and an **email address**. Signatories are unique within a request by email.
   For each supplement document in the request, the sender includes the list of signatory emails that should be able to see it.

3. **Persist the state.**
   When the request is created, the service persists everything needed to drive the rest of the flow:
   - the signature request itself, with its `id` and a creation timestamp,
   - the documents attached to it, their types, and their per-signatory visibility (for supplements),
   - the signatories, each with an initial action status of `pending`,
   - a **signing token**, unique per signatory, generated at creation time.

4. **Sign.**
   Each signatory signs by calling the signing endpoint with their token. The service validates the token, resolves the signatory it belongs to, and flips that signatory's action status from `pending` to `signed`. An invalid or already-used token is rejected.

5. **Auto-complete.**
   As soon as every signatory on the request has signed, the service automatically marks the signature request as `completed`.

### Token handling

- A signing token is generated for each signatory at request creation.
- Every token is unique and identifies exactly one signatory on exactly one signature request.
- Tokens are written to the application log or to a local file. There is no email or SMS delivery — the interviewer will read the token from your logs during the session.
- The signing endpoint accepts the token in the request body and performs all validation server-side.

### Validation

The service must reject these at creation time with a clear error:

- Two signatories on the same request sharing the same email address.
- A visibility list supplied on a primary document — primary documents are always visible to every signatory, so a list on one is meaningless.

### Endpoints

You choose the exact shapes, but the service must expose at least:

- An endpoint to **upload** a file, which returns the file's id and stored type.
- An endpoint to **create a signature request**, which accepts the documents, the signatories, and the supplement-visibility mapping, and returns the request's id and creation timestamp.
- An endpoint to **read the documents** a given signatory is allowed to see for a given request.
- An endpoint to **sign**, which accepts a token and flips the signatory's status to `signed`. When the last signatory signs, the request is marked `completed`.

---

## Phase 2 — vault and summary

Only start this phase after Phase 1 is running end to end.

1. When a signature request is `completed`, it moves into a **vault** record.
2. The vault record carries a short summary of the request:
   - the document names,
   - the list of participants,
   - the creation date,
   - a brief content summary.
   The content summary must be produced by integrating with a real LLM provider — OpenAI, Anthropic Claude, Google Gemini, or an equivalent. What we care about is the shape of the integration: how you handle the API key, how you design the prompt, how you parse the response, and how you handle errors and timeouts. The quality of the summary text itself is secondary.
3. The signature request is flagged as `vaulted`.

---

## Rules

- **Language:** Python.
- **Framework:** FastAPI is the suggested framework. Use something else only if you're faster in it.
- **Storage:** SQLite, JSON files on disk, or anything equivalent.
- **Files:** Save uploaded files to a local path inside the project.
- **AI tools, documentation, internet:** All allowed.
- **Tests:** Ship at least one test for the visibility rule. More is better.
- **Run-book:** Fill in the run-book at the bottom of this README so the service can be started and exercised during the session.

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

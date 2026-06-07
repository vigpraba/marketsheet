# High-Level System Architecture

## Purpose

Describe the main system components and how they communicate.

## Architecture Style

MarketSheet should start as a modular monolith:

- One React frontend.
- One Node.js API server.
- One Node.js worker process.
- One Postgres database.
- One Redis queue.

This keeps deployment and learning simple while still supporting background jobs, provider integrations, and production-like behavior.

## Main Components

| Component | Responsibility |
| --- | --- |
| React Web App | User interface for onboarding, imports, campaigns, approvals, and results. |
| Node.js API | Auth, validation, workspace permissions, orchestration, and request handling. |
| Node.js Worker | Background imports, AI jobs, campaign sends, and webhook processing. |
| Postgres | Durable source of truth for product data and business state. |
| Redis + BullMQ | Background job coordination, retries, delays, and worker concurrency. |
| Google Sheets API | Reads spreadsheet data after OAuth authorization. |
| OpenAI API | Produces structured campaign recommendations and copy drafts. |
| SendGrid | Sends email campaigns and emits email events. |
| Meta WhatsApp Cloud API | Sends compliant WhatsApp messages and emits delivery/reply events. |
| Stripe | Handles subscription billing and plan status. |

## System Boundary

The frontend does not talk directly to external providers. It only calls the MarketSheet API.

Provider credentials, secrets, webhooks, billing, and sending must remain server-side.

## High-Level Flow

```text
Browser -> React App -> Node API -> Postgres
                         |
                         v
                     Redis Queue
                         |
                         v
                    Node Worker
                         |
                         v
        Google / OpenAI / SendGrid / Meta / Stripe
```

## Why A Worker Exists

The worker handles slow and failure-prone work:

- Google Sheets imports.
- AI campaign generation.
- Bulk campaign sends.
- Webhook event processing.
- Scheduled sync and follow-up jobs.

The API stays fast and returns quickly after queueing work.

## Related Diagrams

- `docs/diagrams/full-system-architecture.md`
- `docs/diagrams/campaign-runtime-flow.md`
- `docs/diagrams/job-queue-flow.md`

## Related Issue

- `[Story] Create high-level system architecture`

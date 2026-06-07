# Low-Level Technical Design

## Purpose

Define the internal backend modules, API boundaries, state machines, provider adapter pattern, and worker design for the MVP.

## Recommended Stack

- Frontend: React, Vite, TypeScript.
- Backend: Node.js, Express, TypeScript.
- Worker: Node.js process using the same shared packages.
- Database: Postgres.
- ORM: Prisma.
- Queue: Redis and BullMQ.
- AI: OpenAI structured outputs.

## Backend Modules

| Module | Responsibility |
| --- | --- |
| `auth` | Login, sessions, user identity, workspace access. |
| `workspace` | Workspace settings, members, roles, sender configuration. |
| `billing` | Stripe checkout, subscriptions, plan limits, usage checks. |
| `integrations` | Provider connection records and encrypted credentials. |
| `sheets` | Google OAuth, spreadsheet selection, row reads, sync jobs. |
| `contacts` | Contact normalization, dedupe, tags, consent, suppression. |
| `campaigns` | Campaign drafts, state machine, audience snapshots, approval. |
| `agents` | Structured AI calls and stored agent runs. |
| `sending` | SendGrid and Meta WhatsApp provider adapters. |
| `tracking` | Click tracking, webhook event ingestion, campaign events. |
| `compliance` | Opt-in, unsubscribe, suppression, template and send eligibility. |
| `analytics` | Campaign summaries and dashboard metrics. |

## Campaign State Machine

```text
draft -> needs_review -> approved -> queued -> sending -> sent -> completed
                         |             |          |
                         v             v          v
                       paused        failed     failed
```

Rules:

- `needs_review` cannot transition to `queued` without explicit approval.
- `approved` campaigns must pass billing and compliance checks before queueing.
- `sending` campaigns should be resumable if worker jobs fail.
- `completed` means all eligible recipients were processed and final summaries were calculated.

## Provider Adapter Pattern

Provider-specific code should be isolated behind simple interfaces.

```text
EmailProvider
  sendEmail()
  parseWebhook()
  validateWebhook()

WhatsAppProvider
  sendTemplateMessage()
  sendSessionMessage()
  parseWebhook()
  validateWebhook()

BillingProvider
  createCheckoutSession()
  parseWebhook()
  getSubscriptionStatus()
```

Initial implementations:

- `SendGridEmailProvider`
- `MetaWhatsAppProvider`
- `StripeBillingProvider`
- `GoogleSheetsProvider`

## API Responsibilities

The API should:

- Authenticate and authorize requests.
- Validate request payloads.
- Store product state in Postgres.
- Enqueue background jobs.
- Return job IDs and current state quickly.
- Verify webhook signatures.

The API should not:

- Send thousands of messages inside a request.
- Run long AI workflows inline.
- Store provider credentials in the browser.
- Trust AI output without deterministic validation.

## Worker Responsibilities

The worker should:

- Process queue jobs.
- Call external APIs.
- Update Postgres with durable state.
- Retry transient failures.
- Record send attempts and events.
- Respect provider rate limits.

## Related Issue

- `[Story] Define low-level technical design`

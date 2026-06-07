# Job Queue And Worker Design

## Purpose

Explain why MarketSheet needs background jobs, where queues are used, and how worker processing should be designed.

## Why A Queue Is Needed

Some product actions are too slow, large, or unreliable for normal API requests:

- Importing a Google Sheet.
- Running AI campaign analysis.
- Sending a campaign to many recipients.
- Processing provider webhooks.
- Retrying failed provider calls.

The API should accept the request, store durable state, enqueue work, and return quickly. The worker processes the work in the background.

## Queue Technology

Use `BullMQ + Redis`.

Redis stores temporary job coordination. Postgres stores durable business state.

## Job Types

| Job | Purpose |
| --- | --- |
| `sheet.import` | Read Google Sheet rows and normalize data. |
| `agent.recommendCampaign` | Generate campaign recommendation from normalized data. |
| `campaign.send` | Prepare campaign recipients and create send jobs. |
| `campaign.sendRecipient` | Send one recipient message through one channel. |
| `webhook.process` | Convert provider webhook payloads into internal events. |
| `followup.suggest` | Phase 2 job for follow-up recommendations. |

## Campaign Sending Design

Do not send an entire campaign in one large loop.

Preferred design:

```text
campaign.send
  -> creates many campaign.sendRecipient jobs
  -> each recipient job sends one message/channel
  -> each job records SendAttempt and Event rows
```

Benefits:

- Safer retries.
- Easier progress tracking.
- One failed recipient does not fail the whole campaign.
- Concurrency can be tuned.
- Provider rate limits can be respected.

## Idempotency

Every send job must be safe to retry.

Before sending, the worker should check:

```text
Has this campaign recipient already been successfully sent on this channel?
```

If yes, skip the duplicate send and mark the job as completed.

## Retry Policy

Retry transient failures:

- Provider timeout.
- Temporary rate limit.
- Network failure.
- 5xx provider response.

Do not retry permanent failures:

- Invalid email.
- Invalid phone.
- Missing consent.
- Unapproved WhatsApp template.
- Suppressed recipient.

## Progress Tracking

Campaign progress should be calculated from durable tables:

```text
processed recipients / eligible recipients
```

The UI should not depend on Redis job state alone.

## Related Diagram

- `docs/diagrams/job-queue-flow.md`

## Related Issue

- `[Story] Define job queue and worker design`

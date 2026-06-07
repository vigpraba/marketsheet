# Phase 1 Implementation Stories

## Purpose

Define the first implementation stories after Phase 0 documentation is complete.

Phase 1 should build a usable MVP, not the full long-term platform.

## Recommended Phase 1 Stories

### 1. User Can Sign Up And Create A Workspace

Acceptance criteria:

- User can create an account.
- User can create one workspace.
- API scopes data by workspace.
- Basic authenticated routes are protected.

Suggested subtasks:

- Backend auth model.
- Workspace schema.
- Login/session flow.
- Frontend workspace creation UI.
- Auth tests.

### 2. User Can Connect Google Sheets

Acceptance criteria:

- User can start Google OAuth.
- User can complete OAuth callback.
- System stores encrypted Google tokens.
- User can select a spreadsheet.

Suggested subtasks:

- Google OAuth API.
- Integration connection schema.
- Sheet selector UI.
- Token encryption utility.
- OAuth failure handling.

### 3. User Can Import Contacts From A Sheet

Acceptance criteria:

- Import runs in a background job.
- Rows are normalized into contacts.
- Invalid rows are reported.
- Duplicates are flagged or merged consistently.
- Import status is visible in the UI.

Suggested subtasks:

- ImportRun schema.
- Sheet import worker.
- Contact normalization.
- Import summary API.
- Import UI.

### 4. User Can Create A Campaign Draft

Acceptance criteria:

- User can create a campaign.
- Campaign starts in `draft`.
- User can add email and WhatsApp message drafts.
- Campaign can move to `needs_review`.

Suggested subtasks:

- Campaign schema.
- Campaign message schema.
- Campaign API.
- Campaign builder UI.
- State transition tests.

### 5. User Can Generate A Campaign Recommendation

Acceptance criteria:

- User can request an AI recommendation.
- Agent output is schema-validated.
- Recommendation includes goal, audience, and draft copy.
- Agent output is stored in `AgentRun`.

Suggested subtasks:

- AgentRun schema.
- Structured output schema.
- Campaign recommendation worker.
- Recommendation UI.
- Agent failure handling.

### 6. User Can Review And Approve A Campaign

Acceptance criteria:

- User sees audience, copy, CTA, warnings, and recipient count.
- System blocks approval if required data is missing.
- User approval is recorded.
- Only approved campaigns can be queued for sending.

Suggested subtasks:

- Approval API.
- Compliance checks.
- Review UI.
- Audit event.
- Approval state tests.

### 7. User Can Send Email Campaigns

Acceptance criteria:

- Approved campaign creates queue jobs.
- Worker sends eligible email recipients through SendGrid.
- Send attempts are recorded.
- Unsubscribed or non-opted contacts are skipped.

Suggested subtasks:

- SendGrid adapter.
- Send recipient worker.
- SendAttempt schema.
- Email unsubscribe link.
- Send failure tests.

### 8. User Can Send WhatsApp Campaigns

Acceptance criteria:

- Approved campaign sends only to WhatsApp opted-in contacts.
- Meta Cloud API adapter sends template messages.
- Send attempts and provider IDs are recorded.
- Failed WhatsApp sends are visible.

Suggested subtasks:

- Meta WhatsApp adapter.
- Template configuration model.
- WhatsApp consent enforcement.
- Webhook verification.
- WhatsApp failure tests.

### 9. User Can View Campaign Results

Acceptance criteria:

- User can see send progress.
- User can see delivered, failed, clicked, replied, and unsubscribed counts.
- Results are scoped to workspace.
- Dashboard reads from durable events.

Suggested subtasks:

- Event schema.
- Results API.
- Tracking link route.
- Results dashboard UI.
- Event aggregation tests.

### 10. User Can Subscribe To A Paid Plan

Acceptance criteria:

- User can start Stripe checkout.
- Stripe webhook updates subscription status.
- Campaign sends check plan limits.
- Exceeded limits block sending with clear UI.

Suggested subtasks:

- Stripe adapter.
- Subscription schema.
- Billing checkout API.
- Stripe webhook handler.
- Plan limit tests.

## Related Issue

- `[Story] Define Phase 1 implementation stories`

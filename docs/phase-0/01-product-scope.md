# Product Scope And MVP Boundaries

## Purpose

Define what MarketSheet will build first, who it serves, and what is intentionally excluded from the MVP.

## Product Positioning

MarketSheet turns spreadsheet-style business data into daily marketing campaigns.

The product should not be positioned as generic bulk messaging infrastructure. It should be positioned as a workflow product:

```text
Connect your sheet. Get today's best campaign. Approve. Send. Track who acted.
```

## Target Users

Primary users:

- Digital product sellers selling templates, courses, ebooks, design assets, plugins, or downloads.
- Small ecommerce sellers using spreadsheets to track buyers, orders, and follow-ups.
- Creators and coaches running launches, cohorts, paid communities, or workshops.
- Small agencies managing simple campaigns for clients.

Common characteristics:

- They already use Google Sheets, CSV exports, Gumroad, Shopify, Stripe, or manual notes.
- They do not want to configure a full CRM.
- They need help deciding who to contact and what to send.
- They care about outcomes such as replies, purchases, bookings, and payments.

## Core Problem

Small sellers often have customer and order data, but it is scattered or messy. They manually inspect sheets, write campaign copy, send messages, and forget follow-ups.

MarketSheet solves this by converting existing data into campaign recommendations and execution.

## MVP Workflow

1. User signs up and creates a workspace.
2. User connects a Google Sheet or imports customer data.
3. System normalizes contacts, products, orders, and consent.
4. User asks MarketSheet for a campaign recommendation.
5. Agent suggests a campaign, audience, and message drafts.
6. User reviews and approves.
7. System sends through email and WhatsApp providers.
8. System tracks delivery, clicks, replies, and business outcomes.

## Phase 1 Features

- Workspace and user account.
- Google Sheets import.
- Contact normalization and consent tracking.
- Manual and agent-assisted campaign draft.
- Human approval before sending.
- Email sending through SendGrid.
- WhatsApp sending through Meta WhatsApp Cloud API.
- Basic tracking for send status, clicks, replies, failures, and unsubscribes.
- Stripe billing and plan limits.

## Out Of Scope For MVP

- Full CRM replacement.
- Full marketing automation builder.
- Instagram or Facebook outbound DM sending.
- Multi-client agency white labeling.
- Advanced ecommerce attribution.
- Native mobile app.
- Custom sending infrastructure.
- Evasion-based or non-compliant bulk messaging.

## Success Criteria

- A user can connect a sheet and import usable contacts.
- A user can receive a practical campaign recommendation.
- A user can approve and send a campaign.
- The system prevents sends to contacts without channel-specific consent.
- Campaign results are visible in one dashboard.
- The first workflow is useful without requiring a CRM migration.

## Open Questions

- What exact free plan limits should apply?
- Which sheet template should be recommended for onboarding?
- Should CSV import be added before or after live Google Sheets sync?
- Which first seller segment should be used for pilot users?

## Related Issue

- `[Story] Define MarketSheet product scope and MVP boundaries`

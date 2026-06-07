# Provider Integration Design

## Purpose

Define the first provider integrations and how MarketSheet should isolate provider-specific logic.

## Provider Adapter Pattern

Each provider should live behind an adapter. Product modules should depend on internal interfaces, not provider-specific payloads.

Initial adapters:

- `GoogleSheetsProvider`
- `SendGridEmailProvider`
- `MetaWhatsAppProvider`
- `StripeBillingProvider`
- `OpenAIProvider`

## Google Sheets

Purpose:

- Connect user spreadsheets.
- Read rows.
- Import contacts, products, and orders.

Key design points:

- Use OAuth.
- Store tokens encrypted.
- Use least-privilege scopes.
- Import through a background job.
- Store import results in `ImportRun`.

## SendGrid

Purpose:

- Send email campaigns.
- Receive delivery and engagement events.

Key design points:

- Sender identity must be configured.
- Email sends must include unsubscribe links.
- Webhooks must be verified before processing.
- Provider message IDs should be stored in `SendAttempt`.

## Meta WhatsApp Cloud API

Purpose:

- Send compliant WhatsApp business messages.
- Receive delivery and reply events.

Key design points:

- Business-initiated sends require approved templates when outside the service window.
- Contacts must have WhatsApp opt-in.
- Webhooks must be verified.
- Inbound replies should become `Event` records.
- No evasion-based sending behavior is allowed.

## Stripe

Purpose:

- Manage subscription billing.
- Enforce plan limits.

Key design points:

- Use Stripe Checkout for subscription start.
- Process Stripe webhooks for subscription status.
- Store current plan and limits in `Subscription`.
- Check limits before campaign sends.

## OpenAI

Purpose:

- Structured campaign recommendations.
- Audience reasoning.
- Copy drafts.

Key design points:

- Use structured outputs.
- Store `AgentRun` records.
- Never allow direct sending from AI output.

## Future Integrations

- Shopify.
- Gumroad.
- Lemon Squeezy.
- WooCommerce.
- Airtable.
- Zapier.
- Make.
- n8n.
- Calendly.

## Related Diagram

- `docs/diagrams/provider-integrations.md`

## Related Issue

- `[Story] Define provider integration design`

# Agentic AI Design And Guardrails

## Purpose

Define where MarketSheet should use agentic AI and where deterministic software must remain in control.

## Design Principle

AI recommends and drafts. Deterministic code validates and sends.

MarketSheet should use AI when judgment is useful across messy data. It should not allow AI to directly send campaigns, bypass consent, or make billing/compliance decisions.

## Agent Responsibilities

| Agent | Responsibility |
| --- | --- |
| Data Mapping Agent | Map messy sheet columns to known fields. |
| Data Quality Agent | Explain invalid rows, missing fields, duplicates, and consent gaps. |
| Campaign Opportunity Agent | Recommend useful campaigns from contacts, products, orders, and behavior. |
| Audience Agent | Build a target audience and explain why it qualifies. |
| Copy Agent | Draft email and WhatsApp message variants. |
| Compliance Review Assistant | Explain possible risks after deterministic checks run. |
| Follow-Up Agent | Phase 2 agent for reminder recommendations. |

## Deterministic Responsibilities

These must be handled by normal code:

- Authentication and authorization.
- Billing checks.
- Consent enforcement.
- Unsubscribe and suppression enforcement.
- WhatsApp template eligibility.
- Campaign approval state transitions.
- Provider retries.
- Rate limits.
- Send execution.
- Audit logs.

## Structured Outputs

Agent outputs should use strict schemas.

Example output types:

- `ColumnMappingResult`
- `DataQualityReport`
- `CampaignRecommendation`
- `AudienceDefinition`
- `MessageDraft`
- `ComplianceReview`

If an output does not match schema, the system should reject it and show a recoverable error.

## Human Approval

Phase 1 requires explicit user approval before sending any campaign.

The approval screen should show:

- Campaign goal.
- Audience summary.
- Number of eligible recipients.
- Excluded recipients and reasons.
- Email copy.
- WhatsApp copy.
- CTA link.
- Compliance warnings.

## Agent Run Storage

Every AI call should create an `AgentRun` record with:

- Workspace ID.
- Campaign ID if relevant.
- Agent type.
- Input summary.
- Output JSON.
- Schema version.
- Status.
- Error details.

This makes the system debuggable and easier to learn from.

## Related Diagram

- `docs/diagrams/agentic-ai-flow.md`

## Related Issue

- `[Story] Define agentic AI design and guardrails`

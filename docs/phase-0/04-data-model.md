# Data Model And Core Entities

## Purpose

Define the first version of MarketSheet's durable data model.

Postgres is the source of truth. Redis coordinates background work only.

## Core Entities

| Entity | Purpose |
| --- | --- |
| `User` | Login identity and user profile. |
| `Workspace` | Seller business account. |
| `WorkspaceMember` | Role-based membership in a workspace. |
| `IntegrationConnection` | Connected providers and encrypted credentials. |
| `SheetSource` | Google spreadsheet metadata and sync configuration. |
| `ImportRun` | One import or sync attempt. |
| `Contact` | Normalized customer or lead. |
| `ContactConsent` | Channel-specific consent and opt-out state. |
| `Product` | Optional product/catalog data inferred from sheet rows. |
| `Order` | Optional order/payment data inferred from sheet rows. |
| `Campaign` | Campaign goal, status, channels, and approval state. |
| `CampaignMessage` | Email and WhatsApp content variants. |
| `CampaignRecipient` | Frozen recipient snapshot for one campaign. |
| `SendAttempt` | Provider send attempt and provider message ID. |
| `Event` | Delivery, click, reply, unsubscribe, payment, or failure event. |
| `AgentRun` | Stored AI input/output, schema version, and result. |
| `Subscription` | Stripe subscription and plan limits. |

## Important Relationships

```text
User -> WorkspaceMember -> Workspace
Workspace -> IntegrationConnection
Workspace -> SheetSource -> ImportRun
Workspace -> Contact -> ContactConsent
Workspace -> Product
Workspace -> Order
Workspace -> Campaign -> CampaignMessage
Campaign -> CampaignRecipient -> SendAttempt -> Event
Campaign -> AgentRun
Workspace -> Subscription
```

## Multi-Tenant Rule

Every business entity should belong to a `Workspace`.

Queries must always scope data by workspace. This prevents one seller from accessing another seller's contacts, campaigns, or integrations.

## Consent Model

Consent should be channel-specific:

```text
ContactConsent
  contactId
  channel: email | whatsapp | sms
  status: opted_in | opted_out | unknown
  source
  capturedAt
```

Unknown consent must not be treated as opted in.

## Campaign Recipient Snapshot

Campaign recipients should be snapshotted at send time. This preserves what was actually sent even if contact records change later.

`CampaignRecipient` should store:

- Contact ID.
- Email/phone used at send time.
- Channel eligibility.
- Personalization fields.
- Skip reason if excluded.

## Event Model

Events should be append-only where practical:

- `sent`
- `delivered`
- `opened`
- `clicked`
- `replied`
- `failed`
- `unsubscribed`
- `paid`
- `booked`

Derived campaign summaries can be calculated from events.

## Indexing Notes

Likely indexes:

- Workspace foreign keys.
- Contact email and phone per workspace.
- Campaign status per workspace.
- Send attempt provider message ID.
- Event campaign and recipient IDs.
- Import run status.

## Related Issue

- `[Story] Define data model and core entities`

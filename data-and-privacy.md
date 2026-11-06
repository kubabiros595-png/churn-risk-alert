# Data and privacy

## How data gets in today

There is no API or webhook in this demo. The fictional accounts sit in an n8n data table called `Demo Accounts`. At 8:00 on weekdays the `Get demo accounts` node reads the whole table, and the workflow starts from there.

The table is the contract between the data source and the logic. Whatever fills it, the alert workflow works the same way.

## How data would get in for real

A separate workflow, for example `Account Data Sync`, would fill the table before 8:00.

| | A. Pull from APIs (recommended) | B. Webhook (source pushes) |
| --- | --- | --- |
| Who starts it | n8n, every day at 7:30 | The source system, on change or once a night |
| How | HTTP Request or native CRM and helpdesk nodes, with API keys stored in n8n credentials | A Webhook node at `https://<instance>/webhook/account-data`, POST with a JSON body |
| Authentication | API key or OAuth2 for each system | `X-API-Key` header (Header Auth); requests without it get 401 |
| Writing | Upsert into the table by `account_id` | Validate fields, then upsert by `account_id`; reply 200 with the count or 400 with the errors |
| Strength | Fresh data at a time we control | No need to call several systems |
| Weakness | Several integrations to maintain | We depend on the source sending complete data on time |

Typical sources in a SaaS company: the CRM gives name, plan, renewal date, last contact and notes; the helpdesk gives open tickets; product analytics gives active users and the usage trend.

## Data contract

| Field | Type and rule | Example | Real source | Used for | Sent to AI |
| --- | --- | --- | --- | --- | --- |
| `account_id` | text, required, unique | A022 | CRM ID | Upsert key and deduplication | No |
| `account_name` | text | Clover Pharmacy | CRM | Greeting, added by code after the AI reply | No |
| `renewal_date` | date `YYYY-MM-DD` | 2026-11-19 | Billing or CRM | Renewal points | Only as "19 November" when the topic is renewal |
| `active_users_pct` | number 0 to 100 | 16 | Analytics: active in 30 days / licences × 100 | Adoption points | No |
| `usage_change_30d_pct` | number, negative means a drop | -33 | Analytics: (last 30 days − previous 30) / previous × 100 | Usage points | No |
| `open_tickets` | whole number ≥ 0 | 9 | Helpdesk, open or waiting | Ticket points | Only the count when the topic is tickets |
| `days_since_last_contact` | whole number ≥ 0 | 64 | CRM, last email or meeting | Contact points | No |
| `csm_note` | free text | Complained about reporting | CRM note | Internal note for the reviewer | Never |
| `country`, `plan`, `mrr_eur` | text, text, number | Poland, Enterprise, 1200 | CRM and billing | Context only, no points | No |

An empty cell means "unknown", not zero. The workflow needs no personal data: no names, email addresses or phone numbers of people.

## What the AI receives

| Before the changes | After the changes |
| --- | --- |
| Company name, country, plan, monthly revenue, renewal date, all metrics, the internal note | The email topic, the question to ask and at most one fact: the renewal date or the number of open tickets |
| Real company name in the greeting | The placeholder `[ACCOUNT]`, replaced by code after the reply |
| Free-text notes that could contain names | Notes stay in n8n and only appear in the internal note |

This applies data minimisation and pseudonymises the account name before the AI call.

## Before using real data

1. **Review form:** it now requires an n8n login. Before that change, anyone with the link could change statuses.
2. **Execution history:** n8n stores the input of every run. Keep it for a short time or save failed runs only.
3. **Alert retention:** the alerts table grows every day. Delete alerts older than, for example, 90 days.
4. **Vendors:** n8n Cloud and Anthropic process data for the company. A data processing agreement and approval from whoever owns data protection are needed, and data may leave the EU.
5. **Email addresses:** the workflow does not store or need them. A person copies the draft and sends it from their own mailbox.

This is not legal advice. With real customer data, the company's data protection officer or legal team makes the decision.

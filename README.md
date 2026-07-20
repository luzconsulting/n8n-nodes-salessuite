# @luzconsulting/n8n-nodes-salessuite

![n8n Community Node](https://img.shields.io/badge/n8n-community--node-FF6D5A)
![Version](https://img.shields.io/badge/version-1.0.5-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Node.js](https://img.shields.io/badge/node-%3E%3D20.15-brightgreen)
![SalesSuite API](https://img.shields.io/badge/SalesSuite%20API-1.4.0-orange)

Verified against SalesSuite API **v1.4.0** (2026-07-05).

An n8n community node for [SalesSuite](https://salessuite.com) — read and write contacts, deals, notes and activities, manage webhooks, and react to CRM events in real time.

## What it does

The package ships two nodes:

- **SalesSuite** — a regular action node for CRUD-style operations against the CRM.
- **SalesSuite Trigger** — a webhook-based trigger node that starts a workflow when something happens in SalesSuite.

### SalesSuite (actions)

| Resource | Operations |
|---|---|
| Contacts | create, upsert, update, get by ID, find by email, search, list |
| Contact Persons | create, update, delete, set as main contact person, get by ID |
| Deals | create, update, get by ID, find by email, list by contact, list, list pipelines, move between pipeline/phase |
| Notes | create, get by ID, delete, list, list feed |
| Call Activities | create, get by ID, delete, list, list feed, list call types |
| Email Activities | log, get by ID, delete, list, list feed |
| Forms | list, get submissions |
| Users | list, get by ID, get by email |
| Webhooks | create, update, delete, list, get by ID |
| Action Buttons | list, list executions, preview data |
| Raw API Call | call any SalesSuite endpoint directly with your stored credentials |

### SalesSuite Trigger (events)

Contact created, contact property changed, deal created, deal property changed, deal stage changed, form submitted, call activity created, email activity logged, action button executed.

## Setup

### Requirements

- n8n **2.0.0+**
- A SalesSuite account with API access enabled

### Install

In n8n: **Settings → Community Nodes → Install**, then enter:

```
@luzconsulting/n8n-nodes-salessuite
```

Restart n8n afterwards — the node appears under both "SalesSuite" and "SalesSuite Trigger".

### Credentials

The node authenticates with an API key sent as an `x-api-key` header against `https://api.salessuite.com/api/v1`. Generate a key in SalesSuite under **Settings → Integrations → API Keys**, then add it as a new **SalesSuite API** credential in n8n. The credential includes a built-in connection test.

## Usage example

Create a new contact, create a deal for them, then log a call activity against that deal:

1. Add a **SalesSuite** node, select your **SalesSuite API** credential. Set **Resource** to `Contact` and **Operation** to `Create Contact`. Fill in the required **Fields** (e.g. email, first/last name).
2. Add a second **SalesSuite** node with **Resource** `Deal` and **Operation** `Create Deal`. Set **Deal Name**, then set **Contact Name or ID** to the contact created in step 1, e.g.:
   ```
   {{ $node["SalesSuite"].json.id }}
   ```
   Pick a **Pipeline Name or ID** and **Phase Name or ID** from the dropdowns.
3. Add a third **SalesSuite** node with **Resource** `Call Activity` and **Operation** `Create Call Activity`. Set **Attach To** to `Deal`, then set **Deal Name or ID** to the deal created in step 2, e.g.:
   ```
   {{ $node["SalesSuite1"].json.id }}
   ```
   Pick a **Call Type Name or ID** and fill in **Notes**.

Running the workflow creates the contact, links a deal to it, and logs a call activity on that deal — each step passes its output ID into the next via an expression.

## Maintenance status

This fork is actively kept in sync with the SalesSuite OpenAPI spec. See the version badge above for the API version it was last verified against.

## Disclaimer

This is an independent, community-maintained integration. It is not affiliated with, endorsed by, or sponsored by SalesSuite. For account or API issues, contact SalesSuite support directly; for issues with this node, use the [GitHub issue tracker](https://github.com/luzconsulting/n8n-nodes-salessuite/issues).

## License

MIT — see [LICENSE.md](LICENSE.md). Contributions welcome.

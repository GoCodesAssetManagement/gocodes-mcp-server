# GoCodes MCP Server

**Talk to your asset inventory. Ask questions, get answers — in plain English.**

Connect [GoCodes Asset Management](https://gocodes.com) to Claude and other AI assistants
through the Model Context Protocol (MCP).

[![MCP](https://img.shields.io/badge/Model_Context_Protocol-compatible-6E56CF)](https://modelcontextprotocol.io)
[![Auth](https://img.shields.io/badge/Auth-OAuth_2.1_+_PKCE-2B9348)](#security--privacy)
[![Access](https://img.shields.io/badge/Access-read--only-1E6091)](#read-only-by-design)
[![Website](https://img.shields.io/badge/gocodes.com-black)](https://gocodes.com)

</div>

---

## What is this?

The **GoCodes MCP Server** is a hosted connector that gives AI assistants secure, read-only
access to your GoCodes asset data. Instead of clicking through screens and running reports,
you just ask:

> *"Which assets are overdue for return?"*
> *"Summarize the inventory checked out to the Denver crew."*
> *"What maintenance is coming due in the next 30 days?"*
> *"Show me the photo and full history for asset ABCD-1234."*

Your AI assistant calls the GoCodes MCP server on your behalf, pulls live data from your
account, and answers in seconds — with every request made **as you**, respecting your
existing permissions.

> [!NOTE]
> This repository is documentation for the **hosted** GoCodes MCP service. There is nothing
> to build or install — you connect your AI client to a URL and sign in with your GoCodes
> account.

## Why use it

- **Natural-language inventory** — ask questions instead of building reports.
- **Live data** — answers come straight from your GoCodes account, not a stale export.
- **Zero setup** — no servers, no API keys to manage. Add a connector URL and sign in.
- **Secure by design** — OAuth 2.1 sign-in, per-user attribution, and **read-only** access.
- **Works with the tools you already use** — any MCP-compatible client, including Claude.

## Getting started

Connecting takes about a minute.

### 1. Add the connector

In your MCP-compatible client, add a new **remote MCP server / connector** using the hosted URL:

```
https://mcp.gokodes.com        <!-- replace with your production endpoint -->
```

<details>
<summary><b>Claude (web &amp; desktop)</b></summary>

1. Open **Settings → Connectors**.
2. Choose **Add custom connector**.
3. Paste the GoCodes MCP URL above and save.
4. Click **Connect** and complete the GoCodes sign-in (next step).

</details>

<details>
<summary><b>Other MCP clients</b></summary>

Any client that supports **remote MCP servers with OAuth** can connect. Point it at the
hosted URL above; the client will open the GoCodes sign-in page automatically during the
first connection.

</details>

### 2. Sign in with GoCodes

The first time you connect, you'll be redirected to the **GoCodes login page**. Sign in with
your normal GoCodes email and password. That's it — your client is now connected, and the
server acts as you for every request.

### 3. Ask away

Try one of the [example prompts](#example-prompts) below.

## What you can do

The server exposes focused, read-only tools grouped by area. Your AI assistant picks the
right one automatically based on what you ask.

### Assets

| Tool | What it does |
|------|--------------|
| `search_assets` | Free-text search across the inventory (name, ID, serial, model, …). |
| `get_asset_details` | Full detail record for a single asset by its GoCodes ID. |
| `get_asset_history` | Complete audit/change log for an asset. |
| `get_asset_assignment_history` | Check-out / assignment history: who had it and for how long. |
| `get_asset_location` | Current location of an asset. |
| `get_asset_picture` | The asset's photo, returned inline. |
| `get_assets_batch` | Details for several assets at once. |
| `list_assets_by_location` | Assets at a location, or a per-location count summary. |
| `list_checked_out_assets` | Everything currently checked out, with assignee and due date. |
| `list_overdue_assets` | Checked-out assets past their return date, most overdue first. |
| `list_maintenance_due` | Assets with upcoming or overdue scheduled service. |
| `list_asset_types` | The asset types defined in your account. |
| `list_asset_attachments` | Files attached to an asset. |
| `get_custom_field_schema` | Your account's custom-field definitions. |
| `summarize_customer_inventory` | Account-wide or per-assignee inventory summary (totals, value, breakdowns). |

### Kits

| Tool | What it does |
|------|--------------|
| `list_kits` | All kits (grouped asset bundles) in the account. |
| `get_kit_contents` | The assets contained in a specific kit. |

### Tasks &amp; scheduling

| Tool | What it does |
|------|--------------|
| `list_tasks` | All tasks, with due dates, assigned assets, and customer info. |
| `get_task_details` | Full detail for a single task. |
| `get_tasks_for_asset` | Every task assigned to a given asset. |
| `list_my_tasks` | Tasks assigned to you. |
| `list_task_statuses` | The task statuses configured in your account. |
| `list_task_attachments` | Files attached to a task. |
| `list_upcoming_events` | Upcoming scheduled events. |

### Account

| Tool | What it does |
|------|--------------|
| `list_customers` | Customers / assignees in your account. |

## Example prompts

```text
Which assets are overdue for return, and who has them?
Summarize everything checked out to Maria Gonzalez.
What maintenance is due in the next two weeks?
Show me the details, current location, and photo for asset 275UUSQ4.
List all tasks assigned to asset ABCD-1234.
How many assets do we have by type, and what's their total current value?
What's in the "Field Survey Kit"?
```

## Security &amp; privacy

Security is built into the connection, not bolted on.

- **OAuth 2.1 with PKCE.** You sign in through the official GoCodes login — your credentials
  are never shared with the AI client or stored by it. The client receives only a scoped,
  expiring token.
- **You, not a shared robot.** Every request to GoCodes is made under *your* identity, so
  your row-level permissions apply and your audit trail stays accurate. There is no shared
  service account.
- **Short-lived sessions.** Access tokens are short-lived and refreshed automatically;
  expired sessions simply prompt you to sign in again.
- **Encrypted in transit.** All traffic is over HTTPS.

### Read-only by design

Every tool is **read-only**. The MCP server can look up, list, and summarize your data — it
**cannot** create, edit, move, check out, or delete anything in your GoCodes account. You can
connect with confidence that an AI assistant won't change your records.

## Supported clients

Any MCP client that supports **remote servers with OAuth** works, including:

- **Claude** — web and desktop (Connectors)
- Other MCP-compatible assistants and IDE integrations

New clients are adopting remote MCP + OAuth quickly; if yours supports it, GoCodes will
connect.

## FAQ

<details>
<summary><b>Do I need a GoCodes account?</b></summary>

Yes. The MCP server surfaces *your* GoCodes data, so you sign in with an existing GoCodes
account. Don't have one? [Start here](https://gocodes.com).
</details>

<details>
<summary><b>Does this cost extra?</b></summary>

Contact your GoCodes account representative or <support@gocodes.com> for availability and
plan details. <!-- confirm pricing/positioning -->
</details>

<details>
<summary><b>Can it change my data?</b></summary>

No. Access is strictly read-only — see [Read-only by design](#read-only-by-design).
</details>

<details>
<summary><b>Which of my assets can it see?</b></summary>

Exactly the ones you can see in GoCodes. The server acts under your identity and honors your
existing permissions.
</details>

## Support

- 📖 Product help: [gocodes.com](https://gocodes.com) <!-- link to docs/help center -->
- ✉️ Email: <support@gocodes.com>
- 🐛 Found an issue with the connector? [Open an issue](../../issues).

## About GoCodes

[GoCodes](https://gocodes.com) is a cloud-based asset-tracking platform that helps teams
label, locate, assign, and maintain their equipment with QR codes and a mobile app. The MCP
server brings that same inventory to the AI assistants your team already uses.

---

<div align="center">
<sub>GoCodes and the GoCodes logo are trademarks of GoCodes. Built on the open
<a href="https://modelcontextprotocol.io">Model Context Protocol</a>.</sub>
</div>

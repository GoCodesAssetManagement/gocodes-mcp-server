<div align="center">

# GoCodes MCP Server

**Talk to your asset inventory. Ask questions, get answers — in plain English.**

Connect [GoCodes Asset Management](https://gocodes.com) to Claude, ChatGPT, and other AI
assistants through the Model Context Protocol (MCP).

[![MCP](https://img.shields.io/badge/Model_Context_Protocol-compatible-6E56CF)](https://modelcontextprotocol.io)
[![Auth](https://img.shields.io/badge/Auth-OAuth_2.1_+_PKCE-2B9348)](#security--privacy)
[![Writes](https://img.shields.io/badge/Writes-logged_%26_permission--gated-1E6091)](#safe-by-design)
[![Website](https://img.shields.io/badge/gocodes.com-black)](https://gocodes.com)

</div>

---

## What is this?

The **GoCodes MCP Server** is a hosted connector that gives AI assistants secure access to
your GoCodes asset data — look things up, run summaries, and make safe, logged edits to
assets and tasks when you ask. Instead of clicking through screens and running reports, you
just ask:

> *"Which assets are overdue for return?"*
> *"Summarize the inventory checked out to the Denver crew."*
> *"What maintenance is coming due in the next 30 days?"*
> *"Set asset ABCD-1234's status to In Repair and move its home location to Bay 3."*
> *"Create a task to service the generator, due Friday, and assign it to Maria."*

Your AI assistant calls the GoCodes MCP server on your behalf, pulls live data from your
account, and answers in seconds — with every request made **as you**, respecting your
existing permissions.

> [!NOTE]
> This repository is documentation for the **hosted** GoCodes MCP service. There is nothing
> to build or install — you connect your AI client and sign in with your GoCodes
> account.

## Why use it

- **Natural-language inventory** — ask questions instead of building reports.
- **Live data** — answers come straight from your GoCodes account, not a stale export.
- **Zero setup** — no servers, no API keys to manage. Add a connector/plugin and sign in.
- **Currently in beta** — available to all users including free trial users.
- **Secure by design** — OAuth 2.1 sign-in, per-user attribution, and edits that are
  permission-gated and fully logged.
- **Works with the tools you already use** — any MCP-compatible client, including Claude (just add the GoCodes connector)
  and ChatGPT (just add the GoCodes plugin).

## Getting started

> [!IMPORTANT]
> The GoCodes MCP server is currently in **beta**

### 1. Beta Terms of Service

> Participation is governed by the
> [MCP Beta Terms of Service Addendum](https://gocodes.com/terms-of-service/mcpbeta/), which
> applies in addition to your existing GoCodes agreement.

### 2. Add the connector

Claude and ChatGPT offer dedicated GoCodes Connector(Plugin) available with zero set up required. For other MCP-compatible clients, add a new **remote MCP server / connector** using the hosted URL:

```
https://mcp.gocodes.com/mcp
```

<details>
<summary><b>Other MCP clients</b></summary>

Any client that supports **remote MCP servers with OAuth** can connect. Point it at the
hosted URL above; the client will open the GoCodes sign-in page automatically during the
first connection.

</details>

### 3. Sign in with GoCodes

The first time you connect, you'll be redirected to the **GoCodes login page**. Sign in with
your normal GoCodes email and password — the same credentials you use for GoCodes. That's it:
your client is now connected, and the server acts as you for every request.

### 4. Ask away

Try one of the [example prompts](#example-prompts) below.

## What you can do

The server exposes focused tools grouped by area. Your AI assistant picks the right one
automatically based on what you ask. Most are read-only; the tools that can change data are
listed under [Editing and undo](#editing-and-undo).

### Assets

| Tool | What it does |
|------|--------------|
| `search_assets` | Free-text search across the inventory (name, ID, serial, model, …). |
| `get_asset_details` | Full detail record for a single asset by its GoCodes ID. |
| `get_asset_history` | Complete audit/change log for an asset. |
| `get_asset_assignment_history` | Check-out / assignment history: who had it and for how long. |
| `get_asset_location` | The asset's last recorded location (GPS coordinates and the date). |
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

### Editing and undo

These are the only tools that change anything. Each one requires an account role that
already permits the change, and every edit is recorded in your account's history.

**Assets**

| Tool | What it does |
|------|--------------|
| `update_asset` | Update the editable fields of a single asset — status, home location, assignment / check-out, service dates, costs, model, serial number, custom fields, and more. Only the fields you name change; every other field is left exactly as it was. |
| `restore_assets` | Undo recent **asset** edits. Previews by default, then rolls the affected assets back to their earlier state for a chosen day or date range. Restores are themselves logged, so they can be undone too. |

**Tasks** *(requires the Tasks feature on your account)*

| Tool | What it does |
|------|--------------|
| `create_task` | Create a task on an asset — name, description, due date, cost, priority, and optionally assign it in one step. |
| `update_task` | Edit an existing task. As with assets, only the fields you name change; the rest are preserved. |
| `assign_task` | Assign a task to a user by email — or leave the email out to unassign it. |
| `update_task_status` | Change only a task's status (e.g. to In Progress or Completed). |

> [!IMPORTANT]
> **`restore_assets` undoes asset edits only.** Task changes are recorded in your account's
> history but are not covered by the one-step restore — review task edits before confirming
> them.

## Example prompts

```text
Which assets are overdue for return, and who has them?
Summarize everything checked out to Maria Gonzalez.
What maintenance is due in the next two weeks?
Show me the details, current location, and photo for asset 275UUSQ4.
List all tasks assigned to asset ABCD-1234.
How many assets do we have by type, and what's their total current value?
What's in the "Field Survey Kit"?
Mark asset ABCD-1234 as checked out to Maria Gonzalez.
Set the next service date for pump 275UUSQ4 to March 1st and its status to In Service.
Create a task to replace the filter on 275UUSQ4, due next Friday, assigned to Maria.
Mark the inspection task on ABCD-1234 as Completed.
Undo the changes I made to my assets today.
```

## Security &amp; privacy

Security is built into the connection, not bolted on.

- **OAuth 2.1 with PKCE.** You sign in through the official GoCodes login — your credentials
  are never shared with the AI client or stored by it. The client receives only a scoped,
  expiring token.
- **You, not a shared robot.** Every request to GoCodes is made under *your* identity, so
  your row-level permissions apply and your audit trail stays accurate. There is no shared
  service account.
- **Token lifetimes.** Authorization codes last 5 minutes. Access tokens last 15 minutes and
  refresh automatically. Refresh tokens and the login cookie last 8 hours.
- **Encrypted in transit.** All traffic is over HTTPS.
- **Rate limited.** 100 requests per minute, per user. Requests beyond that are throttled.
- **Data retention.** The server reads from your account live and does not create a second
  copy. GoCodes does not retain MCP request or response content.
- **No AI training on your data.** GoCodes does not permit AI providers to train on customer
  data. Nothing is shared beyond the session context needed to answer the request.
- **Hosted on Azure.** GoCodes hosts the server on Microsoft Azure in the US East region, with
  offsite backups to alternate Azure locations.
- **View-only users.** Write tools remain visible to the client but return a permission error
  instead of applying a change if your role doesn't allow the edit.

### Safe by design

Most tools are **read-only** — lookups, lists, and summaries. The handful that can change
data are fenced in on every side:

- **Permission-gated.** Editing an asset requires a role that already allows editing
  (Group Administrator or Asset Manager). Creating
  and editing **tasks** is Group Administrator, or Asset
  Manager — with Asset Assigners also able to assign tasks and change their status. If your
  role is view-only, the server simply can't write.
- **Surgical.** Only the fields you name are changed; every other field on the asset or task
  is preserved exactly as it was. Values replace the existing field rather than being
  appended to it.
- **Fully logged.** Changes are recorded in your account's history, attributed to you.
- **Reversible (assets).** The **`restore_assets`** tool previews and then rolls back recent
  asset edits for a day or date range — an undo button for asset changes an assistant made.

The server can **never delete anything**, and it never creates or deletes assets. The only
thing it can create is a task, and only when you ask.

## Supported clients

Any MCP client that supports **remote servers with OAuth** works, including:

- **Claude** — web and desktop (Connectors)
- **ChatGPT** — Plugins (availability varies by plan)
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

No
</details>

<details>
<summary><b>What terms apply during the beta?</b></summary>

The [MCP Beta Terms of Service Addendum](https://gocodes.com/terms-of-service/mcpbeta/) governs
use of the MCP server during the beta, in addition to your existing GoCodes agreement.
</details>

<details>
<summary><b>Can it change my data?</b></summary>

Only in specific, guarded ways. It can edit fields on an **asset**, and create or edit
**tasks** — but only if your GoCodes role already permits that change, only the fields you
ask it to, and every change is logged and attributed to you. Asset edits can be rolled back
with `restore_assets`. The server can never delete anything, and never creates or deletes
assets. See [Safe by design](#safe-by-design).
</details>

<details>
<summary><b>Which of my assets can it see?</b></summary>

Exactly the ones you can see in GoCodes. The server acts under your identity and honors your
existing permissions.
</details>

<details>
<summary><b>Why can't I see the task tools?</b></summary>

The task tools require the **Tasks** feature to be enabled on your GoCodes account, and a
role that permits task changes. If tasks aren't enabled, those tools will report that rather
than making a change. Contact your account representative to enable them.
</details>

## Support

- 📖 Product help: [gocodes.com](https://support.gocodes.com)
- ✉️ Email: <support@gocodes.com>
- 🐛 Found an issue with the connector? [Open an issue](../../issues).
- 📄 Beta terms: [MCP Beta Terms of Service Addendum](https://gocodes.com/terms-of-service/mcpbeta/)

## About GoCodes

[GoCodes](https://gocodes.com) is a cloud-based asset-tracking platform that helps teams
label, locate, assign, and maintain their equipment with QR codes and a mobile app. The MCP
server brings that same inventory to the AI assistants your team already uses.

---

<div align="center">
<sub>GoCodes and the GoCodes logo are trademarks of GoCodes. Built on the open
<a href="https://modelcontextprotocol.io">Model Context Protocol</a>.</sub>
</div>

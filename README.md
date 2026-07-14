<div align="center">

# GoCodes MCP Server

**Talk to your asset inventory. Ask questions, get answers — in plain English.**

Connect [GoCodes Asset Management](https://gocodes.com) to Claude and other AI assistants
through the Model Context Protocol (MCP).

[![MCP](https://img.shields.io/badge/Model_Context_Protocol-compatible-6E56CF)](https://modelcontextprotocol.io)
[![Auth](https://img.shields.io/badge/Auth-OAuth_2.1_+_PKCE-2B9348)](#security--privacy)
[![Writes](https://img.shields.io/badge/Writes-logged_%26_reversible-1E6091)](#safe-by-design)
[![Website](https://img.shields.io/badge/gocodes.com-black)](https://gocodes.com)

</div>

---

## What is this?

The **GoCodes MCP Server** is a hosted connector that gives AI assistants secure access to
your GoCodes asset data — look things up, run summaries, and make safe, logged edits to an
asset when you ask. Instead of clicking through screens and running reports, you just ask:

> *"Which assets are overdue for return?"*
> *"Summarize the inventory checked out to the Denver crew."*
> *"What maintenance is coming due in the next 30 days?"*
> *"Set asset ABCD-1234's status to In Repair and move its home location to Bay 3."*

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
- **Secure by design** — OAuth 2.1 sign-in, per-user attribution, and edits that are
  permission-gated, fully logged, and reversible.
- **Works with the tools you already use** — any MCP-compatible client, including Claude.

## Getting started

Connecting takes about a minute.

### 1. Add the connector

In your MCP-compatible client, add a new **remote MCP server / connector** using the hosted URL:

```
https://mcp.gocodes.com/mcp
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

The server exposes focused tools grouped by area. Your AI assistant picks the right one
automatically based on what you ask. All but two are read-only; the write tools are called
out under [Editing and undo](#editing-and-undo).

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

### Editing and undo

These are the only tools that change anything. Both require an account role that permits
editing, and every change is written to the asset's audit history.

| Tool | What it does |
|------|--------------|
| `update_asset` | Update the editable fields of a single asset — status, home location, assignment / check-out, service dates, costs, model, serial number, custom fields, and more. Only the fields you name change; every other field is left exactly as it was. |
| `restore_assets` | Undo recent edits. Previews by default, then rolls the affected assets back to their earlier state for a chosen day or date range. Restores are themselves logged, so they can be undone too. |

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
- **Short-lived sessions.** Access tokens are short-lived and refreshed automatically;
  expired sessions simply prompt you to sign in again.
- **Encrypted in transit.** All traffic is over HTTPS.

### Safe by design

Almost every tool is **read-only** — lookups, lists, and summaries. The one way the server
can change anything is the **`update_asset`** tool, and it's fenced in on every side:

- **Permission-gated.** Edits require an account role that already allows editing
  (Administrator, Customer, Group Administrator, Asset Manager, or Asset Assigner). If your
  role is view-only, the server simply can't write.
- **Surgical.** Only the fields you name are changed; every other field on the asset is
  preserved exactly as it was.
- **Fully logged.** Every change is recorded in the asset's audit history, attributed to you.
- **Reversible.** The **`restore_assets`** tool previews and then rolls back recent edits for
  a day or date range — an undo button for anything an assistant changed.

The server can **never create or delete** assets.

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
plan details.
</details>

<details>
<summary><b>Can it change my data?</b></summary>

Only in one specific, guarded way. The `update_asset` tool can edit fields on an asset — but
only if your GoCodes role already permits editing, only the fields you ask it to, and every
change is logged and reversible with `restore_assets`. It can never create or delete assets.
See [Safe by design](#safe-by-design).
</details>

<details>
<summary><b>Which of my assets can it see?</b></summary>

Exactly the ones you can see in GoCodes. The server acts under your identity and honors your
existing permissions.
</details>

## Support

- 📖 Product help: [gocodes.com](https://support.gocodes.com)
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

[![M8ven Score](https://m8ven.ai/badge/mcp/agencyhandy/agencyhandy-mcp)](https://m8ven.ai/mcp/agencyhandy/agencyhandy-mcp?s=readme)

<p align="center"><img src="assets/logo-400.png" width="96" height="96" alt="Agency Handy logo"></p>

# Agency Handy MCP server

Connect an AI assistant to your [Agency Handy](https://agencyhandy.com) workspace. Ask about clients, leads, projects, tasks, tickets, invoices and proposals, or create and update them, from Claude, Cursor, VS Code, Cline and other MCP clients.

The server is hosted by Agency Handy. This repository holds the connection instructions and the registry listing; there is nothing to install or run yourself.

| | |
|---|---|
| **Endpoint** | `https://mcp.agencyhandy.com/` (Streamable HTTP, `POST /`) |
| **Auth** | **Sign in with Agency Handy** (OAuth 2.1, PKCE, dynamic client registration), or a workspace API key sent as `Authorization: Bearer <key>` |
| **Registry** | `com.agencyhandy/mcp` in the [Official MCP Registry](https://registry.modelcontextprotocol.io) |
| **Docs** | [docs.agencyhandy.com](https://docs.agencyhandy.com/integrations/claude-cursor-mcp) |

## How you connect

**Sign in with Agency Handy (recommended).** Add the server URL in your assistant and it opens an Agency Handy window. Enter your workspace, sign in, choose the workspace and click **Allow**. Nothing to copy or paste. Each connection appears in Agency Handy under **Settings → Workspace Config → API Key**, where you can remove it at any time.

**Or use an API key.** For tools that can't do OAuth, generate a key in **Settings → Workspace Config → API Key** and send it as `Authorization: Bearer YOUR_API_KEY`. The key acts as you, with your role's permissions, in that one workspace.

## Set up your client

### Claude (claude.ai and Claude Desktop)

**Settings → Connectors → Add custom connector** (on claude.ai: **Customize → Connectors → Add**). Name `Agency Handy`, URL `https://mcp.agencyhandy.com/`, then **Connect** and sign in.

### ChatGPT

In developer mode: **Settings → Apps & Connectors → Create**. URL `https://mcp.agencyhandy.com/`, authentication **OAuth**, then sign in with Agency Handy.

### Claude Code

Install the plugin, which also adds a usage skill and asks you to confirm before anything is sent to a client or deleted:

```
/plugin marketplace add AgencyHandy/claude-plugins
/plugin install agencyhandy@agencyhandy
```

Or add the server directly, then run `/mcp` and choose **Authenticate**:

```
claude mcp add --transport http agencyhandy https://mcp.agencyhandy.com/
```

With an API key instead: `claude mcp add --transport http agencyhandy https://mcp.agencyhandy.com/ --header "Authorization: Bearer YOUR_API_KEY"`

### Cursor

`~/.cursor/mcp.json`, then click **Login** next to the server in Cursor's MCP settings:

```json
{
  "mcpServers": {
    "agencyhandy": { "url": "https://mcp.agencyhandy.com/" }
  }
}
```

With an API key instead, add `"headers": { "Authorization": "Bearer YOUR_API_KEY" }`.

### VS Code (GitHub Copilot)

`.vscode/mcp.json`. VS Code asks you to sign in when the server starts:

```json
{
  "servers": {
    "agencyhandy": { "type": "http", "url": "https://mcp.agencyhandy.com/" }
  }
}
```

### Cline and other stdio-only clients

```json
{
  "mcpServers": {
    "agencyhandy": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.agencyhandy.com/"]
    }
  }
}
```

`mcp-remote` opens the sign-in in your browser. With an API key instead, add `"--header", "Authorization: Bearer ${AGENCYHANDY_API_KEY}"` to `args` and set `AGENCYHANDY_API_KEY` in `env`.

## What it can do

About 100 tools, all starting with `ah_`:

- **Read:** clients, leads, client companies, projects and their tasks, tickets, comments, time entries, invoices, proposals, orders, subscriptions, services, forms, custom fields, webhooks, chat.
- **Owner insights:** `ah_dashboard_digest`, `ah_cash_risk` (overdue and at-risk invoices), `ah_client_churn_risk`.
- **Write:** create and update leads, clients, tasks, tickets, comments, invoices, proposals, orders and services; assign work and change status.
- **Send:** `ah_invoice_send`, `ah_proposal_send` and `ah_client_invite` email your clients.

Start a session with `ah_health` and read the `ah://full-context` resource for the data model.

## Safety

Everything runs with the permissions of the person who connected, in the one workspace they chose. Tools that email clients or delete data change real customer data, so check what the assistant proposes before you confirm it. The Claude Code plugin always asks before those actions. The sign-in screen warns when an app is not verified, and always shows where it will send you back to.

The hosted server does not store your workspace data. It keeps short request logs (tool, timing, member and workspace IDs, IP address, errors with keys removed) for up to 14 days. See the [privacy policy](https://www.agencyhandy.com/privacy-policy/).

## Support

[agencyhandy.com/contact-us](https://www.agencyhandy.com/contact-us/) · support@agencyhandy.com

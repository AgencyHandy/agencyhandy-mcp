# Installing the Agency Handy MCP server (for AI agents)

The server is hosted. Do not clone, build or run anything from this repository.

1. Ask the user for their Agency Handy API key. They create it in Agency Handy under **Settings → Workspace Config → API Key**. Never invent a key and never print it back in full.
2. Add this server to the client's MCP settings, using the key in the `Authorization` header:

```json
{
  "mcpServers": {
    "agencyhandy": {
      "url": "https://mcp.agencyhandy.com/",
      "headers": { "Authorization": "Bearer THE_USER_API_KEY" }
    }
  }
}
```

   If the client only supports stdio servers (for example Claude Desktop), use:

```json
{
  "mcpServers": {
    "agencyhandy": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.agencyhandy.com/", "--header", "Authorization: Bearer ${AGENCYHANDY_API_KEY}"],
      "env": { "AGENCYHANDY_API_KEY": "THE_USER_API_KEY" }
    }
  }
}
```

3. Check the connection by calling the `ah_health` tool. It returns `"ok": true` when the key works. A 401 means the key is wrong, expired or deleted.
4. Before calling any tool that sends something to a client (`ah_invoice_send`, `ah_proposal_send`, `ah_client_invite`) or deletes data, show the user exactly what will happen and wait for their confirmation.

# Connect MARU Japan Hands in Cline

Add this to the MCP servers configuration. No API key or local install is required.

```json
{
  "mcpServers": {
    "maru-japan-hands": {
      "type": "streamableHttp",
      "url": "https://maru.senpinai.com/mcp"
    }
  }
}
```

Verify by listing tools; do not create a service, takeout or wallet payment request as a test. Human services are paid.

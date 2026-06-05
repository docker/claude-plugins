---
description: Show Docker MCP Gateway status and enabled servers
allowed-tools: Bash(docker mcp:*)
---

# Docker MCP Gateway Status

Display the current status of Docker MCP Gateway and configured MCP servers.

## Current Status

- **Enabled MCP Servers**: !`docker mcp profile server ls`

## Summary

Please analyze the above information and present a clear summary showing:

- ✅ Docker MCP Gateway status (operational/not operational)
- 📦 List of enabled MCP servers
- 🔧 Total available tools across all servers
- ⚙️  Any custom configuration settings
- 💡 Recommendations for optimization or issues found

If the commands fail or show errors, recommend running `/docker-mcp-toolkit:gateway-debug` for detailed troubleshooting.

# Docker MCP Toolkit Plugin

This plugin integrates Docker Desktop's MCP Toolkit with Claude Code, providing seamless access to containerized MCP servers managed by Docker Desktop.

> **⚠️ Important**: This plugin is designed specifically for Docker Desktop's bundled MCP Toolkit. It is **not** intended for the standalone open-source MCP Gateway from the [mcp-gateway repository](https://github.com/docker/mcp-gateway). While it may work with the standalone version, compatibility is not guaranteed and features may differ.

## Prerequisites

**This plugin requires Docker Desktop with MCP Toolkit:**

- **Docker Desktop** - Version 4.28.0 or later
- **MCP Toolkit Feature Enabled** - Navigate to: Settings → Beta Features → Enable "Docker MCP Toolkit"

Docker Desktop's MCP Toolkit provides:
- Pre-configured MCP Gateway bundled with Docker Desktop
- Integrated secrets management
- Automatic catalog management
- Native Docker Desktop UI integration

### Verifying Docker Desktop MCP Toolkit

Before installing this plugin, verify the Docker MCP Toolkit is enabled in Docker Desktop:

```bash
# Check if the docker mcp command is available
docker mcp --help

# Should display the MCP Gateway help information from Docker Desktop
```

If this command fails, ensure:
1. Docker Desktop 4.28+ is installed
2. MCP Toolkit feature is enabled in Docker Desktop settings

## What This Plugin Does

When enabled, this plugin:

1. Automatically starts the Docker MCP Gateway as an MCP server within Claude Code
2. Exposes all MCP servers configured in Docker Desktop (via `docker mcp server enable`)
3. Makes containerized MCP tools, resources, and prompts available to Claude

## Available Commands

This plugin provides helpful slash commands for managing and debugging Docker MCP Gateway:

- **`/docker-mcp-toolkit:gateway-status`** - Quick status check showing enabled servers and available tools
- **`/docker-mcp-toolkit:gateway-debug`** - Comprehensive diagnostics for troubleshooting gateway issues

## Usage

### Installation

```bash
# Add the marketplace (if not already added)
/plugin marketplace add /path/to/claude-plugins

# Install the plugin
/plugin install docker-mcp-toolkit@docker
```

### Configuration

This plugin uses the default Docker MCP Gateway configuration. To configure which MCP servers are available:

```bash
# List available servers in the Docker MCP catalog
docker mcp catalog show docker-mcp

# Enable specific MCP servers
docker mcp server enable <server-name>

# List currently enabled servers
docker mcp server ls

# Configure server settings (if needed)
docker mcp config write '<yaml-config>'
```

### Verification

Once the plugin is installed and Claude Code is running, the Docker MCP Gateway will automatically start. You can verify this by:

1. Running `/docker-mcp-toolkit:gateway-status` to see enabled servers and tool count
2. Checking that MCP tools are available in Claude's toolkit
3. Using Claude to interact with your enabled MCP servers
4. Running `/docker-mcp-toolkit:gateway-debug` if you encounter any issues

## Troubleshooting

### Plugin fails to start

**Error**: `command not found: docker` or `unknown command: mcp`

**Solution**: Ensure Docker Desktop is installed with Docker MCP Toolkit enabled (Settings → Beta Features → Enable "Docker MCP Toolkit").

### No MCP servers available

**Error**: Gateway starts but no tools are available

**Solution**: Enable MCP servers using `docker mcp server enable <server-name>`

### Gateway connection issues

**Error**: MCP server fails to connect

**Solution**:
1. Verify Docker daemon is running: `docker ps`
2. Check MCP server status: `docker mcp server ls`
3. Review gateway logs for errors

## Learn More

- [Docker MCP Gateway Documentation](https://github.com/docker/mcp-gateway)
- [Docker MCP Catalog](https://hub.docker.com/mcp)
- [MCP Specification](https://spec.modelcontextprotocol.io/)
- [Claude Code MCP Integration](https://docs.claude.com/en/docs/claude-code/mcp)

## License

MIT

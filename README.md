# Docker Marketplace for Claude Code

This repository contains a Claude Code plugin marketplace that provides Docker-related plugins.

## Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace configuration
└── plugins/
    └── docker-mcp-toolkit/       # Docker MCP Toolkit plugin
        ├── .claude-plugin/
        │   └── plugin.json       # Plugin manifest
        └── .mcp.json             # MCP server configuration
```

## Plugins

### docker-mcp-toolkit

Integrates Docker Desktop's MCP Toolkit with Claude Code, enabling access to containerized MCP servers managed by Docker Desktop.

> **Note**: This plugin is designed specifically for Docker Desktop's bundled MCP Toolkit. It is not intended for the standalone open-source MCP Gateway. A separate plugin for the OSS version may be provided in the future.

**Features:**
- Connects Claude Code to Docker Desktop's MCP Gateway
- Provides access to all MCP servers configured via Docker Desktop's MCP Toolkit
- Includes diagnostic commands for troubleshooting gateway issues
- Leverages Docker Desktop's integrated secrets management

**Prerequisites:**
- Docker Desktop 4.28+ with MCP Toolkit feature enabled
- MCP Gateway accessible via `docker mcp` command

## Usage

### Adding the Marketplace

Add this marketplace to Claude Code using one of the following methods:

**If your git works with SSH:**
```bash
/plugin marketplace add docker/claude-plugins
```

**If your git doesn't work with SSH (or prefer HTTPS):**
```bash
/plugin marketplace add https://github.com/docker/claude-plugins.git
```

### Installing the Plugin

Once the marketplace is added, you can browse available plugins and install the Docker MCP Toolkit plugin:

```bash
# Browse all plugins in the docker marketplace
/plugin

# Install the docker-mcp-toolkit plugin
/plugin install docker-mcp-toolkit@docker
```

### Troubleshooting Commands

If the MCP Gateway server isn't working as expected, the plugin provides two diagnostic slash commands to help identify and resolve issues:

- **`/docker-mcp-toolkit:gateway-status`** - Quick health check showing enabled servers and available tools
- **`/docker-mcp-toolkit:gateway-debug`** - Comprehensive diagnostics to troubleshoot installation and configuration problems

These commands will automatically check your Docker MCP Gateway setup and provide actionable recommendations if problems are found.

### Verifying Installation

Once installed, the Docker MCP Gateway will be available as an MCP server in Claude Code. You can verify by:

1. Running `/docker-mcp-toolkit:gateway-status` to see enabled servers and tool count
2. Checking that MCP tools are available in Claude's toolkit
3. Using Claude to interact with your configured MCP servers

## Development

To test changes locally:

1. Make modifications to the plugin files
2. If the marketplace is already added, update it:
   ```bash
   /plugin marketplace update docker
   ```
3. Reinstall or update the plugin if needed

## License

MIT

---
description: Debug Docker MCP Gateway installation and configuration
allowed-tools: Bash(docker:*), Bash(which:*)
---

# Docker MCP Gateway Debug

Run comprehensive diagnostics on the Docker MCP Gateway setup to identify and resolve configuration issues.

## System Checks

### 1. Docker Installation
- **Docker command available**: !`which docker`
- **Docker daemon status**: !`docker ps 2>&1 | head -5`

### 2. Docker MCP CLI Installation
- **Docker MCP command available**: !`docker mcp --help 2>&1 | head -5`

### 3. MCP Server Status
- **Enabled servers**: !`docker mcp server ls 2>&1`
- **Available tools**: !`docker mcp tools count 2>&1`

### 4. Configuration
- **Current config**: !`docker mcp config read 2>&1`

### 5. Gateway Test
- **Gateway command**: !`docker mcp gateway run --help 2>&1 | head -10`

## Analysis

Please analyze the above diagnostic results and provide:

### ✅ Working Components
List what is functioning correctly.

### ❌ Issues Found
Identify any problems or failures with specific error messages.

### 💡 Recommended Actions
Provide step-by-step instructions to resolve issues, such as:
- If `docker mcp` command not found: Enable Docker MCP Toolkit in Docker Desktop (Settings → Beta Features → Enable "Docker MCP Toolkit")
- If no servers enabled: Run `docker mcp catalog show docker-mcp` to see available servers, then enable with `docker mcp server enable <server-name>`
- If Docker daemon not running: Start Docker Desktop
- If permission errors: Verify user has Docker access permissions

### 📋 Next Steps
Suggest what the user should do next based on the findings.

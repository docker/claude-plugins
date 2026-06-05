---
name: mcp-server-yaml-creator
description: "Add and configure custom MCP servers in Docker MCP Gateway. Use when users want to: (1) Integrate their own MCP server into the gateway, (2) Add new servers to catalogs or profiles, (3) Set up server authentication, networking, and configuration, (4) Understand MCP server integration requirements."
---

# MCP Server YAML Creator

## Overview

Create valid server.yaml files for custom MCP servers that can be added to Docker MCP Gateway catalogs and profiles using the `--server file://server.yaml` flag.

MCP servers can be of three types:
- **server**: Containerized MCP servers (most common)
- **remote**: Remote HTTP/SSE MCP servers

## Quick Start - Creation from Templates

Start from a template for your server type:
- `assets/template-server.yaml` - Containerized MCP servers
- `assets/template-remote.yaml` - Remote HTTP/SSE servers

Copy the relevant template and customize it for your needs.

### Advanced Configuration

For complex scenarios (nested objects, conditional requirements, OAuth), consult `references/spec.md` for the complete specification.

## Core Requirements

All server.yaml files must include:

```yaml
name: server-name          # Lowercase, hyphen-separated
type: server               # or: remote
title: Display Name
description: Server capabilities and purpose (1-2 sentences)
```

## Type: "server" (Containerized)

Most common type for custom MCP servers. Requires a Docker image. If the user doesn't have a Docker image, ALWAYS guide them on the process on creating a Dockerfile, building it, and pushing it. If they already have a Dockerfile, ALWAYS guide them on building and pushing the docker image so that it can be referenced in the `image` field.

**Essential fields:**
```yaml
name: my-server
type: server
title: My Server
description: Brief description of what the server does (1-2 sentences)
image: myorg/my-server:latest
```

**Common additions:**
- `secrets`: API keys, tokens (never hardcode)
- `config`: User-configurable settings
- `env`: Environment variables (can reference config with `{{server-name.property}}`)
- `allowHosts`: Network access restrictions
- `volumes`: File system mounts (can be a named volume `"named-volume:/app/data` or mapped to a config `"{{my-server.config_path}}:/app/config"`)
- `longLived`: Keep running vs on-demand for tool calls (only enable this if there's good reason to keep the server running)
- `tools`: An optional (but very helpful) list of tools the server exposes for discovery purposes

See `assets/template-server.yaml` for a complete example.

## Type: "remote" (HTTP/SSE)

For remotely hosted MCP servers.

**Essential fields:**
```yaml
name: remote-server
type: remote
title: Remote Server
description: Brief description of what the server does (1-2 sentences)
remote:
  url: https://mcp.example.com/sse
  transport_type: sse
```

See `assets/template-remote.yaml` for a complete example.

## Configuration Schema

User-configurable settings allow users to provide values when using your server.

**Basic example:**
```yaml
config:
  - name: server-name
    description: Connection settings
    type: object
    properties:
      endpoint:
        type: string
        description: API endpoint URL
      timeout:
        type: number
        description: Timeout in seconds
    required:
      - endpoint
```

**Reference config values in environment variables:**
```yaml
env:
  - name: API_ENDPOINT
    value: "{{server-name.endpoint}}"
```

Environment variables should only ever be used for:
1. Config values that are setup in the `config` at the top level.
2. Hard-coded environment variables that the server requires.
3. Secrets should never be added here, as they are injected automatically as environment variables.

For arrays, nested objects, conditional requirements (anyOf/oneOf), see `references/spec.md`.

## Secrets and Authentication

Always use the `secrets` field for sensitive values:

```yaml
secrets:
  - name: server-name.api_key
    env: API_KEY
    example: YOUR_API_KEY
```

Secrets are automatically injected as environment variables. NEVER put a secret in the `env` field of the server yaml.

For OAuth configuration, see `references/spec.md`.

## Network Security

**Option 1: Restrict to specific hosts**
```yaml
allowHosts:
  - api.example.com:443
  - github.com:443
```

**Option 2: Disable all network**
```yaml
disableNetwork: true
```

## Usage

After creating server.yaml, add it to a profile or catalog:

```bash
# Add to a profile
docker mcp profile server add <profile-id> --server file://./server.yaml

# Create catalog with the server
docker mcp catalog create <oci-reference> --title "My Catalog" --server file://./server.yaml
```

Be sure to ask the user what they would like to do. The recommended approach would be to create a new catalog and add it to a new catalog.

If you need to understand more about what `docker mcp` commands are available, check out the docs at https://raw.githubusercontent.com/docker/mcp-gateway/refs/heads/main/docs/profiles.md

## Best Practices

1. Use lowercase-hyphenated naming: `my-custom-server`
2. Keep server descriptions short and concise (1-2 sentences maximum)
3. Always use `secrets` field for API keys/tokens, never hardcode in `env`
4. Use `allowHosts` to restrict network access
5. Provide clear descriptions for all config properties
6. Consider if any environment variables should be hard-coded rather than user-defined.
7. Use SHA256 digests for production images, tags for development
8. Mark config fields as required when necessary
9. Omit fields that are empty (e.g. `env`, `secrets`)
10. Test the server.yaml by adding it to a profile and running the gateway
11. ONLY use or show `docker mcp` commands that you've been told about. Don't make up commands.

## Resources

- **assets/**: Template examples for each server type (server, remote)
- **references/spec.md**: Complete technical specification with advanced patterns

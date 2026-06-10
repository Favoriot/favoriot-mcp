# Favoriot MCP Proxy Installation Guide

This guide explains how to install and configure the Favoriot MCP Server using the standalone proxy binaries. The proxy allows AI clients that only support `stdio` connections to seamlessly connect to a remote Favoriot MCP server running over HTTP/SSE.

## 1. Download the Proxy Binary

Download the correct pre-compiled proxy binary for your operating system from the [GitHub Releases](https://github.com/favoriot/favoriot-mcp/releases) page.

- **Windows**: `favoriot-mcp-proxy-windows.exe`
- **Linux**: `favoriot-mcp-proxy-linux`
- **macOS (Intel/x64)**: `favoriot-mcp-proxy-macos-x64`
- **macOS (Apple Silicon/M-series)**: `favoriot-mcp-proxy-macos-arm64`

Save the binary to a permanent location on your machine (e.g., `C:\mcp\favoriot-mcp-proxy-windows.exe` or `~/.local/bin/favoriot-mcp-proxy-linux`). Ensure the binary is executable on Linux (`chmod +x favoriot-mcp-proxy-linux`).

### macOS Specific Setup

If you are on a Mac, you must grant the file execution permissions and remove Apple's Gatekeeper quarantine (which might falsely flag the file as "damaged" when opening).

Open your Terminal and run these two commands (replace `/path/to/` with your actual file path):

1. **Make it executable (fixes "Permission denied" error):**
   ```bash
   chmod +x /path/to/favoriot-mcp-proxy-macos
   ```

2. **Remove quarantine flag (fixes "damaged" error):**
   ```bash
   xattr -cr /path/to/favoriot-mcp-proxy-macos
   ```

---

## 2. Configuration by AI Platform

Below are the configuration formats for the most popular AI agent platforms. Replace `/absolute/path/to/favoriot-mcp-proxy` with the actual path to the downloaded binary, and insert your `FAVORIOT_API_KEY`.

### Antigravity
**Config File**: `C:\Users\<user>\.gemini\config\mcp_config.json`
*(Note: Antigravity natively supports remote SSE, so you do not technically need the proxy if the server is remote. However, if you wish to use the proxy binary directly, use the config below)*
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

### Claude Desktop
**Config File (macOS)**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Config File (Windows)**: `%APPDATA%\Claude\claude_desktop_config.json`
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

### Codex
**Config File**: Workspace `.codex/mcp.json` or global `~/.codex/mcp.json`
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

### Cursor
**Config File**: Workspace `.cursor/mcp.json` or global `~/.cursor/mcp.json`
*(Alternatively, you can configure this directly via Cursor's settings UI under "Features" -> "MCP")*
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

### OpenClaw
**Config File**: `~/.openclaw/mcp.json`
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

### Hermes
**Config File**: `~/.hermes/mcp.json`
```json
{
  "mcpServers": {
    "favoriot": {
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": [],
      "env": {
        "FAVORIOT_API_KEY": "YOUR_FAVORIOT_API_KEY"
      }
    }
  }
}
```

---

## 3. Passing Custom Arguments

If your remote Favoriot MCP server is hosted at a custom URL or port (and not the default), you can pass the remote URL to the proxy using the `args` array in your configuration file.

Example:
```json
      "command": "/absolute/path/to/favoriot-mcp-proxy",
      "args": ["--remote-url", "https://mcp.yourdomain.com/sse"],
```

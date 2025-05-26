# How to Connect GitHub Copilot to Azure AI Agent Service via MCP in Visual Studio Code

The integration of GitHub Copilot with Azure AI Agent Service through the Model Context Protocol (MCP) empowers developers to create intelligent, context-aware applications. This guide walks you through setting up this integration in Visual Studio Code (VS Code).

---

## Prerequisites

Before you begin, ensure you have the following:

- **Visual Studio Code**: Latest version installed.
- **GitHub Copilot**: Active subscription.
- **GitHub Copilot Chat Extension**: Installed in VS Code.
- **Node.js**: Version 20 or higher.
- **Azure Account**

---

## Step 1: Setup Azure AI Agent as an MCP Server

### Step 1.1: Azure Configuration

- Configure your Azure AI Agents in Azure AI Foundry.
- Retrieve your Azure AI Project connection string and agent IDs.
- Authenticate Azure CLI: `az login`

### Step 1.2: Setting Environment Variables

Create an .env file in your project’s root:

```
PROJECT_CONNECTION_STRING=your-project-connection-string
DEFAULT_AGENT_ID=your-default-agent-id # optional
```

### Step 1.3: Server Installation and Execution

Set up a virtual environment and install dependencies:

```
uv venv
source .venv/bin/activate # macOS/Linux
.venv\Scripts\activate # Windows
uv add mcp[cli] azure-identity python-dotenv azure-ai-projects aiohttp
```

Start the MCP server:

```
uv run -m python.azure_agent_mcp_server
```

## Step 2: Setup VS Code as an MCP Client

### Step 2.1: Enable MCP Support in VS Code

> **Note**: MCP support in agent mode in VS Code is available starting from VS Code 1.99 and is currently in preview.

1. Open VS Code.
2. Enable MCP support by navigating to Settings and enabling the **chat.mcp.enabled** setting, or add the following to your `settings.json` (press `Ctrl+Shift+P` to open the Command Palette, then type "Preferences: Open Settings (JSON)" and press Enter)

   ```json
   {
     "othersettings": "othersettingsvalue",
     "chat.mcp.enabled": true,
     "othersettings": "othersettingsvalue"
   }
   ```

## Step 2.2: Configure the MCP client settings

1. Create a `.vscode/mcp.json` file in your project directory.
2. Add the following configuration:

```
{
"mcpServers": {
"azure-agent": {
"command": "uv",
"args": [
"--directory",
"/ABSOLUTE/PATH/TO/PARENT/FOLDER",
"run",
"-m",
"azure_agent_mcp_server"
],
"env": {
"PROJECT_CONNECTION_STRING": "your-project-connection-string",
"DEFAULT_AGENT_ID": "your-default-agent-id"
}
}
}
}
```

```json
{
  "servers": {
    "azureMcpServer": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@azure/mcp@latest", "server", "start"]
    }
  }
}
```

### Managing MCP Servers

- Run the **MCP: List Servers** command from the Command Palette (Ctrl+Shift+P) to view and manage configured MCP servers
- Use the **MCP: Add Server** command to add new servers through the UI
- When you open the `.vscode/mcp.json` file, VS Code shows commands to start, stop, or restart servers directly from the editor

---

## Step 4: Test the Integration

1. Open the **GitHub Copilot Chat** panel in VS Code (⌃⌘I on Mac, Ctrl+Alt+I on Windows/Linux).
2. Switch to **Agent Mode** from the dropdown menu.
3. Select the **Tools** button to view available tools and verify that **NAME** tools are listed.
4. Optionally, select or deselect the tools you want to use. You can search tools by typing in the search box.
5. Try a prompt like:

   ```
   <Prompt>
   ```

6. When a tool is invoked, you'll need to confirm the action before it runs (by default). You can use the **Continue** button dropdown options to automatically confirm specific tools for the current session, workspace, or all future invocations.

> **Tip**: You can also directly reference a tool in your prompt by typing `#` followed by the tool name. This works in all chat modes (ask, edit, and agent mode).

---

## Understanding MCP in VS Code

https://code.visualstudio.com/docs/copilot/chat/mcp-servers#_configuration-format

---

## Troubleshooting

<to add>

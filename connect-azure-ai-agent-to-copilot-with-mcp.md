# 🚀 How to Connect GitHub Copilot to Azure AI Agent Service via MCP in Visual Studio Code

The integration of GitHub Copilot with Azure AI Agent Service through the Model Context Protocol (MCP) empowers developers to create intelligent, context-aware applications. This guide walks you through setting up this integration in Visual Studio Code (VS Code).

---

## 🧰 Prerequisites

Before you begin, ensure you have the following:

* **Visual Studio Code**: Latest version installed.
* **GitHub Copilot**: Active subscription.
* **GitHub Copilot Chat Extension**: Installed in VS Code.
* **Node.js**: Version 20 or higher.
* **Azure Account**: With necessary permissions.([TECHCOMMUNITY.MICROSOFT.COM][1])

---

## 🔧 Step 1: Enable MCP Support in VS Code

1. Open VS Code.
2. Navigate to `Settings` > `Extensions` > `GitHub Copilot Chat`.
3. Enable the **Agent Mode** feature.
4. Ensure MCP support is enabled by adding the following to your `settings.json`:([GitHub][2])

   ```json
   {
     "chat.mcp.enabled": true
   }
   ```

---

## 🛠️ Step 2: Install Azure MCP Server

The Azure MCP Server acts as a bridge between GitHub Copilot and Azure AI Agent Service.([GitHub][2])

### Option 1: One-Click Install

Run the following command in your terminal:

```bash
npx -y @azure/mcp@latest server start
```

### Option 2: Manual Configuration

1. Create a `.vscode/mcp.json` file in your project directory.
2. Add the following configuration:([TECHCOMMUNITY.MICROSOFT.COM][1])

   ```json
   {
     "servers": {
       "Azure MCP Server": {
         "command": "npx",
         "args": [
           "-y",
           "@azure/mcp@latest",
           "server",
           "start"
         ]
       }
     }
   }
   ```

This setup allows VS Code to recognize and communicate with the Azure MCP Server.

---

## 🔐 Step 3: Authenticate with Azure

Ensure that your environment is authenticated with Azure to allow the MCP server to access your Azure resources.([Visual Studio Code][3])

1. Install the Azure CLI if you haven't already.
2. Run the following command to log in:

   ```bash
   az login
   ```

This command opens a browser window for you to sign in with your Azure credentials.

---

## 🧪 Step 4: Test the Integration

1. Open the **GitHub Copilot Chat** panel in VS Code.
2. Switch to **Agent Mode**.
3. You should see **Azure MCP Server** listed under available tools.
4. Try a prompt like:([GitHub][2])

   ```
   List my Azure Storage containers.
   ```

GitHub Copilot will utilize the Azure MCP Server to fetch and display your Azure Storage containers.([GitHub][2])

---

## 🧠 Understanding MCP

The Model Context Protocol (MCP) is an open standard that enables AI models to interact with external tools and services through a unified interface. By integrating MCP, GitHub Copilot can access Azure services, enhancing its capabilities to assist in development tasks. ([Visual Studio Code][3])

---

## 📚 Additional Resources

* **Azure MCP Server GitHub Repository**: [https://github.com/Azure/azure-mcp](https://github.com/Azure/azure-mcp)
* **VS Code MCP Server Documentation**: [https://code.visualstudio.com/docs/copilot/chat/mcp-servers](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)
* **Azure AI Foundry Blog**: [https://azure.microsoft.com/en-us/blog/azure-ai-foundry-your-ai-app-and-agent-factory/](https://azure.microsoft.com/en-us/blog/azure-ai-foundry-your-ai-app-and-agent-factory/)([Visual Studio Code][3])

---

By following these steps, you can harness the power of GitHub Copilot and Azure AI Agent Service to build intelligent, context-aware applications directly within Visual Studio Code.

---

[1]: https://techcommunity.microsoft.com/blog/azuredevcommunityblog/getting-started-with-azure-mcp-server-a-guide-for-developers/4408974?utm_source=chatgpt.com "Getting Started with Azure MCP Server: A Guide for Developers"
[2]: https://github.com/Azure/azure-mcp?utm_source=chatgpt.com "The Azure MCP Server, bringing the power of Azure to your agents."
[3]: https://code.visualstudio.com/docs/copilot/chat/mcp-servers?utm_source=chatgpt.com "Use MCP servers in VS Code (Preview)"

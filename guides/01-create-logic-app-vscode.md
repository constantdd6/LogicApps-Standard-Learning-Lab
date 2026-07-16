# Creating Your First Logic App in Visual Studio Code

A comprehensive guide to setting up and creating a Logic App Standard workflow in VS Code.

## Prerequisites

Before starting, ensure you have:
- Azure subscription (free tier available)
- Visual Studio Code installed
- Azure Account extension for VS Code
- Node.js 14.x or later
- Azure CLI (optional but recommended)
- Azure Logic Apps (Standard) extension

## Step 1: Install Required Extensions

1. Open **Visual Studio Code**
2. Click on the **Extensions** icon (or press `Ctrl+Shift+X`)
3. Search for and install these extensions:
   - **Azure Account** - Microsoft official Azure integration
   - **Azure Logic Apps (Standard)** - Official Logic Apps extension
   - **Azure Tools** - Additional Azure utilities

```
Recommended Extensions:
- ms-vscode.azure-account
- Microsoft.vscode-azurelogicapps
- ms-vscode.vscode-node-azure-pack
```

## Step 2: Sign In to Azure

1. Open the **Command Palette** (`Ctrl+Shift+P` on Windows/Linux, `Cmd+Shift+P` on Mac)
2. Type `Azure: Sign In` and press Enter
3. Your browser will open - sign in with your Azure account
4. Authorize VS Code to access your Azure resources
5. Return to VS Code - you should see your subscription in the Azure panel

## Step 3: Create a New Logic App Project

### Option A: Using Command Palette

1. Open **Command Palette** (`Ctrl+Shift+P`)
2. Type `Azure Logic Apps: Create new project` and press Enter
3. Select a folder for your project
4. Choose your workflow type:
   - **Stateful** - Maintains execution history
   - **Stateless** - Lightweight, no history storage

### Option B: Using File Explorer

1. Click on the **Azure** icon in the Activity Bar (left sidebar)
2. Expand your subscription
3. Right-click on **Logic Apps** → **Create New Logic App (Standard)**
4. Follow the prompts to name your project

## Step 4: Project Structure Overview

After creation, your project will look like this:

```
my-logic-app/
├── .vscode/
│   └── settings.json
├── .funcignore
├── .gitignore
├── connections.json          # Connection references
├── host.json                 # Runtime configuration
├── local.settings.json       # Local settings (don't commit)
├── workflow.json             # Your workflow definition
└── package.json              # Dependencies
```

**Important Files:**
- `workflow.json` - Contains your logic app workflow definition
- `connections.json` - References to connectors (HTTP, Service Bus, Storage, etc.)
- `local.settings.json` - Local development settings

## Step 5: Open the Workflow Designer

1. Locate the `workflow.json` file in your project
2. Right-click on `workflow.json`
3. Select **Open in Designer** (or **Logic App Designer**)
4. The visual designer will open in VS Code

**Note:** The visual designer may also open in your browser for better UI experience.

## Step 6: Add Your First Trigger (HTTP Trigger Example)

1. In the designer, click **+ New Step**
2. Search for **HTTP**
3. Select **HTTP** trigger
4. Configure:
   - **Method:** POST
   - **URL:** Leave blank (will be generated)
5. Click **Save** to generate the HTTP endpoint

### HTTP Trigger Configuration

```json
{
  "type": "Request",
  "kind": "Http",
  "inputs": {
    "method": "POST",
    "schema": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string"
        },
        "email": {
          "type": "string"
        }
      }
    }
  }
}
```

## Step 7: Add an Action

1. Click **+ New Step** below the trigger
2. Search for your desired action (e.g., **Compose** for data transformation)
3. Configure the action inputs
4. Click **Save**

### Example: Add a Compose Action

1. Click **+ New Step**
2. Search for **Compose**
3. In the **Inputs** field, enter dynamic content from the trigger:
   ```
   @{triggerBody()?['name']}
   ```
4. Save the workflow

## Step 8: Add Response Action

1. Click **+ New Step**
2. Search for **Response**
3. Configure:
   - **Status Code:** 200
   - **Body:** Add output from compose action
4. Save

## Step 9: Test Your Workflow Locally

### Start the Local Runtime

1. Open **Terminal** in VS Code (`Ctrl+` `)
2. Run:
   ```bash
   func start
   ```
3. Wait for the message: `HTTP Functions:`
4. Copy the HTTP POST URL (e.g., `http://localhost:7071/api/...`)

### Test the Trigger

1. Open **Postman** or use `curl` in terminal:
   ```bash
   curl -X POST http://localhost:7071/api/workflows/MyLogicApp/triggers/Request/invoke \
     -H "Content-Type: application/json" \
     -d '{"name":"John","email":"john@example.com"}'
   ```

2. Check the response - should return status 200 with your composed data

## Step 10: Configure Connections (Service Bus, Storage, etc.)

### Add a Service Bus Connection

1. In the designer, click **+ New Step**
2. Search for **Service Bus**
3. Select **When a message is received in a queue**
4. Click **Sign in** to authenticate
5. Select your:
   - **Subscription**
   - **Service Bus Namespace**
   - **Queue Name**
6. Save

### Environment Variables Setup

Update `local.settings.json`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "FUNCTIONS_EXTENSION_VERSION": "~4",
    "SERVICE_BUS_CONNECTION_STRING": "your-connection-string",
    "STORAGE_ACCOUNT_CONNECTION_STRING": "your-storage-connection-string"
  },
  "ConnectionReferences": {
    "serviceBusConnection": {
      "connectionName": "servicebus",
      "source": "Azure"
    }
  }
}
```

## Step 11: Deploy to Azure

### Create Resource Group (if needed)

```bash
az group create --name my-logic-apps-rg --location eastus
```

### Deploy the Logic App

1. In the **Azure** panel, expand your subscription
2. Right-click on **Logic Apps** → **Deploy to Logic App**
3. Select or create a new Logic App resource
4. Choose your resource group
5. Wait for deployment to complete

### Monitor Deployment

```bash
az logicapp show --name my-logic-app --resource-group my-logic-apps-rg
```

## Step 12: Monitor and Debug

### View Execution History

1. In the **Azure** panel, find your Logic App
2. Right-click → **Open in Portal** (or **View in Portal**)
3. Click **Runs** to see execution history
4. Click on any run to see details

### Local Debugging

1. Set breakpoints in `workflow.json`
2. Use **Run** → **Start Debugging** (F5)
3. Step through actions

## Best Practices

✅ **Do:**
- Use version control (commit `workflow.json`)
- Store secrets in Azure Key Vault
- Use meaningful names for actions and variables
- Add error handling with Scopes
- Test locally before deploying

❌ **Don't:**
- Commit `local.settings.json` (add to `.gitignore`)
- Hardcode connection strings
- Skip error handling
- Deploy without testing

## Common Issues & Solutions

### Issue: "Extensions not found"
**Solution:** Run `func extensions install` in terminal

### Issue: "Connection failed during local testing"
**Solution:** Verify connection strings in `local.settings.json`

### Issue: "Port 7071 already in use"
**Solution:** 
```bash
func start --port 7072
```

### Issue: "Cannot find module 'azure-functions'"
**Solution:**
```bash
npm install
```

## Next Steps

- Add error handling with Scopes (see: `02-error-handling.md`)
- Implement retry policies (see: `03-retry-policies.md`)
- Create multiple triggers (see: `04-multiple-triggers.md`)
- Deploy to production

## Resources

- [Azure Logic Apps Documentation](https://docs.microsoft.com/azure/logic-apps/)
- [Logic Apps Standard in VS Code](https://docs.microsoft.com/azure/logic-apps/create-single-tenant-workflows-visual-studio-code)
- [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local)
- [Logic Apps Extension Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azurelogicapps)

---

Happy coding! 🚀

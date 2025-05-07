# Azure Resourge Managment template for ot-agent

## Deploy via Azure Portal

You can deploy this template directly in the Azure Portal using the following link:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fteebu%2Fms-ot%2Fmain%2Fresources%2Fot_agent_arm_template.json)

# validate

```sh
az deployment group validate \
--resource-group "BOND" \
--template-file ot_agent_arm_template.json \
--parameters @ot_agent.parameters.json
```

# create

```sh
az deployment group create \
 --resource-group BOND \
 --template-file ot_agent_arm_template.json \
 --parameters \
 logicAppName=TESTARM-logicapp \
 keyVaultName=TESTARM-ot-kv \
 token="Bearer <token>" \
 host="https://uat.onetrust.com"
```

```sh
az deployment group create \
 --resource-group BOND \
 --template-file ot_agent_arm_template.json \
 --parameters @ot_agent.parameters.json
```

### note about arm template and managed connectors:

You cannot fully create and authorize Logic Apps managed connectors (like Key Vault, Office 365, etc.) using only ARM templates—especially when using managed identity authentication.

Key points:

ARM templates can create the Microsoft.Web/connections resource, but this only creates a placeholder.
The actual authorization (binding the connector to a managed identity or user credentials) must be done interactively (via the Azure Portal, Logic App Designer, or programmatically via REST API/PowerShell).
For managed identity, the best practice is to use the built-in HTTP action (as you are now) or the built-in connector (if available), not the ARM-created connection.

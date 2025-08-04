# Azure Resourge Managment template for ot-agent

## Deploy via Azure Portal

You can deploy this template directly in the Azure Portal using the following link:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fteebu%2Fms-ot%2Fmain%2FTools%2FPrivacyBreach%2Fresources%2Fot_agent_arm_template.json)

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

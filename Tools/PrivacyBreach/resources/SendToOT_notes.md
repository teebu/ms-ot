# LogicApp Schema:

```json
{
  "type": "object",
  "properties": {
    "payload": {
      "type": "string"
    },
    "url": {
      "type": "string"
    }
  },
  "required": ["payload", "path"]
}
```

example host:
`https://uat.onetrust.com`

example path:
`/api/incident/v1/incidents`

example payloads:

Detected PII Sample

```json
{
  "payload": "{\"TenantId\":\"f96a0792-d1da-476d-9fda-bcce77b5e672\",\"TimeGenerated\":\"2025-02-11T19:26:52.4538212Z\",\"PurviewTenantId\":\"714a1b55-6ae7-46e7-aa08-8978b2c8746c\",\"PurviewAccountName\":\"Purtoso\",\"PurviewRegion\":\"eastus2\",\"SourceName\":\"AzureSqlDatabase-Crl\",\"SourceType\":\"AzureSqlDatabase\",\"SourcePath\":\"/subscriptions/8a081f00-1372-4400-9886-a704be213b86/resourceGroups/BOND/providers/Microsoft.Sql/servers/bondsql\",\"SourceSubscriptionId\":\"8a081f00-1372-4400-9886-a704be213b86\",\"SourceRegion\":\"eastus\",\"SourceCollectionName\":\"purtoso\",\"SourceScanId\":\"602daf06-2336-4b42-8e51-980f78257cc5\",\"AssetName\":\"CustomerFlights17\",\"AssetPath\":\"mssql://bondsql.database.windows.net/CustomerDB/dbo/CustomerFlights17\",\"AssetType\":\"Table\",\"AssetCreationTime\":\"2025-02-11T19:22:35Z\",\"AssetModifiedTime\":\"2025-02-11T19:22:35Z\",\"AssetLastScanTime\":\"2025-02-11T19:23:45.1595015Z\",\"FileExtension\":\"\",\"FileSize\":0,\"ActivityType\":\"Classification\",\"ClassificationTrigger\":\"System\",\"Classification\":[\"MICROSOFT.GOVERNMENT.CITY_NAME\",\"MICROSOFT.GOVERNMENT.US.STATE\",\"MICROSOFT.GOVERNMENT.US.ZIP_CODE\",\"MICROSOFT.PERSONAL.EMAIL\",\"MICROSOFT.PERSONAL.GENDER\",\"MICROSOFT.PERSONAL.NAME\",\"MICROSOFT.PERSONAL.PHYSICALADDRESS\",\"MICROSOFT.PERSONAL.US.PHONE_NUMBER\",\"Taiwan Passport Number\"],\"ClassificationDetails\":[{\"Id\":\"c86fc3df-7985-6114-f187-9c5eca581bdf\",\"Name\":\"MICROSOFT.GOVERNMENT.CITY_NAME\",\"Confidence\":86,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"ee6e068c-0b84-2813-2cb3-24c189b90be2\",\"Name\":\"MICROSOFT.GOVERNMENT.US.STATE\",\"Confidence\":90,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"00566c33-9227-b4a0-0f13-2596ac2c1063\",\"Name\":\"MICROSOFT.GOVERNMENT.US.ZIP_CODE\",\"Confidence\":87,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"1dab37e2-213f-a88d-7e13-064af9561e51\",\"Name\":\"MICROSOFT.PERSONAL.EMAIL\",\"Confidence\":87,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"69b7b5fe-7451-c80a-4857-59d62eba3c3d\",\"Name\":\"MICROSOFT.PERSONAL.GENDER\",\"Confidence\":100,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"50b8b56b-4ef8-44c2-a924-03374f5831ce\",\"Name\":\"MICROSOFT.PERSONAL.NAME\",\"Confidence\":92,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"8548332d-6d71-41f8-97db-cc3b5fa544e6\",\"Name\":\"MICROSOFT.PERSONAL.PHYSICALADDRESS\",\"Confidence\":85,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"46b001f5-a4ce-0497-8177-7b13916bb6fa\",\"Name\":\"MICROSOFT.PERSONAL.US.PHONE_NUMBER\",\"Confidence\":87,\"Count\":1,\"UniqueCount\":1},{\"Id\":\"e7251cb4-4c2c-41df-963e-924eb3dae04a\",\"Name\":\"Taiwan Passport Number\",\"Confidence\":90,\"Count\":128,\"UniqueCount\":128}],\"SensitivityLabelTrigger\":\"\",\"SensitivityLabel\":[],\"SensitivityLabelDetails\":[],\"SourceSystem\":\"\",\"Type\":\"PurviewDataSensitivityLogs\",\"_ResourceId\":\"/subscriptions/8a081f00-1372-4400-9886-a704be213b86/resourcegroups/kj-purview-preview/providers/microsoft.purview/accounts/purtoso\",\"AssetPath1\":\"mssql://bondsql.database.windows.net/CustomerDB/dbo/CustomerFlights17\",\"minScanTime\":\"2025-02-11T19:23:45.1595015Z\"}",
  "path": "/integrationmanager/api/v1/webhook/5c22522a-95a9-4d0e-bd3d-341b834f44a7"
}
```

Incident Breach sample:

```json
{
  "payload": "{\"incidentTypeName\":\"Unauthorized Disclosure of Information\",\"name\":\"Azure SQL Security Breach\",\"sourceType\":\"INTEGRATION\",\"jurisdictions\":[{\"countryCode\":\"FRA\"}]}",
  "path": "/api/incident/v1/incidents"
}
```

### KV keys:

- `token`: bearer token ie: `Bearer <token>`
- `host`: OT host ie: `https://yourcompany.onetrust.com`

### notes:

Uses managed identity with `Key Vault User` permission to read secrets from kv (needs to be created in RG IAM) and assigned to the logicapp

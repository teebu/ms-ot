# OneTrust Privacy Breach Response

Sample text with multiple regions:

```
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France, Germany and California. Who do I need to notify?
```

Sample text with one region:

```
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France. Who do I need to notify?
```

Sample text to not show answer, just create incident record in OneTrust:

```
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France, Germany and California.
```

Example prompts:
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France. Who do I need to notify?
There was a breach in databricks containing 1000 social security numbers of Data Subjects from Canada
There was a breach in snowflake containing 60 emails of Data Subjects from California. Who do I need to notify?
There was a breach in databricks containing 10 social security numbers of Data Subjects from UK, Germany, Greece, Ireland

Sample generated payload sent to OneTrust:

```json
{
  "incidentTypeName": "Unauthorized Disclosure of Information",
  "name": "Databricks Security Breach",
  "sourceType": "INTEGRATION",
  "jurisdictions": [{ "countryCode": "GBR" }, { "countryCode": "DEU" }, { "countryCode": "GRC" }, { "countryCode": "IRL" }],
  "autoAssessJurisdictions": true,
  "linkedInventories": {
    "assets": {
      "externalIds": ["Databricks"]
    }
  }
}
```

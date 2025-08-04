https://developer.onetrust.com/onetrust/reference/getgroupedpurposesusingget
https://developer.onetrust.com/onetrust/reference/getpurposedetailusingget

_note: schema missing 'type' argument in `api/consentmanager/v2/purposes` definitions_

### consent questions:

What are my approved purposes for processing data?
-> returns active purposes

What are my purposes for processing data?
-> returns active and draft purposes

What are my active purposes for processing data for cookie type?
-> returns active and cookie types

using onetrust consent, give me details about 39d0c533-a82b-4bcc-81c6-991057cef639 purpose
-> returns details about that specific purpose

@OneTrust Consent Manager Agent give me details about 39d0c533-a82b-4bcc-81c6-991057cef639 purpose
-> Exception: Microsoft.Medeina.Skills.Runtime.Exceptions.SkillExceptions.FatalSkillInvocationException: Get request to https://uat.onetrust.com/api/consentmanager/v2/purposes/%7BpurposeId%7D failed with with HTTP status: 400 BadRequest and exception: System.Net.Http.HttpRequestException: Request failed with status code BadRequest

@OneTrust Consent Tool give me details about 39d0c533-a82b-4bcc-81c6-991057cef639 purpose
-> gets details

promptbook for single consent purpose details (uses list and grabs id from results) :
input -> consent name of active purpose within the page limit ("Email Marketing")

@OneTrust Consent Manager Agent give me details about Email Marketing purpose
-> runs getGroupedPurposesUsingGET and gets info from the response (doesn't run extra details step)

what are the data elements that are approved for that purpose?
todo: what api is this?

### notes

enable Agents and AgentView flags:
https://securitycopilot.microsoft.com/?featureFlags=Agents,AgentView

# terms

![](diagram_terms.png)

# outline

![](diagram.png)

```json
{
  "IT_OWNER": "joe@example.app",
  "SourceName": "AzureSqlDatabase-UjS",
  "SourceType": "AzureSqlDatabase",
  "SourceRegion": "northeurope",
  "AssetName": "new_customerdata",
  "AssetType": "Table",
  "AssetPath": "mssql://ecohackssqlsvr.database.windows.net/ecohacksSQL/dbo/new_customerdata",
  "Classification": [
    "MICROSOFT.GOVERNMENT.US.STATE",
    "MICROSOFT.PERSONAL.NAME",
    "MICROSOFT.PERSONAL.US.PHONE_NUMBER",
    "U.S. Social Security Number (SSN)"
  ],
  "Classificationdetails": [
    {
      "Id": "ee6e068c-0b84-2813-2cb3-24c189b90be2",
      "Name": "MICROSOFT.GOVERNMENT.US.STATE",
      "Confidence": 90,
      "Count": 1,
      "UniqueCount": 1
    },
    {
      "Id": "50b8b56b-4ef8-44c2-a924-03374f5831ce",
      "Name": "MICROSOFT.PERSONAL.NAME",
      "Confidence": 85,
      "Count": 1,
      "UniqueCount": 1
    },
    {
      "Id": "46b001f5-a4ce-0497-8177-7b13916bb6fa",
      "Name": "MICROSOFT.PERSONAL.US.PHONE_NUMBER",
      "Confidence": 86,
      "Count": 1,
      "UniqueCount": 1
    },
    {
      "Id": "a44669fe-0d48-453d-a9b1-2cc83f2cba77",
      "Name": "U.S. Social Security Number (SSN)",
      "Confidence": 88,
      "Count": 108,
      "UniqueCount": 108
    }
  ]
}
```

---

to kick off flow of generated pii detected find the agent:
@ Send Privacy Payload to OneTrust

# Logic Apps

### SendToOT

Forward payload to OT host with payload string and url (/example/create) endpoint.
Using secrets 'token' and 'host' stored in kv and populatd at runtime.

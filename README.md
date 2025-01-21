https://developer.onetrust.com/onetrust/reference/getgroupedpurposesusingget
https://developer.onetrust.com/onetrust/reference/getpurposedetailusingget

*note: schema missing 'type' argument in `api/consentmanager/v2/purposes` definitions* 

### consent questions:
What are my approved purposes for processing data?
  -> returns active purposes

What are my purposes for processing data?
  -> returns active and draft purposes

What are my active purposes for processing data for cookie type?
  -> returns active and cookie types

using onetrust consent, give me details about 39d0c533-a82b-4bcc-81c6-991057cef639 purpose
  -> returns details about that specific purpose

promptbook for single consent purpose details (uses list and grabs id from results) :
input -> consent name of active purpose within the page limit ("Email Marketing")

what are the data elements that are approved for that purpose?
todo: what api is this?


### notes
enable Agents and AgentView flags:
https://securitycopilot.microsoft.com/?featureFlags=Agents,AgentView

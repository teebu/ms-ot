# Privacy Risk Agent for Microsoft Security Copilot

## Overview

The **Privacy Risk Agent** is an automated compliance solution that integrates **Microsoft Security Copilot**, **Microsoft Purview**, and **OneTrust** to detect new instances of personal data and collect regulatory context for privacy risk analysis.

This agent monitors data sensitivity classifications from Microsoft Purview, enriches them with contextual information from Microsoft Entra ID and Purview metadata, and automatically submits PII (Personally Identifiable Information) findings to OneTrust for risk assessment and regulatory compliance tracking.

## Purpose

The Privacy Risk Agent automates the privacy compliance workflow by:

1. **Detecting Personal Data**: Continuously monitors Microsoft Purview data sensitivity logs for newly classified personal information
2. **Enriching Context**: Automatically retrieves asset owners and contact information from Microsoft Purview and Entra ID
3. **Regulatory Submission**: Submits enriched PII classification data to OneTrust for privacy risk analysis and compliance review
4. **Automated Processing**: Runs on a configurable schedule to ensure timely detection and submission of privacy-sensitive data

## Technology Stack

### Microsoft Security Copilot
- **Agent Framework**: Implements a custom Security Copilot agent with trigger-based automation
- **Multi-Tenancy**: Supports deployment across multiple organizations
- **KQL Skills**: Uses Kusto Query Language (KQL) to query Microsoft Sentinel data lakes
- **Agent Skills**: Orchestrates complex workflows with built-in reasoning and error handling

### Microsoft Purview
- **Data Sensitivity Classification**: Sources PII classifications from Purview's data discovery engine
- **Asset Metadata**: Retrieves asset ownership and contact information via Purview APIs
- **Compliance Integration**: Leverages Microsoft's native classification taxonomy (MICROSOFT.PERSONAL.EMAIL, etc.)

### Microsoft Sentinel
- **Data Lake**: Stores Purview logs in custom table `UniquePurviewTable_SPRK`
- **Entra ID Integration**: Queries `EntraUsers` table for user contact information
- **Analytics Jobs**: Runs scheduled Spark notebook jobs to sync Purview data

### OneTrust
- **Privacy Risk Management**: Receives enriched PII classification records
- **API Integration**: Uses OneTrust Data Discovery Platform API for scan data submission
- **Bearer Token Authentication**: Secured via API key authentication

## Architecture

```
Microsoft Purview (Data Classification)
    ↓
Microsoft Sentinel (Analytics Job - Daily Sync)
    ↓
UniquePurviewTable_SPRK (Custom Table)
    ↓
Security Copilot Agent (Trigger Every 30s)
    ↓
├─ Query Purview Logs (KQL)
├─ Enrich with Asset Metadata (Purview API)
├─ Resolve Contact Info (Entra ID)
└─ Submit to OneTrust (API)
```

## Components

### 1. Agent Manifest ([AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml))
Defines the Security Copilot agent with:
- **Trigger**: `PurviewLogTrigger` - Polls every 30 seconds for new Purview logs
- **Fetch Skill**: `GetPurviewLogsFromMSG` - Retrieves 5 new records from Sentinel
- **Process Skill**: `PurviewLogSubmissionAgent` - Enriches and submits each record
- **Child Skills**:
  - `GetEntraUsersFromMSGByID` - Resolves user emails from Entra ID
  - `scanData` - Submits to OneTrust webhook
  - `purview_search_asset` - Retrieves asset metadata from Purview

### 2. Package Manifest ([PrivacyRiskAgentManifest.package.yaml](PrivacyRiskAgentManifest.package.yaml))
Packaging configuration for the Security Copilot extension:
- **Package Name**: PrivacyRiskAgent
- **Include Paths**: Bundles the entire `PrivacyRiskAgent` directory

### 3. OpenAPI Specification ([openapispec_1.yaml](PrivacyRiskAgent/openapispec_1.yaml))
Defines the OneTrust API integration:
- **Endpoint**: `/api/discovery-platform/v1/scan-data`
- **Method**: POST
- **Schema**: Classification records with asset metadata, PII details, and ownership info

### 4. Analytics Job ([onetrust_purviewlog_sentinel.job.yaml](PrivacyRiskAgent/AnalyticsJobPurviewLog/onetrust_purviewlog_sentinel/onetrust_purviewlog_sentinel.job.yaml))
Sentinel scheduled job configuration:
- **Type**: Spark Notebook
- **Schedule**: Daily execution
- **Purpose**: Syncs Purview data sensitivity logs to custom Sentinel table

## Deployment

### Prerequisites

1. **Microsoft Security Copilot** environment with agent support
2. **Microsoft Purview** with data sensitivity classification enabled
3. **Microsoft Sentinel** workspace with:
   - Custom table `UniquePurviewTable_SPRK` configured
   - `EntraUsers` table accessible
4. **OneTrust** account with:
   - Data Discovery Platform API access
   - API bearer token
5. **Visual Studio Code** with [Microsoft Sentinel extension](https://marketplace.visualstudio.com/items?itemName=microsoftsentinelcommunity.vscode-sentinel) installed (required for marketplace publishing)

### Common Setup Steps

These steps are required for both deployment methods:

#### Step 1: Configure OneTrust Credentials

1. Obtain your OneTrust API bearer token
2. Note your OneTrust organization URL (e.g., `https://yourorg.onetrust.com`)

#### Step 2: Deploy Analytics Job

1. Open the [onetrust_purviewlog_sentinel.job.yaml](PrivacyRiskAgent/AnalyticsJobPurviewLog/onetrust_purviewlog_sentinel/onetrust_purviewlog_sentinel.job.yaml) in Sentinel
2. Deploy the notebook job to your Sentinel workspace
3. Verify the job runs successfully and populates `UniquePurviewTable_SPRK`

---

## Deployment Option 1: Manual Testing / Development

Use this method for development, testing, or direct organizational deployment without marketplace distribution.

### Steps

#### 1. Upload Agent Manifest to Security Copilot

1. Navigate to **Microsoft Security Copilot** portal
2. Go to **Settings** → **Plugins** → **Upload custom plugin**
3. Upload the [AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml) file
4. Ensure the [openapispec_1.yaml](PrivacyRiskAgent/openapispec_1.yaml) file is in the same directory as the manifest (the manifest references it by filename)

#### 2. Configure Plugin Settings

1. In Security Copilot, configure the plugin settings:
   - **OneTrust API URL**: Your organization URL (e.g., `https://yourorg.onetrust.com`)
   - **Authorization**: Your OneTrust API bearer token

#### 3. Enable the Agent

1. Go to **Security Copilot** → **Agents**
2. Find **Privacy Risk Agent** in the agent list
3. Click **Deploy** to make it available
4. Enable the agent for your organization/users
5. Verify the timer trigger is active (polls every 30 seconds)

---

## Deployment Option 2: Publishing to Microsoft Marketplace

Use this method to distribute the agent publicly via the Microsoft Marketplace.

### Steps

#### 1. Create Agent Package

Using the **Microsoft Sentinel extension** for VS Code:

1. Open the project folder in VS Code
2. Open the Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P`)
3. Run command: **"Microsoft Sentinel: Package Agent"**
4. Select the [PrivacyRiskAgentManifest.package.yaml](PrivacyRiskAgentManifest.package.yaml) file
5. The extension will create `PrivacyRiskAgent.zip` containing:
   - [AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml)
   - [openapispec_1.yaml](PrivacyRiskAgent/openapispec_1.yaml)
   - All supporting files from the `PrivacyRiskAgent` directory

**Important**: The `OpenApiSpecUrl` in [AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml#L47) must use the filename `openapispec_1.yaml` (not a URL) for the packaged zip to work correctly.

#### 2. Submit to Microsoft Marketplace

1. Submit the `PrivacyRiskAgent.zip` package through the Microsoft Partner Center
2. Complete the marketplace listing with:
   - Product description
   - Screenshots and documentation
   - Pricing information (if applicable)
   - Support contacts
3. Wait for Microsoft approval and publication

View the published agent: [OneTrust Privacy Risk Agent - Microsoft Marketplace](https://marketplace.microsoft.com/en-us/product/saas/onetrustllc1594047340198.ot_privacy_risk_agent?tab=Overview)

#### 3. Install from Marketplace

Once published, users can:

1. Navigate to the [marketplace listing](https://marketplace.microsoft.com/en-us/product/saas/onetrustllc1594047340198.ot_privacy_risk_agent?tab=Overview)
2. Click **Get It Now** or **Install**
3. Follow the installation wizard to add it to their Security Copilot environment
4. Configure OneTrust API credentials in the plugin settings
5. Enable the agent for their organization

---

### Configuration Options

You can customize the agent behavior by modifying:

- **Poll Period**: Change `DefaultPollPeriodSeconds` in [AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml#L37) (default: 30 seconds)
- **Batch Size**: Modify the `take` value in the KQL query at [AgentManifest.yaml](PrivacyRiskAgent/AgentManifest.yaml#L66) (default: 5 records)
- **Schedule**: Update the analytics job schedule in [onetrust_purviewlog_sentinel.job.yaml](PrivacyRiskAgent/AnalyticsJobPurviewLog/onetrust_purviewlog_sentinel/onetrust_purviewlog_sentinel.job.yaml#L8-L9) (default: daily)

## Data Flow Example

1. **Purview classifies** an Azure SQL table column as containing `MICROSOFT.PERSONAL.EMAIL`
2. **Sentinel notebook** syncs the classification to `UniquePurviewTable_SPRK` daily
3. **Agent trigger fires** every 30 seconds, fetching 5 new records
4. **For each record**, the agent:
   - Queries Purview API for asset metadata (owner/expert contacts)
   - Resolves contact ID to email address via Entra ID
   - Prepares enriched payload with IT owner, asset details, and classification confidence
   - Submits to OneTrust `/scan-data` endpoint
5. **OneTrust** receives the record for privacy risk assessment and compliance review

## Agent Workflow

The `PurviewLogSubmissionAgent` performs these steps for each Purview log:

1. **Extract Asset Path** from the log record
2. **Search Purview** using `purview_search_asset` skill to find asset metadata
3. **Extract Contact ID** with priority: Owner > Expert
4. **Resolve to Email** by calling `GetEntraUsersFromMSGByID` if contact ID is a UUID
5. **Prepare Payload** with enriched data (defaults to `null` if contact info unavailable)
6. **Submit to OneTrust** via `scanData` skill
7. **Error Handling**: Retries once on failure, logs any skipped records

## Useful Links

- [OneTrust Privacy Risk Agent - Microsoft Marketplace](https://marketplace.microsoft.com/en-us/product/saas/onetrustllc1594047340198.ot_privacy_risk_agent?tab=Overview)
- [Microsoft Security Copilot Documentation](https://learn.microsoft.com/en-us/security-copilot/)
- [Microsoft Purview Data Loss Prevention](https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp)
- [Microsoft Sentinel Analytics Rules](https://learn.microsoft.com/en-us/azure/sentinel/detect-threats-custom)
- [Microsoft Sentinel VS Code Extension](https://marketplace.visualstudio.com/items?itemName=microsoftsentinelcommunity.vscode-sentinel)
- [OneTrust Data Discovery API](https://developer.onetrust.com/)
- [Kusto Query Language (KQL) Reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)

## Troubleshooting

### Agent Not Processing Records
- Verify the Sentinel notebook job is running and populating `UniquePurviewTable_SPRK`
- Check the trigger is enabled in Security Copilot agent settings
- Review agent execution logs for errors

### Authentication Errors
- Confirm OneTrust API token is valid and has correct permissions
- Verify the organization URL matches your OneTrust instance

### Missing IT Owner Information
- Check if Purview assets have owner/expert contacts configured
- Verify Entra ID user IDs are valid and accessible
- The agent will use `null` as default if contacts cannot be resolved

## License

This project is provided as-is for integration between Microsoft Security Copilot, Microsoft Purview, and OneTrust.

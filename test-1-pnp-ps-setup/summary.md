# Overview

This test evaluates how well Copilot performs in setting up you local environment for using PnP PowerShell. Each test was started on a clean setup with no PnP PowerShell installation or configuration. The test was run with the same prompt for each of the different Copilot customizations, and the results were evaluated based on the criteria outlined below.

## Criteria

Each Copilot run was evaluated based on the following criteria:

Must have criteria (required to be considered successful):
- Copilot installed the latest version of PnP PowerShell. The installed version needs to be 3.0 or higher.
- Copilot created an app registration in Entra ID that will be used by PnP PowerShell in order to make the connection
- Copilot created a connection to a SharePoint Site

Nice to have criteria (not required, but would be a plus):
- Copilot validated if if PowerShell version is not below 7.4.0 and in case it is it informed the user to update PowerShell to the latest version
- Copilot proactively asked if the user if the connection that will be created is for interactive login (delegated) or to be used by an application
- Copilot asked for permission scopes that should added to the app registration 
- Copilot validated the created connection

## Test

Each test was run 3 times to ensure consistency of the results. The tests were run with the same prompt for each of the different Copilot customizations. Each run was with the following prompt:

```
Prepare my local environment for using PnP PowerShell. Install whatever is needed and create a connection to a SharePoint site "https://tenanttocheck.sharepoint.com/sites/PnP-PowerShell-Agent-Test".
```

### Baseline - Clean Copilot without any customizations - Claude Sonnet 4.6

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ |
| #1  | ❌ 1/3             | ❌ 1/4               | 157,032      | 2,759         | 146,231       | 12.58   | 9          | 9           | 0      |
| #2  |                    |                       |              |               |               |         |            |             |        |
| #3  |                    |                       |              |               |               |         |            |             |        |

//🏗️ In Progress

### Context7 - Copilot with Context7 MCP - Claude Sonnet 4.6

// TODO

### PnP PowerShell Copilot Plugin - Copilot with the PnP PowerShell Copilot Plugin - Claude Sonnet 4.6

// TODO

### PnP PowerShell MCP - Copilot with the PnP PowerShell MCP - Claude Sonnet 4.6

// TODO - confirm if it is usable with the PnP PS team

## Comparison

// TODO

## Conclusion

// TODO

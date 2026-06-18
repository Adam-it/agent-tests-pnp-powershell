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

### Claude Sonnet 4.6

The following tests were executed using Claude Sonnet 4.6 model

#### Baseline - Clean Copilot without any customizations

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  | ❌ 1/3             | ❌ 1/4               | 157,032      | 2,759         | 146,231       | 12.58   | 9          | 9           | 0      | |
| #2  | ✅ 3/3             | ❌ 1/4               | 405,907      | 4,872         | 386,435       | 26.20   | 19         | 21          | 0      | 👈 |
| #3  | ✅ 3/3             | ❌ 1/4               | 629,746      | 7,067         | 595,966       | 41.14   | 24         | 28          | 0      | |

Copilot usually assumed it may use the Client ID for making the connections using the deprecated PnP Management Shell app. It also usually used device login for making the connection and never asked the user about what should be the permissions scoped added to the app registration or if the app registration will be used for interactive login or for an application. Copilot never validated if PowerShell terminal is in the correct and supported version. Lastly, Copilot many times made a mistake to kill the terminal which means loosing the connection to the site before confirming if the connection was established.

#### Context7 - Copilot with Context7 MCP

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  | ✅ 3/3             | ❌ 2/4               | 430,208      | 6,987         | 408,152       | 30.99   | 21         | 21          | 0      | |
| #2  | ✅ 3/3             | ❌ 1/4               | 218,017      | 3,030         | 221,047       | 15.10   | 12         | 12          | 0      | 👈 |
| #3  | ✅ 3/3             | ❌ 1/4               | 764,237      | 8,210         | 731,047       | 46.69   | 29         | 32          | 0      | |

Copilot one time validate the PowerShell version installed before installing the PnP PowerShell module. Copilot did not use any of the Context7 MCP tools. Copilot again used the old PnP Management Shell app registration for making the connection and it also does not ask the user for the authentication type nor the permissions scopes that should be used.

#### PnP PowerShell MCP - Copilot with the PnP PowerShell MCP - ☠️

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ |
| #1  |                    |                       |              |               |               |         |            |             |        |
| #2  |                    |                       |              |               |               |         |            |             |        |
| #3  |                    |                       |              |               |               |         |            |             |        |

**⚠️ Unable to test.**

Starting the PnP PowerShell MCP server gives the following error: `[server stderr] Version 0.1.0-beta of package pnp.powershell.mcpserver.win-x64 is not found in NuGet feeds https://api.nuget.org/v3/index.json.`

#### PnP PowerShell Copilot Plugin - Copilot with the PnP PowerShell Copilot Plugin

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  | ✅ 3/3             | ✅ 4/4               | 229,454      | 3,056         | 203,065       | 20.57   | 9          | 11          | 0      | |
| #2  | ✅ 3/3             | ✅ 4/4               | 206,257      | 2,752         | 180,377       | 19.24   | 8          | 10          | 0      | 👈 |
| #3  | ✅ 3/3             | ✅ 4/4               | 348,448      | 4,035         | 328,026       | 23.55   | 11         | 16          | 0      | |

Copilot installed the latest version of PnP PowerShell and created a new app registration with the correct permissions scopes. Copilot also proactively asked about the authentication type to use for the connection and scopes that should be added. Copilot validated the connection and always used interactive login, did not fallback to certificate or device code login.

#### Comparison

Below is a comparison between best runs of different methods.

| Method         | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| -------------- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| baseline       | ✅ 3/3             | ❌ 1/4               | 405,907      | 4,872         | 386,435       | 26.20   | 19         | 21          | 0      | |
| context7       | ✅ 3/3             | ❌ 1/4               | 218,017      | 3,030         | 221,047       | 15.10   | 12         | 12          | 0      | |
| PnP PS MCP     |                    |                       |              |               |               |         |            |             |        | |
| PnP PS Plugin  | ✅ 3/3             | ✅ 4/4               | 206,257      | 2,752         | 180,377       | 19.24   | 8          | 10          | 0      | 👈 |

Below comparison highlights the differences between the best runs for different criteria.

| Criteria        | baseline       | context7       | PnP PS MCP     | PnP PS Plugin  |
| --------------- | -------------- | -------------- | -------------- | -------------- |
| Must have       | ✅ 3/3         | ✅ 3/3        |                | ✅ 3/3         |
| Nice to have    | ❌ 1/4         | ❌ 1/4        |                | ✅ 4/4 🔼      |
| Input tokens    | 405,907        | 218,017        |                | 206,257 🔼     |
| Output tokens   | 4,872          | 3,030          |                | 2,752 🔼       |
| Cached tokens   | 386,435        | 221,047        |                | 180,377 🔼     |
| Credits         | 26.20          | 15.10 🔼       |                | 19.24          |
| Tool calls      | 19             | 12             |                | 8 🔼           |
| Model turns     | 21             | 12             |                | 10 🔼          |
| Errors          | 0              | 0              |                | 0              |

### GPT 5.5

The following tests were executed using GPT 5.5 model

#### Baseline - Clean Copilot without any customizations

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  | ✅ 3/3             | ❌ 2/4               | 262,177      | 4,191         | 219,648       | 44.82   | 12         | 14          | 0      | |
| #2  | ✅ 3/3             | ❌ 2/4               | 265,165      | 2,857         | 220,672       | 41.85   | 13         | 13          | 0      | |
| #3  | ✅ 3/3             | ❌ 2/4               | 264,019      | 4,922         | 239,616       | 38.95   | 14         | 14          | 0      | 👈|

Copilot most of the times figured out it needed to register an app registration in order to perform the sign in on first go and did not use the legacy PnP Management Shell app. Copilot always validated PowerShell version installed and if the connection was established. Copilot never asked for the scopes that should be created for the app registration.

#### Context7 - Copilot with Context7 MCP

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  |                    |                       |              |               |               |         |            |             |        | |
| #2  |                    |                       |              |               |               |         |            |             |        | |
| #3  |                    |                       |              |               |               |         |            |             |        | |

//🤡 Skipped for now as Context7 MCP does not make sense if the prompt does not explicitly states that Context7 should be used.

#### PnP PowerShell MCP - Copilot with the PnP PowerShell MCP - ☠️

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ |
| #1  |                    |                       |              |               |               |         |            |             |        |
| #2  |                    |                       |              |               |               |         |            |             |        |
| #3  |                    |                       |              |               |               |         |            |             |        |

**⚠️ Unable to test.**

Starting the PnP PowerShell MCP server gives the following error: `[server stderr] Version 0.1.0-beta of package pnp.powershell.mcpserver.win-x64 is not found in NuGet feeds https://api.nuget.org/v3/index.json.`

#### PnP PowerShell Copilot Plugin - Copilot with the PnP PowerShell Copilot Plugin

| Run | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| --- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| #1  | ✅ 3/3             | ❌ 2/4               | 204,886      | 3,227         | 178,688       | 31.71   | 9          | 10          | 0      | 👈|
| #2  | ✅ 3/3             | ✅ 4/4               | 476,039      | 3,308         | 370,688       | 81.13   | 8          | 10          | 0      | 😱|
| #3  | ✅ 3/3             | ❌ 2/4               | 319,055      | 5,278         | 302,080       | 39.43   | 13         | 14          | 0      | |

Copilot does not follow the skill instructions fully. It does not explicitly ask the user for the scopes that should be added to the app registration. 
In one run Copilot fetched additional information from docs web page which significantly increased the number of input tokens and credits used. 
Copilot checked `[Environment]::GetEnvironmentVariable('ENTRAID_CLIENT_ID', 'User')` and found a previously registered app registration and and tried to use it, this corrupted the tests

#### Comparison

Below is a comparison between best runs of different methods.

| Method         | Must have criteria | Nice to have criteria | Input tokens | Output tokens | Cached tokens | Credits | Tool calls | Model turns | Errors | |
| -------------- | ------------------ | --------------------- | ------------ | ------------- | ------------- | ------- | ---------- | ----------- | ------ | --- |
| baseline       | ✅ 3/3             | ❌ 2/4               | 264,019      | 4,922         | 239,616       | 38.95   | 14         | 14          | 0      | |
| context7       |                    |                       |              |               |               |         |            |             |        | |
| PnP PS MCP     |                    |                       |              |               |               |         |            |             |        | |
| PnP PS Plugin  | ✅ 3/3             | ❌ 2/4               | 204,886      | 3,227         | 178,688       | 31.71   | 9          | 10          | 0      | 👈 |

Below comparison highlights the differences between the best runs for different criteria.

| Criteria        | baseline       | context7       | PnP PS MCP     | PnP PS Plugin  |
| --------------- | -------------- | -------------- | -------------- | -------------- |
| Must have       | ✅ 3/3         |               |                | ✅ 3/3         |
| Nice to have    | ❌ 2/4         |               |                | ❌ 2/4         |
| Input tokens    | 264,019        |                |                | 204,886 🔼     |
| Output tokens   | 4,922          |                |                | 3,227 🔼       |
| Cached tokens   | 239,616        |                |                | 178,688 🔼     |
| Credits         | 38.95          |                |                | 31.71 🔼       |
| Tool calls      | 14             |                |                | 9 🔼           |
| Model turns     | 14             |                |                | 10 🔼          |
| Errors          | 0              |                |                | 0              |

## Conclusion

// 🚫 Blocked 

Will rerun the tests and perform final conclusion after I perform fixup in PnP PS Copilot Plugin. For now I have the following observations that should be fixed up in the PnP PS Copilot Plugin:

- Skills should be reviewed with different LLM models. What works for Anthropic models, making the model follow the instructions, does not have to be the same for OpenAI models. In my case GPT model did not ask the user for scopes and login mode but assumed it should be minimal and interactive mode. It also fetched additional information from docs web page which should be avoided.
- Anthropic are the only models that charge different amount for cache writes. This is tricky as VS Code agent debug view does not show cache writes, only cache inputs.
- Skill should provide information how the output of the process/tool should look like to make it consistent for every run and it should be short and clean
- Currently for both Anthropic and OpenAI models the PnP PS Copilot Plugin is an obvious lift but it still could be tweaked to make it behave better for OpenAI models

---

[Back](../README.md)
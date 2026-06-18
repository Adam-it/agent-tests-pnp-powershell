# Agent tests for PnP PowerShell Copilot extensibility

This repository contains tests and comparison of different Copilot customizations that support using PnP PowerShell with Copilot. The tests are designed to evaluate the effectiveness of various approaches in enhancing the Copilot experience for PnP PowerShell usage.

## Tests

### Test 1 - PnP PowerShell setup

This test evaluates how well Copilot performs in setting up you local environment for using PnP PowerShell.

[Summary of Test 1 - PnP PowerShell setup](test-1-pnp-ps-setup/summary.md)

### Test 2 - Scaffolding a SharePoint Communications site with Content Type and List and a few items

// 🏗️ Work in progress

### Test 3 - Create a PowerShell script that uses PnP PowerShell to migrate files between libraries on different SharePoint sites

// 🏗️ Work in progress

## Harness

The tests used the following harness to run the tests and collect the results:

- Copilot in VS Code 

## Overall conclusion

// TODO

## Tips & Learnings

- as one of the first steps Copilot checks the current folder/workspace it is in and the files it has which may influence how Copilot behaves and responds to the prompt. In order to ensure consistency of the tests, it is recommended to run the tests in a clean folder with no files or folders present.
- Context7 is only used when specified in the prompt by name, otherwise Copilot ignores and does not use it
- From Cached tokens point of view it's best that the biggest prompt with most context be one of the first ones to be run, and all other prompts in the conversations should be follow up or Copilot question answers that best do not influence the tooling. 
- Changing model in the middle of the chat conversation resets the token cache
- Anthropic are the only models that charge different amount for cache writes. This is tricky as VS Code agent debug view does not show cache writes, only cache inputs.
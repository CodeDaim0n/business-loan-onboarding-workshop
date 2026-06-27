# Prerequisites

Complete this checklist before you start. You only need the accounts for the integrations in your track.

## Required for every track

| Item | Details |
|---|---|
| Camunda 8 account | Sign up at [console.camunda.io](https://console.camunda.io/). A SaaS cluster is used to deploy and run the process. |
| Modern web browser | Latest Chrome, Edge, Firefox, or Safari. Used for Console, Web Modeler, Operate, and Tasklist. |
| Workshop BPMN file | Provided with your workshop materials. Place it in [`../camunda/processes/`](../camunda/processes/). |
| Stable internet connection | Required for SaaS, model providers, and external APIs. |

## Required by integration

| Integration | Account / tool | Where to sign up |
|---|---|---|
| AI (OpenAI) | OpenAI Platform account with billing | [platform.openai.com](https://platform.openai.com/) |
| Company registry | Companies House account (UK) | [Companies House Developer Hub](https://developer.company-information.service.gov.uk/) |
| CRM | Salesforce org (Developer Edition is free) | [developer.salesforce.com/signup](https://developer.salesforce.com/signup) |
| Email | Gmail account with 2-Step Verification | [myaccount.google.com/security](https://myaccount.google.com/security) |

## Optional local AI track

| Item | Minimum |
|---|---|
| Operating system | Windows 10+ or macOS 13+ |
| Python | 3.10–3.14 ([python.org/downloads](https://www.python.org/downloads/)) |
| `uv` package manager | [Install guide](https://docs.astral.sh/uv/getting-started/installation/) |
| Disk space | A few GB if you pull local Ollama models |

Full instructions: [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop).

## Before your first deploy

- [ ] You can sign in to [Camunda Console](https://console.camunda.io/).
- [ ] You have (or can create) a SaaS cluster.
- [ ] The provided BPMN file is in `camunda/processes/`.
- [ ] You know which integrations your track requires.
- [ ] You have created the matching [Connector Secrets](../camunda/docs/saas-setup-guide.md#4-create-connector-secrets).

## Regional note

The company registry step uses Companies House (UK). If you are in another region, read the [regional notes](./regional-notes.md) first.

# Prerequisites

Complete this checklist before you start. You only need the accounts for the integrations in your track.

## Required for every track

| Item | Details |
|---|---|
| Your own Camunda 8 account | Sign up yourself at [console.camunda.io](https://console.camunda.io/). **No account is provided for you.** A free trial is enough for this workshop. |
| Your own SaaS cluster | Create it yourself in Camunda Console (the free trial cluster works). |
| Modern web browser | Latest Chrome, Edge, Firefox, or Safari. Used for Console, Web Modeler, Operate, and Tasklist. |
| Provided BPMN and forms | Supplied with your workshop materials. Import them and place files in [`../camunda/processes/`](../camunda/processes/). |
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

- [ ] You created your own [Camunda Console](https://console.camunda.io/) account and can sign in.
- [ ] You created your own SaaS cluster and it is ready.
- [ ] The provided BPMN and forms are imported and in `camunda/processes/`.
- [ ] You know which integrations your track requires.
- [ ] You have created the matching [Connector Secrets](../camunda/docs/saas-setup-guide.md#4-create-connector-secrets).

## Regional note

The company registry step uses Companies House (UK). If you are in another region, read the [regional notes](./regional-notes.md) first.

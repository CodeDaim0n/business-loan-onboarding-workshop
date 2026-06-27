# Prerequisites

Complete this checklist before you start. With the provided **mocks** variant you can run the whole example with only the first section below.

## Required for every track

| Item | Details |
|---|---|
| Your own Camunda 8 account | Sign up yourself at [console.camunda.io](https://console.camunda.io/). **No account is provided for you.** A free trial is enough for this workshop. |
| Your own SaaS cluster | Create it yourself in Camunda Console (the free trial cluster works). |
| An OpenAI API key | Required by the AI Agent in **both** process variants. Stored as the `OPENAI_API_KEY` secret. |
| Modern web browser | Latest Chrome, Edge, Firefox, or Safari. Used for Console, Web Modeler, Operate, and Tasklist. |
| Provided BPMN and forms | Included in [`../camunda/processes/`](../camunda/processes/). You create a Camunda project and upload them. |
| Stable internet connection | Required for SaaS, model providers, and external APIs. |

> **Run with no integrations.** Deploy `business-loan-onboarding-verification_demo_mocks.bpmn` to run the entire flow using only the items above — the Companies House, Salesforce, and email steps are simulated.

## Required by integration (full variant only)

> You only need the accounts below to run the **full** variant (`business-loan-onboarding-verification_demo.bpmn`) against real systems. The mocks variant needs none of them. You create each account yourself — none are provided.

| Integration | Account / tool | Where to sign up |
|---|---|---|
| Company registry | Your own Companies House account (UK) | [Companies House Developer Hub](https://developer.company-information.service.gov.uk/) |
| CRM | Your own Salesforce Developer Edition org (free). Only the **Lead** object is used. | [developer.salesforce.com/signup](https://developer.salesforce.com/signup) |
| Email | Your own Gmail account with 2-Step Verification (SMTP send + IMAP listener) | [myaccount.google.com/security](https://myaccount.google.com/security) |

> The AI provider (`OPENAI_API_KEY`) is listed under **Required for every track** above — it is needed by both variants, including the mocks one.

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
- [ ] You created a Camunda project and uploaded a BPMN variant plus the three forms from `camunda/processes/`.
- [ ] You chose a variant: **mocks** (no integrations) or **full** (live integrations).
- [ ] You created the `OPENAI_API_KEY` secret (required by both variants).
- [ ] *(Full variant only)* You created the matching integration [Connector Secrets](../camunda/docs/saas-setup-guide.md#4-create-connector-secrets).

## Regional note

The company registry step uses Companies House (UK). If you are in another region, read the [regional notes](./regional-notes.md) first.

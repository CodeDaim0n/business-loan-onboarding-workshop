# Business Loan Onboarding & Verification — Workshop

A hands-on, end-to-end workshop for building an **agentic business-loan onboarding process** on [Camunda 8](https://camunda.com/platform/). You will orchestrate parallel verification checks, an AI Agent operating within defined process boundaries, a CRM update, and a customer outcome email — all driven by a BPMN model.

> **Learning environment.** Use **synthetic test data only**. Do not enter real customer data, production credentials, or regulated information at any point in this workshop.

---

## About this workshop

This repository accompanies the code-along webinar **“Build a No Code AI Banking Agent.”**

| | |
|---|---|
| **Session** | [Build a No Code AI Banking Agent](https://www.datacamp.com/webinars/build-a-no-code-ai-banking-agent) (free to join) |
| **When** | Monday, June 29, 11 AM ET |
| **Presenter** | **[Anjali Jain](https://www.linkedin.com/in/anjali-jain-42942014/)** — Enterprise AI Architect at Metro Bank, CTO at Erdos Research, and Senior Tutor in AI & Machine Learning at the University of Oxford. Author of *AI-Assisted Programming for Web and Machine Learning* and the forthcoming *Enterprise Architecture in an Agentic World*. |

**What you will take away**

- How to build an AI agent using no-code tools for financial-services use cases.
- Where AI agents add value in banking and other regulated environments.
- How to architect agents that meet performance, compliance, and reliability requirements.

AI agents are transforming financial services, but building them in a regulated environment requires careful design, strong controls, and knowing where no-code tools can safely accelerate delivery. This workshop combines speed with responsibility: the BPMN model keeps the AI agent inside explicit, auditable boundaries.

---

## Do I need the integrations?

**No — not to learn.** You can complete the core learning with just a free Camunda account (and optionally Langflow):

- Design and run **agentic process flows in Camunda**.
- Build **AI agents in Langflow** (optional local track).

The external integrations — **Companies House**, **Salesforce**, and **email** — are only required to run the **provided end-to-end example** exactly as shipped. If you do not set them up, you can still follow every concept and build your own agentic flows; you simply will not be able to exercise those specific integration steps.

| Goal | What you need |
|---|---|
| Learn agentic flows and AI agents | A free Camunda account (Langflow optional) |
| Run the full provided example | Camunda **plus** the integrations for your track |

---

## Table of contents

- [About this workshop](#about-this-workshop)
- [Do I need the integrations?](#do-i-need-the-integrations)
- [What you will build](#what-you-will-build)
- [Learning objectives](#learning-objectives)
- [Repository structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Getting started](#getting-started)
- [Learning path](#learning-path)
- [Camunda Connector Secrets](#camunda-connector-secrets)
- [Security and safe data](#security-and-safe-data)
- [Troubleshooting and FAQ](#troubleshooting-and-faq)
- [Regional notes](#regional-notes)
- [Related repository](#related-repository)
- [Reference documentation](#reference-documentation)
- [License](#license)

---

## What you will build

```text
                Application received
                        │
                        ▼
            Create or reuse CRM Lead (Salesforce)
                        │
                        ▼
              Two checks run in parallel
        ┌───────────────┴───────────────┐
        ▼                               ▼
 Company registry check        AI document analysis
 (Companies House)             (Camunda AI Agent / Langflow)
        └───────────────┬───────────────┘
                        ▼
            Set a controlled verification route
        ┌───────────────┼───────────────────────────┐
        ▼               ▼                           ▼
   Hard fail       Passed / exception          AI Agent selects a
        │          → AI Agent next action       permitted next action
        │               │
        ▼               ▼
   Update CRM + send applicant outcome email + complete
```

The BPMN model controls the path at all times. The AI Agent can only recommend or trigger actions that are already defined in the process.

**Process name:** `Business Loan Onboarding & Verification`
**Process ID / BPMN file:** `business-loan-onboarding-verification.bpmn`

---

## Learning objectives

By the end of this workshop you will be able to:

1. Deploy and run a process on a Camunda 8 SaaS cluster.
2. Configure Connector Secrets and reference them safely from a process.
3. Run parallel service tasks and combine their results into a decision.
4. Use a Camunda AI Agent within explicit, auditable process boundaries.
5. Integrate a CRM (Salesforce) and an email channel.
6. Test happy-path, exception, and escalation scenarios, and inspect them in Operate and Tasklist.

---

## Repository structure

```text
business-loan-onboarding-workshop/
├── README.md                     # This guide
├── LICENSE
├── docs/
│   ├── prerequisites.md          # Accounts and tools you need before you start
│   ├── troubleshooting.md        # Common errors and how to resolve them
│   ├── glossary.md               # Key terms used in the workshop
│   └── regional-notes.md         # Adapting data sources by region (e.g. US)
├── camunda/
│   ├── README.md
│   ├── docs/saas-setup-guide.md  # Set up, deploy, and run on Camunda 8 SaaS
│   └── processes/                # Place the provided BPMN file(s) here
├── integrations/
│   ├── README.md
│   ├── companies-house/setup-guide.md
│   ├── salesforce/setup-guide.md
│   └── email-gmail/smtp-app-password-setup.md
└── workshop/
    ├── README.md
    └── business-loan-flow-guide.md   # Full walkthrough and test scenarios
```

---

## Prerequisites

A short summary is below. See **[docs/prerequisites.md](./docs/prerequisites.md)** for the full checklist, supported versions, and account sign-up links.

| Requirement | Why you need it |
|---|---|
| Your own [Camunda 8 account](https://console.camunda.io/) | You sign up yourself — no account is provided. A free trial covers this workshop |
| Your own Camunda 8 SaaS cluster | You create this yourself in Camunda Console |
| Modern web browser | Use Console, Web Modeler, Operate, and Tasklist |
| The provided BPMN and forms | Supplied with your workshop materials; imported and placed in `camunda/processes/` |
| Integration accounts | Only those required by your chosen track (OpenAI, Companies House, Salesforce, Gmail) |
| Windows or macOS | Only if you run the optional local Langflow track |

> **You create your own Camunda account and cluster.** A free trial (signup) cluster is sufficient.
>
> The Camunda process files (BPMN, forms, and any connector templates) are **provided with your workshop materials** and are not stored in this repository. Import them and add them to [`camunda/processes/`](./camunda/processes/) before deploying.

---

## Getting started

```bash
git clone https://github.com/CodeDaim0n/business-loan-onboarding-workshop.git
cd business-loan-onboarding-workshop
```

1. Confirm you meet the [prerequisites](./docs/prerequisites.md), including your own Camunda account.
2. Add the provided BPMN and forms to [`camunda/processes/`](./camunda/processes/).
3. Follow the [Camunda 8 SaaS setup guide](./camunda/docs/saas-setup-guide.md) to create your own cluster, add secrets, and deploy.
4. Complete the [integration guides](./integrations/README.md) required by your track.
5. Work through the [business loan flow guide](./workshop/business-loan-flow-guide.md) and run the test scenarios.
6. Optional: set up the [local AI track](https://github.com/CodeDaim0n/langflow-local-setup-workshop) with Langflow.

---

## Learning path

Follow the steps in order. Skip integrations that are not part of your track.

| Step | Topic | Guide |
|---|---|---|
| 1 | Platform setup | [Camunda 8 SaaS setup](./camunda/docs/saas-setup-guide.md) |
| 2 | Company registry check | [Companies House setup](./integrations/companies-house/setup-guide.md) |
| 3 | CRM | [Salesforce setup](./integrations/salesforce/setup-guide.md) |
| 4 | Email channel | [Gmail SMTP setup](./integrations/email-gmail/smtp-app-password-setup.md) |
| 5 | Process walkthrough | [Business loan flow guide](./workshop/business-loan-flow-guide.md) |
| 6 | Local AI (optional) | [Langflow local setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) |

---

## Camunda Connector Secrets

Create only the secrets required by your track. Reference them in connector fields with `{{secrets.SECRET_NAME}}`.

| Secret | Used for |
|---|---|
| `OPENAI_API_KEY` | AI document analysis and AI Agent tasks |
| `LANGFLOW_API_KEY` | Local Langflow REST calls (optional local AI track) |
| `COMPANY_HOUSE_KEY` | Companies House company and director checks |
| `SFDC_DEMO_BASE_URL`, `SFDC_DEMO_CONSUMER_KEY`, `SFDC_DEMO_CONSUMER_SECRET` | Salesforce OAuth |
| `INBOUND_MAIL_USER`, `INBOUND_MAIL_PASSWORD`, `INBOUND_MAIL_SERVER`, `INBOUND_MAIL_ADDRESS` | SMTP email |

Full details are in the [Camunda setup guide](./camunda/docs/saas-setup-guide.md#4-create-connector-secrets).

---

## Security and safe data

- Store every credential in **Camunda Connector Secrets** — never in BPMN XML, process variables, form fields, screenshots, or shared documents.
- Never commit `.env` files, API keys, passwords, or client secrets to Git.
- Use **synthetic test data only**. The credit-score step in this process is **simulated** and is not a real credit-bureau integration.
- Rotate any credential that is accidentally exposed.

---

## Troubleshooting and FAQ

Most setup and runtime issues are covered in **[docs/troubleshooting.md](./docs/troubleshooting.md)**, including connector errors (401/403/404/422), secret resolution, deployment problems, and human-task visibility.

---

## Regional notes

This workshop uses a UK company registry (Companies House) for the verification step. If you are running it in another region (for example, the US), see **[docs/regional-notes.md](./docs/regional-notes.md)** for what to use as-is and how to swap or mock the registry check.

---

## Related repository

The optional local AI track — Langflow on Windows or macOS, Ollama/Qwen models, Cloudflare Tunnel, and the Camunda REST integration — lives in a separate repository:

**[langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop)**

---

## Reference documentation

- [Camunda documentation](https://docs.camunda.io/)
- [Camunda Console](https://console.camunda.io/)
- [Camunda REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)
- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Salesforce External Client Apps](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm&type=5)
- [Langflow documentation](https://docs.langflow.org/) (optional track)

---

## License

See [LICENSE](./LICENSE). These materials are provided for educational use as part of the workshop.

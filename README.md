# Business Loan Onboarding & Verification — Workshop

A hands-on, end-to-end workshop for building an **agentic business-loan onboarding process** on [Camunda 8](https://camunda.com/platform/). You will orchestrate parallel verification checks, an AI Agent operating within defined process boundaries, a CRM update, and a customer outcome email — all driven by a BPMN model.

> **This workshop spans two repositories — each works on its own:**
>
> | Repository | What it teaches |
> |---|---|
> | **Camunda workflow — you are here** | Orchestrate an agentic business process on Camunda 8. |
> | **[Langflow — no-code AI agent builder](https://github.com/CodeDaim0n/langflow-local-setup-workshop)** | Build no-code AI agents, independent of any workflow engine. |

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

**No.** A **mocked process variant is provided** so you can run the entire example end-to-end with **no external integrations** — no Companies House, Salesforce, or email accounts. The Companies House, CRM, and email steps are replaced by built-in simulations, so you only need a free Camunda account and an `OPENAI_API_KEY` for the AI Agent.

Two skills in this workshop also stand entirely on their own:

- **Build no-code AI agents in [Langflow](https://github.com/CodeDaim0n/langflow-local-setup-workshop)** — a no-code agent builder that works independently of any workflow engine.
- **Design and run agentic process flows in Camunda** — workflow orchestration with an AI Agent inside auditable boundaries.

The external integrations — **Companies House**, **Salesforce**, and **email** — are only needed if you want to run the **full** variant against real systems instead of the mocks.

| Goal | What you need |
|---|---|
| Learn a no-code AI agent builder | [Langflow](https://github.com/CodeDaim0n/langflow-local-setup-workshop) on its own — no workflow engine required |
| Run the full example with **no integrations** | A free Camunda account **+ `OPENAI_API_KEY`** — use the **mocks** BPMN |
| Run the example against **real** systems | Camunda **plus** Companies House, Salesforce, and email — use the **full** BPMN |

> **Two BPMN variants are provided** in [`camunda/processes/`](./camunda/processes/):
> `business-loan-onboarding-verification_demo_mocks.bpmn` (mocked, recommended starting point) and
> `business-loan-onboarding-verification_demo.bpmn` (full, live integrations).

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
- [Companion track — Langflow](#companion-track--langflow-no-code-agent-builder)
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
**Provided BPMN files:** `business-loan-onboarding-verification_demo_mocks.bpmn` (mocked — no integrations) and `business-loan-onboarding-verification_demo.bpmn` (full — live integrations)

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
│   └── processes/                # Provided BPMN variants (mocks + full) and forms
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
| The provided BPMN and forms | Included in [`camunda/processes/`](./camunda/processes/) — you upload them into your own Camunda project |
| An OpenAI API key | Required by the AI Agent in **both** variants |
| Integration accounts | **Only for the full variant** (Companies House, Salesforce, Gmail) — not needed for the mocks variant |
| Windows or macOS | Only if you run the optional local Langflow track |

> **You create your own Camunda account and cluster.** A free trial (signup) cluster is sufficient.
>
> The Camunda process files (two BPMN variants and three forms) are **included in this repository** under [`camunda/processes/`](./camunda/processes/). You create your own Camunda project and upload them before deploying.

---

## Getting started

```bash
git clone https://github.com/CodeDaim0n/business-loan-onboarding-workshop.git
cd business-loan-onboarding-workshop
```

1. Confirm you meet the [prerequisites](./docs/prerequisites.md), including your own Camunda account.
2. In Camunda Web Modeler, **create a project** and **upload** a BPMN variant from [`camunda/processes/`](./camunda/processes/) together with the three `.form` files. Start with `business-loan-onboarding-verification_demo_mocks.bpmn` to run with no integrations.
3. Follow the [Camunda 8 SaaS setup guide](./camunda/docs/saas-setup-guide.md) to create your own cluster, add the `OPENAI_API_KEY` secret, and deploy.
4. Work through the [business loan flow guide](./workshop/business-loan-flow-guide.md) and run the test scenarios — the mocks variant runs end-to-end on its own.
5. *(Optional)* To use real systems, complete the [integration guides](./integrations/README.md) and deploy the full variant instead.
6. Build no-code AI agents in the standalone [Langflow track](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — usable on its own or connected to this process.

---

## Learning path

Follow the steps in order. With the **mocks** variant you can go straight from step 1 to step 5 — the integration steps (2–4) are only needed for the full variant.

| Step | Topic | Guide |
|---|---|---|
| 1 | Platform setup, upload, and deploy | [Camunda 8 SaaS setup](./camunda/docs/saas-setup-guide.md) |
| 2 | *(Full variant only)* Company registry check | [Companies House setup](./integrations/companies-house/setup-guide.md) |
| 3 | *(Full variant only)* CRM | [Salesforce setup](./integrations/salesforce/setup-guide.md) |
| 4 | *(Full variant only)* Email channel | [Gmail SMTP setup](./integrations/email-gmail/smtp-app-password-setup.md) |
| 5 | Process walkthrough | [Business loan flow guide](./workshop/business-loan-flow-guide.md) |
| — | No-code AI agent builder (standalone) | [Langflow track](https://github.com/CodeDaim0n/langflow-local-setup-workshop) |

---

## Camunda Connector Secrets

Reference secrets in connector fields with `{{secrets.SECRET_NAME}}`.

**Mocks variant** — create just one secret:

| Secret | Used for |
|---|---|
| `OPENAI_API_KEY` | AI document analysis and AI Agent tasks |

**Full variant** — also create the integration secrets:

| Secret | Used for |
|---|---|
| `COMPANY_HOUSE_KEY` | Companies House company and director checks |
| `SFDC_DEMO_BASE_URL`, `SFDC_DEMO_CONSUMER_KEY`, `SFDC_DEMO_CONSUMER_SECRET` | Salesforce OAuth |
| `INBOUND_MAIL_USER`, `INBOUND_MAIL_PASSWORD`, `INBOUND_MAIL_SERVER`, `INBOUND_MAIL_ADDRESS` | Outgoing (SMTP) email |
| `INBOUND_MAIL_IMAP_SERVER`, `INBOUND_MAIL_IMAP_PORT`, `INBOUND_MAIL_FOLDER` | Incoming (IMAP) applicant-response listener |

> The provided process calls **OpenAI directly**; it does not use `LANGFLOW_API_KEY`. Add a Langflow secret only if you adapt the process to call a Langflow flow.

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

## Companion track — Langflow (no-code agent builder)

Want to learn a **no-code AI agent builder that is independent of any workflow engine**? Use the Langflow track. It stands on its own — build, run, and iterate on agents with just a model provider — and can optionally connect to this Camunda process through Cloudflare Tunnel and the REST connector.

**[langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop)**

---

## Reference documentation

- [Camunda documentation](https://docs.camunda.io/)
- [Camunda Console](https://console.camunda.io/)
- [Camunda REST connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)
- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Salesforce External Client Apps](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm&type=5)
- [Langflow documentation](https://docs.langflow.org/)

---

## Star and contribute

If you like this repo, give it a star :) — it helps others find it.

If you find issues, please help others by providing corrections :) — open an issue or a pull request.

---

## License

See [LICENSE](./LICENSE). These materials are provided for educational use as part of the workshop.

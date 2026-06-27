# Business Loan Onboarding & Verification — Workshop

Hands-on materials for building and running an agentic business-loan onboarding process with **Camunda 8**, optional **Langflow** AI flows, and integrations to **Companies House**, **Salesforce**, and **email**.

> **Synthetic data only.** This is a learning demonstration. Do not use real customer data, production credentials, or regulated information.

## What you will build

```text
Application received
        |
        v
Create or reuse CRM Lead (Salesforce)
        |
        v
Parallel verification
  ├─ Companies House company check
  └─ AI document analysis (Langflow / OpenAI)
        |
        v
Controlled verification route + AI Agent next actions
        |
        v
Update CRM + send applicant outcome email
```

**Process name:** `Business Loan Onboarding & Verification`  
**Process ID / BPMN file:** `business-loan-onboarding-verification.bpmn`

## Repository layout

```text
business-loan-onboarding-workshop/
├── README.md                 ← you are here
├── docs/                     ← step-by-step setup guides
│   ├── 01-camunda-saas-setup.md
│   ├── 02-langflow-local-setup.md
│   ├── 03-companies-house-api-setup.md
│   ├── 04-salesforce-camunda-setup.md
│   ├── 05-gmail-smtp-setup.md
│   └── 06-business-loan-flow-guide.md
└── camunda/                  ← place instructor-supplied BPMN here
    └── README.md
```

## Camunda resources (provided by instructor)

This repo contains **documentation only**. Your instructor will supply:

- `business-loan-onboarding-verification.bpmn`
- Any additional forms, DMN tables, or connector assets required for the workshop

Copy those files into [`camunda/`](./camunda/) before importing into Camunda Modeler.

## Quick start

1. **Clone this repository**

   ```bash
   git clone <your-repo-url>
   cd business-loan-onboarding-workshop
   ```

2. **Add Camunda assets** from your instructor to `camunda/`.

3. **Follow the guides** in [`docs/`](./docs/README.md), starting with [Camunda 8 SaaS setup](./docs/01-camunda-saas-setup.md).

4. **Create Connector Secrets** in Camunda Console (never in BPMN or Git). Typical secrets:

   | Secret | Used for |
   |---|---|
   | `OPENAI_API_KEY` | AI document analysis / agent tasks |
   | `LANGFLOW_API_KEY` | Local Langflow REST calls |
   | `COMPANY_HOUSE_KEY` | Companies House lookups |
   | `SFDC_DEMO_*` | Salesforce OAuth |
   | `INBOUND_MAIL_*` | SMTP email (e.g. Gmail App Password) |

5. **Deploy and test** using scenarios in the [flow guide](./docs/06-business-loan-flow-guide.md).

## Recommended workshop order

| Phase | Guide |
|---|---|
| Platform setup | [01 — Camunda SaaS](./docs/01-camunda-saas-setup.md) |
| Local AI (optional) | [02 — Langflow + Cloudflare Tunnel](./docs/02-langflow-local-setup.md) |
| Integrations | [03](./docs/03-companies-house-api-setup.md) · [04](./docs/04-salesforce-camunda-setup.md) · [05](./docs/05-gmail-smtp-setup.md) |
| Process deep-dive | [06 — Business Loan flow](./docs/06-business-loan-flow-guide.md) |

## Prerequisites

| Item | Notes |
|---|---|
| [Camunda account](https://console.camunda.io/) | SaaS cluster for deploy/run |
| Modern browser | Console, Web Modeler, Operate, Tasklist |
| Windows or macOS | For optional local Langflow |
| Integration accounts | As required by your workshop track (OpenAI, Companies House, Salesforce, Gmail) |

## Security rules

- Store all credentials in **Camunda Connector Secrets** using `{{secrets.SECRET_NAME}}`.
- Never commit `.env` files, API keys, passwords, or client secrets.
- Never embed secrets in BPMN XML, process variables, screenshots, or slide decks.
- Use personal Gmail and demo Salesforce orgs only for learning — not production.

## Official references

- [Camunda documentation](https://docs.camunda.io/)
- [Langflow documentation](https://docs.langflow.org/)
- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/)

## License

Workshop materials for educational use. Check with your instructor before redistributing.

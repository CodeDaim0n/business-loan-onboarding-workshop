# Business Loan Onboarding & Verification — Workshop

Hands-on materials for an agentic business-loan onboarding process with **Camunda 8**, integrations to **Companies House**, **Salesforce**, and **email**, plus optional **Langflow** AI (separate repo).

> **Synthetic data only.** Learning demonstration — no real customer data or production credentials.

## Repository layout

```text
business-loan-onboarding-workshop/
├── README.md
├── camunda/
│   ├── docs/saas-setup-guide.md
│   └── processes/              ← instructor-supplied BPMN
├── integrations/
│   ├── companies-house/
│   ├── salesforce/
│   └── email-gmail/
└── workshop/
    └── business-loan-flow-guide.md
```

## Related repository — Langflow

Local Langflow, Ollama, Cloudflare Tunnel, and Camunda REST integration are documented separately:

**[langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop)**

Clone that repo when your workshop track includes local AI flows on Windows or macOS.

## Quick start

```bash
git clone https://github.com/CodeDaim0n/business-loan-onboarding-workshop.git
cd business-loan-onboarding-workshop
```

1. Add instructor BPMN files to [`camunda/processes/`](./camunda/processes/).
2. Follow [Camunda SaaS setup](./camunda/docs/saas-setup-guide.md).
3. Complete integration guides in [`integrations/`](./integrations/README.md) as needed.
4. Read the [business loan flow guide](./workshop/business-loan-flow-guide.md).
5. Optional: clone [Langflow local setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop).

## Recommended order

| Step | Location |
|---|---|
| Camunda platform | [camunda/docs/saas-setup-guide.md](./camunda/docs/saas-setup-guide.md) |
| Companies House | [integrations/companies-house/](./integrations/companies-house/setup-guide.md) |
| Salesforce | [integrations/salesforce/](./integrations/salesforce/setup-guide.md) |
| Email (Gmail SMTP) | [integrations/email-gmail/](./integrations/email-gmail/smtp-app-password-setup.md) |
| Process deep-dive | [workshop/business-loan-flow-guide.md](./workshop/business-loan-flow-guide.md) |
| Local AI (optional) | [Langflow repo](https://github.com/CodeDaim0n/langflow-local-setup-workshop) |

## Typical Camunda secrets

| Secret | Used for |
|---|---|
| `OPENAI_API_KEY` | AI document analysis / agent tasks |
| `LANGFLOW_API_KEY` | Local Langflow REST calls (if using Langflow repo) |
| `COMPANY_HOUSE_KEY` | Companies House lookups |
| `SFDC_DEMO_*` | Salesforce OAuth |
| `INBOUND_MAIL_*` | SMTP email |

## Security

- Credentials in **Camunda Connector Secrets** only (`{{secrets.NAME}}`).
- Never commit `.env`, keys, or secrets to Git.

## Official references

- [Camunda documentation](https://docs.camunda.io/)
- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Langflow documentation](https://docs.langflow.org/) (optional track)

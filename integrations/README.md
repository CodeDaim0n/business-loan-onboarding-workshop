# Integrations

Setup guides for the external services used by the **full** variant of the **Business Loan Onboarding & Verification** process. Complete only the integrations you actually want to run live.

> **Optional — a mocked process variant is provided.** If you deploy `business-loan-onboarding-verification_demo_mocks.bpmn`, the Companies House, Salesforce, and email steps are simulated, so you can run the entire example with **none** of the integrations below — only an `OPENAI_API_KEY`. Complete these guides only if you run the **full** variant (`business-loan-onboarding-verification_demo.bpmn`) against real systems. You create each account yourself — none are provided.

| Integration | Guide | Camunda secrets |
|---|---|---|
| Company registry (Companies House) | [setup-guide.md](./companies-house/setup-guide.md) | `COMPANY_HOUSE_KEY` |
| CRM (Salesforce — Lead object only) | [setup-guide.md](./salesforce/setup-guide.md) | `SFDC_DEMO_BASE_URL`, `SFDC_DEMO_CONSUMER_KEY`, `SFDC_DEMO_CONSUMER_SECRET` |
| Email (Gmail SMTP + IMAP) | [smtp-app-password-setup.md](./email-gmail/smtp-app-password-setup.md) | `INBOUND_MAIL_USER`, `INBOUND_MAIL_PASSWORD`, `INBOUND_MAIL_SERVER`, `INBOUND_MAIL_ADDRESS`, `INBOUND_MAIL_IMAP_SERVER`, `INBOUND_MAIL_IMAP_PORT`, `INBOUND_MAIL_FOLDER` |

Store all credentials in **Camunda Connector Secrets** — never in BPMN XML or Git.

## Regional notes

Companies House is a UK registry. To run the verification step in another region, see the [regional notes](../docs/regional-notes.md).

## Companion track — Langflow

- [Langflow local setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — a no-code AI agent builder that works independently of any workflow engine

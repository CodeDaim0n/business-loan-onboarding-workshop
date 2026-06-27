# Integrations

Setup guides for the external services used by the **Business Loan Onboarding & Verification** process. Complete only the integrations required by your track.

> **Optional for learning.** These integrations are only needed to run the provided end-to-end example. You can still learn and build agentic process flows in Camunda (and agents in Langflow) without them. You create each account yourself — none are provided.

| Integration | Guide | Camunda secrets |
|---|---|---|
| Company registry (Companies House) | [setup-guide.md](./companies-house/setup-guide.md) | `COMPANY_HOUSE_KEY` |
| CRM (Salesforce — Lead object only) | [setup-guide.md](./salesforce/setup-guide.md) | `SFDC_DEMO_BASE_URL`, `SFDC_DEMO_CONSUMER_KEY`, `SFDC_DEMO_CONSUMER_SECRET` |
| Email (Gmail SMTP) | [smtp-app-password-setup.md](./email-gmail/smtp-app-password-setup.md) | `INBOUND_MAIL_USER`, `INBOUND_MAIL_PASSWORD`, `INBOUND_MAIL_SERVER`, `INBOUND_MAIL_ADDRESS` |

Store all credentials in **Camunda Connector Secrets** — never in BPMN XML or Git.

## Regional notes

Companies House is a UK registry. To run the verification step in another region, see the [regional notes](../docs/regional-notes.md).

## Related repository

- [Langflow local setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — optional local AI flows, Ollama, and Cloudflare Tunnel

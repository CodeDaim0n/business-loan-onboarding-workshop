# Integrations

Setup guides for external services used by the **Business Loan Onboarding & Verification** Camunda process.

| Integration | Guide | Camunda secret examples |
|---|---|---|
| Companies House | [setup-guide.md](./companies-house/setup-guide.md) | `COMPANY_HOUSE_KEY` |
| Salesforce | [setup-guide.md](./salesforce/setup-guide.md) | `SFDC_DEMO_BASE_URL`, `SFDC_DEMO_CONSUMER_KEY`, `SFDC_DEMO_CONSUMER_SECRET` |
| Gmail SMTP | [smtp-app-password-setup.md](./email-gmail/smtp-app-password-setup.md) | `INBOUND_MAIL_*` |

Store all credentials in **Camunda Connector Secrets** — never in BPMN or Git.

## Related repositories

- [Business Loan Onboarding workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop) — Camunda process and flow guide
- [Langflow local setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — local AI flows and Cloudflare Tunnel

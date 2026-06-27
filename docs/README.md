# Workshop documentation

Follow these guides in order for the full hands-on path.

## Recommended sequence

| Step | Guide | When you need it |
|---|---|---|
| 1 | [Camunda 8 SaaS setup](./01-camunda-saas-setup.md) | Always — cluster, secrets, deploy |
| 2 | [Langflow local setup](./02-langflow-local-setup.md) | When using local AI flows via Cloudflare Tunnel |
| 3 | [Companies House API setup](./03-companies-house-api-setup.md) | Company and director verification |
| 4 | [Salesforce + Camunda setup](./04-salesforce-camunda-setup.md) | CRM Lead create/update steps |
| 5 | [Gmail SMTP App Password setup](./05-gmail-smtp-setup.md) | Outcome email from a personal Gmail demo account |
| 6 | [Business Loan flow guide](./06-business-loan-flow-guide.md) | End-to-end process behaviour and test scenarios |

## Process assets

BPMN and related Camunda files are **not** in this repository. Your instructor will provide them — place them in [`../camunda/`](../camunda/).

## Safety reminder

Use **synthetic test data only**. Never put credentials in BPMN, Git, screenshots, or shared documents.

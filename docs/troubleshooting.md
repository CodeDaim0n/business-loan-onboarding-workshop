# Troubleshooting

Common issues and how to resolve them. Work from the local component outward: confirm the process is deployed, then secrets, then connectors, then the external service.

## Deployment

**The process will not deploy.**

- Confirm you are connected to the intended cluster and have permission to deploy.
- In Desktop Modeler, check the environment selector is connected to Camunda 8 and the correct cluster.
- Confirm the BPMN file in `camunda/processes/` opens without validation errors.

## Connector secrets

**A secret is not resolved (empty value or error).**

- The secret must exist in the **same cluster** you deployed to.
- The secret **name** must exactly match the reference in the process.
- The reference must use the form `{{secrets.SECRET_NAME}}`.
- In a FEEL expression, the placeholder must be in double quotes, for example `"{{secrets.OPENAI_API_KEY}}"`.
- Check the stored value has no leading/trailing spaces or stray quotes.

## REST / HTTP connector errors

| Status | Likely cause | What to check |
|---|---|---|
| 401 / 403 | Wrong or missing credential | Use the correct API key for that service; confirm the header name (for example `x-api-key`); check the secret reference matches |
| 404 | Wrong URL or resource ID | Re-copy the endpoint and any ID (such as a Langflow flow ID) from the source application |
| 400 / 422 | Request body does not match the expected input | Copy the exact request shape from the service's API example |
| Timeout | Service unreachable or slow | Confirm the target is running; for local Langflow, confirm Cloudflare Tunnel is up and the URL is current |

## AI document analysis

- Confirm `OPENAI_API_KEY` (or your local model) is configured and has available credit.
- If using a local model for an agent, confirm the model supports tool calling.

## Companies House

- Use a valid, known company number for tests.
- A public `GET` lookup needs an API key only — not OAuth.
- See the [Companies House setup guide](../integrations/companies-house/setup-guide.md).

## Salesforce

- Confirm the OAuth client credentials and base URL are stored as secrets.
- Confirm the integration user has access to the objects the process uses (Lead, Account, Contact).
- See the [Salesforce setup guide](../integrations/salesforce/setup-guide.md).

## Email

- Use a Google **App Password**, not your normal account password.
- Confirm 2-Step Verification is enabled on the Gmail account.
- See the [Gmail SMTP setup guide](../integrations/email-gmail/smtp-app-password-setup.md).

## Human tasks (Tasklist)

- Confirm you are using the Tasklist for the correct cluster.
- Confirm the task is assigned to you or is available to your group.

## Inspecting a running instance

Open **Operate** to see the active step, process variables, connector input/output, and any incidents. Incidents include the error message that usually identifies the root cause.

## Still stuck?

1. Isolate the failing component (deploy → secret → connector → external service).
2. Reproduce it with the minimal test payload from the [flow guide](../workshop/business-loan-flow-guide.md).
3. Re-check the relevant setup guide before changing the BPMN.

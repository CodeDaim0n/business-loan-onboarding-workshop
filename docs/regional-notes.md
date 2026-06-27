# Regional notes

This workshop uses a **UK company registry** ([Companies House](https://developer.company-information.service.gov.uk/)) for the verification step. Everything else is region-neutral. This page explains what works anywhere and how to adapt the registry check for other regions, such as the **United States**.

> Use **synthetic test data only**. Do not use real national identifiers, tax IDs, credit-bureau data, or live customer information.

---

## Works in any region

| Material | Location | Notes |
|---|---|---|
| Camunda 8 SaaS setup | [camunda/docs/saas-setup-guide.md](../camunda/docs/saas-setup-guide.md) | Console, deploy, secrets, Operate, Tasklist |
| Business loan flow guide | [workshop/business-loan-flow-guide.md](../workshop/business-loan-flow-guide.md) | The process patterns transfer; read UK registry references as a generic "business registry lookup" |
| Salesforce integration | [integrations/salesforce/setup-guide.md](../integrations/salesforce/setup-guide.md) | Developer Edition or sandbox works globally |
| Gmail SMTP (demo email) | [integrations/email-gmail/smtp-app-password-setup.md](../integrations/email-gmail/smtp-app-password-setup.md) | Personal Gmail + App Password is suitable for learning |
| Langflow local setup | [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop) | OpenAI, Ollama, Cloudflare Tunnel — region-neutral |
| Simulated credit score | Flow guide | A built-in stub; it is **not** a real credit bureau in any region |

---

## Region-specific — the company registry

| Material | Location | Consideration |
|---|---|---|
| Companies House API | [integrations/companies-house/setup-guide.md](../integrations/companies-house/setup-guide.md) | UK companies only (company number, officers, filings) |
| The provided BPMN | `camunda/processes/` | If it calls Companies House endpoints, it expects UK company numbers |

### Options when running outside the UK

1. **Skip the registry connector** — Run the rest of the process with mock company variables and focus on orchestration, AI, CRM, and email.
2. **Use a mock REST service** — Point the REST connector at a small fake "company lookup" API for consistent, repeatable results.
3. **Use a regional data source** — Replace Companies House with an appropriate API, for example in the US:
   - [OpenCorporates](https://api.opencorporates.com/) — multi-jurisdiction, includes US entities
   - [SEC EDGAR](https://www.sec.gov/edgar/sec-api-documentation) — US **public** companies only
   - State **Secretary of State** business-search APIs — vary by state; there is no single national equivalent of Companies House

Choose **one** registry approach for your run rather than mixing several.

---

## Suggested tracks

### Track A — Full process (recommended)

Camunda + Salesforce + Gmail + OpenAI (Camunda AI Agent or Langflow) + flow guide. Skip or mock the registry step where Companies House is not available.

### Track B — Orchestration + local AI

[Langflow repository](https://github.com/CodeDaim0n/langflow-local-setup-workshop) + Camunda REST Connector + Cloudflare Tunnel. Suitable when CRM and email are out of scope.

### Track C — Process + CRM

Camunda + Salesforce + flow guide. Skip Langflow and use the Camunda AI Agent with `OPENAI_API_KEY`.

---

## Synthetic test data conventions (US example)

| Field | Example (synthetic) |
|---|---|
| Business name | `Demo Lending LLC` |
| Registration / ID | `12-3456789` (format only — not a real EIN) |
| Applicant email | `demo.applicant@example.com` |
| Company proof | A sample PDF or image — no real financials |

---

## Secrets by track

Create only what your track needs.

| Secret | Track A | Track B | Track C |
|---|---|---|---|
| `OPENAI_API_KEY` | ✓ | ✓ (or Langflow only) | ✓ |
| `LANGFLOW_API_KEY` | If using Langflow | ✓ | — |
| `COMPANY_HOUSE_KEY` | UK only | — | UK only |
| `SFDC_DEMO_*` | ✓ | — | ✓ |
| `INBOUND_MAIL_*` | ✓ | — | — |

Add a regional registry secret only if you wire a replacement API, for example `OPENCORPORATES_API_KEY`.

---

## Adaptation checklist

- [ ] Decide whether to skip, mock, or replace the registry check.
- [ ] Prepare region-appropriate synthetic test payloads.
- [ ] Confirm the Salesforce org and Gmail demo account you will use.
- [ ] Remember the credit-score step is **simulated**.
- [ ] Place the correct BPMN variant in `camunda/processes/`.

---

## Related repositories

- [business-loan-onboarding-workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop) — this repository
- [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — optional local AI track

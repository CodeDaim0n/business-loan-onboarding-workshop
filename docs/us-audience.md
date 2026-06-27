# US audience — what to use and what to swap

This workshop was originally shaped around a **UK company registry** (Companies House). Most materials work for a **US audience** with one integration swap or skip.

> Use **synthetic test data only**. Do not use real SSNs, tax IDs, credit bureau data, or live customer information.

---

## Use as-is

| Material | Location | US notes |
|---|---|---|
| Camunda 8 SaaS setup | [camunda/docs/saas-setup-guide.md](../camunda/docs/saas-setup-guide.md) | Console, deploy, secrets, Operate, Tasklist |
| Business loan flow guide | [workshop/business-loan-flow-guide.md](../workshop/business-loan-flow-guide.md) | Process patterns transfer; replace UK registry references mentally with “business registry lookup” |
| Salesforce integration | [integrations/salesforce/setup-guide.md](../integrations/salesforce/setup-guide.md) | Developer Edition / sandbox works well in the US |
| Gmail SMTP (demo email) | [integrations/email-gmail/smtp-app-password-setup.md](../integrations/email-gmail/smtp-app-password-setup.md) | Personal Gmail + App Password is fine for learning |
| Langflow local setup | [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop) | OpenAI, Ollama, Cloudflare Tunnel — region-neutral |
| Simulated credit score | Flow guide | Already a **demo stub**, not a real US bureau (Experian, Equifax, TransUnion) |

---

## UK-only — skip or replace

| Material | Location | Issue |
|---|---|---|
| Companies House API | [integrations/companies-house/setup-guide.md](../integrations/companies-house/setup-guide.md) | UK companies only (company number, officers, filings) |
| Supplied BPMN (if UK-wired) | `camunda/processes/` | May call Companies House endpoints and expect UK company numbers |

### Instructor options for US delivery

1. **Skip registry connector** — Run the rest of the process with mock company variables; focus on orchestration, AI, CRM, and email.
2. **Mock REST service** — Host a simple fake “company lookup” API; point the REST connector at it (easiest for consistent classroom results).
3. **US data source** — Replace Companies House with a US-appropriate API, for example:
   - [OpenCorporates](https://api.opencorporates.com/) — multi-jurisdiction, includes US entities
   - [SEC EDGAR](https://www.sec.gov/edgar/sec-api-documentation) — US **public** companies only
   - State **Secretary of State** business search APIs — vary by state; no single national API like Companies House

There is no direct US equivalent of Companies House. Plan for **one registry story** per workshop, not all sources above.

---

## Suggested US workshop tracks

### Track A — Full demo (recommended)

Camunda + Salesforce + Gmail + OpenAI (Camunda AI Agent or Langflow) + flow guide.

- Omit or mock the company registry step.
- Use fictional US business names and test emails (`@example.com`).

### Track B — Orchestration + local AI

[Langflow repo](https://github.com/CodeDaim0n/langflow-local-setup-workshop) + Camunda REST Connector + Cloudflare Tunnel.

- Good when CRM/email are out of scope.

### Track C — Process + CRM

Camunda + Salesforce + flow guide.

- Skip Langflow; use Camunda AI Agent with `OPENAI_API_KEY`.

---

## US test data conventions

| Field | Example (synthetic) |
|---|---|
| Business name | `Demo Lending LLC` |
| Registration / ID | `12-3456789` (format only — not a real EIN) |
| Applicant email | `demo.applicant@example.com` |
| Company proof | Sample PDF or image you provide — no real financials |

---

## Camunda secrets (US track)

Create only what your track needs:

| Secret | Track A | Track B | Track C |
|---|---|---|---|
| `OPENAI_API_KEY` | ✓ | ✓ (or Langflow only) | ✓ |
| `LANGFLOW_API_KEY` | If using Langflow | ✓ | — |
| `COMPANY_HOUSE_KEY` | — (UK only) | — | — |
| `SFDC_DEMO_*` | ✓ | — | ✓ |
| `INBOUND_MAIL_*` | ✓ | — | — |

Add a US registry secret only if you wire a replacement API, e.g. `OPENCORPORATES_API_KEY`.

---

## Related repositories

- [business-loan-onboarding-workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop) — this repo
- [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — optional local AI track

---

## Instructor checklist (US)

- [ ] Decide: skip, mock, or replace Companies House in BPMN.
- [ ] Provide US-friendly synthetic test payloads.
- [ ] Confirm Salesforce org and Gmail demo accounts for participants.
- [ ] State clearly that credit scoring in the process is **simulated**.
- [ ] Place BPMN (US variant if applicable) in `camunda/processes/`.

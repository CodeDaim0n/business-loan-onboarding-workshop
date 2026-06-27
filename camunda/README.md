# Camunda assets and setup

This folder contains the Camunda 8 setup guide and the process files for the workshop.

## Folder layout

```text
camunda/
├── docs/
│   └── saas-setup-guide.md            # Set up, import, deploy, and run on Camunda 8 SaaS
└── processes/
    ├── business-loan-onboarding-verification_demo_mocks.bpmn   # Mocked — run with NO integrations
    ├── business-loan-onboarding-verification_demo.bpmn         # Full — uses the live integrations
    ├── Business Loan Application.form                          # Start form
    ├── Email Review.form                                       # "Review Customer Response" task form
    └── Loan Specialist decision.form                           # "Loan Specialist decision" task form
```

## Two process variants — pick one

| File | Use it when | What it needs |
|---|---|---|
| `business-loan-onboarding-verification_demo_mocks.bpmn` | **Recommended starting point.** Run the entire example end-to-end with **no external integrations**. | Only an `OPENAI_API_KEY` secret (for the AI Agent). |
| `business-loan-onboarding-verification_demo.bpmn` | You want to exercise the **real** Companies House, Salesforce, and email integrations. | All integration secrets (see the setup guide). |

In the **mocks** variant, the Companies House, Salesforce (CRM), and email steps are replaced by Script Tasks labelled `[DEMO MOCK]`. They produce realistic local results — no registry lookup, CRM record, or email is real — so you can run and learn the whole flow without signing up for any integration. The AI Agent is **not** mocked, so you still set one `OPENAI_API_KEY`.

## Import the files into Camunda

You create your own project in Camunda and **upload these files** — they are not deployed for you.

1. In Camunda Console, open **Web Modeler** and **create a project** (for example, `Business Loan Onboarding`).
2. **Upload / import all of the following into that project** so the form references resolve:
   - One BPMN file — the mocks variant (recommended) or the full variant.
   - All three `.form` files (`Business Loan Application.form`, `Email Review.form`, `Loan Specialist decision.form`).
3. Open the BPMN process in Web Modeler.

Full step-by-step instructions, including Desktop Modeler, are in the [Camunda 8 SaaS setup guide](./docs/saas-setup-guide.md).

## Then

1. Follow the [Camunda 8 SaaS setup guide](./docs/saas-setup-guide.md).
2. Create the required [Connector Secrets](./docs/saas-setup-guide.md#4-create-connector-secrets) — just `OPENAI_API_KEY` for the mocks variant.
3. Deploy the process to your cluster.
4. Run the test scenarios in the [business loan flow guide](../workshop/business-loan-flow-guide.md).

## Forms

| Form file | Where it is used |
|---|---|
| `Business Loan Application.form` | The start event — the applicant submits the business-loan application. |
| `Email Review.form` | The **Review Customer Response** user task. |
| `Loan Specialist decision.form` | The **Loan Specialist decision** user task (escalation path). |

## Optional: standalone Langflow agent builder

To build no-code AI agents independently of any workflow engine, see the companion repository: [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop).

## Security

Never commit API keys, passwords, or real customer data. Store all credentials in Camunda Connector Secrets. Use synthetic test data only.

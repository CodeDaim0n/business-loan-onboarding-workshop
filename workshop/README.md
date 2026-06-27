# Workshop walkthrough

The end-to-end explanation of the **Business Loan Onboarding & Verification** process: the business journey, verification routes, AI Agent boundaries, test scenarios, and a suggested live walkthrough sequence.

| Guide | Description |
|---|---|
| [business-loan-flow-guide.md](./business-loan-flow-guide.md) | Full walkthrough and test scenarios |

## Before you start

1. Complete the [Camunda 8 SaaS setup](../camunda/docs/saas-setup-guide.md).
2. In Camunda, create a project and upload a BPMN variant plus the three forms from [`../camunda/processes/`](../camunda/processes/). Start with the **mocks** variant to run with no integrations.
3. *(Full variant only)* Complete the required [integration guides](../integrations/README.md). With the mocks variant you can skip this.
4. Companion track: [Langflow](https://github.com/CodeDaim0n/langflow-local-setup-workshop) — a no-code AI agent builder usable on its own (separate repository).

> **Mocks vs full.** The flow guide below describes the live integrations. When you run the **mocks** variant, those same steps run as built-in simulations (labelled `[DEMO MOCK]`) — the journey and outcomes are identical, but no registry, CRM, or email is contacted.

## Adapting by region

Running this outside the UK? See the [regional notes](../docs/regional-notes.md) for swapping or mocking the company registry check.

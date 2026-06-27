# Camunda assets and setup

## Folder layout

```text
camunda/
├── docs/
│   └── saas-setup-guide.md    ← Camunda 8 SaaS participant setup
└── processes/                 ← place instructor-supplied BPMN here
    └── business-loan-onboarding-verification.bpmn
```

## Instructor-provided files

Copy into `processes/`:

| File | Purpose |
|---|---|
| `business-loan-onboarding-verification.bpmn` | Main onboarding and verification process |
| Additional forms, DMN, or connector assets | As supplied by the instructor |

## Participant steps

1. Follow [Camunda 8 SaaS setup](./docs/saas-setup-guide.md).
2. Import `processes/business-loan-onboarding-verification.bpmn` in Web Modeler or Desktop Modeler.
3. Create [Connector Secrets](./docs/saas-setup-guide.md#4-create-connector-secrets) in Camunda Console.
4. Deploy to your workshop cluster.
5. Run test scenarios from the [workshop flow guide](../workshop/business-loan-flow-guide.md).

## Optional: Langflow integration

Local Langflow setup lives in a **separate repository**: [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop).

## Security

Never commit API keys, passwords, or real customer data. Use Camunda Connector Secrets only.

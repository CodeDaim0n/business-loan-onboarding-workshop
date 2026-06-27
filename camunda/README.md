# Camunda assets and setup

This folder contains the Camunda 8 setup guide and the location for the workshop process files.

## Folder layout

```text
camunda/
├── docs/
│   └── saas-setup-guide.md     # Set up, deploy, and run on Camunda 8 SaaS
└── processes/                  # Place the provided BPMN file(s) here
    └── business-loan-onboarding-verification.bpmn
```

## Process files

The Camunda process files are **provided with your workshop materials** and are not stored in this repository. Copy them into `processes/`:

| File | Purpose |
|---|---|
| `business-loan-onboarding-verification.bpmn` | Main onboarding and verification process |
| Forms, DMN tables, or connector templates | Additional assets, when supplied with the workshop |

## Steps

1. Follow the [Camunda 8 SaaS setup guide](./docs/saas-setup-guide.md).
2. Import `processes/business-loan-onboarding-verification.bpmn` in Web Modeler or Desktop Modeler.
3. Create the required [Connector Secrets](./docs/saas-setup-guide.md#4-create-connector-secrets) in Camunda Console.
4. Deploy the process to your cluster.
5. Run the test scenarios in the [business loan flow guide](../workshop/business-loan-flow-guide.md).

## Optional local AI track

Local Langflow setup is documented in a separate repository: [langflow-local-setup-workshop](https://github.com/CodeDaim0n/langflow-local-setup-workshop).

## Security

Never commit API keys, passwords, or real customer data. Store all credentials in Camunda Connector Secrets.

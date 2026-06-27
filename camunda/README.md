# Camunda process assets

Your instructor will provide the Camunda resources for this workshop. Place them in this folder before you import or deploy.

## Expected files

| File | Purpose |
|---|---|
| `business-loan-onboarding-verification.bpmn` | Main onboarding and verification process |
| Additional forms, DMN, or connector assets | As supplied by the instructor |

## What to do

1. Copy the instructor-supplied files into this `camunda/` folder.
2. Follow [Camunda 8 SaaS setup](../docs/01-camunda-saas-setup.md) to import `business-loan-onboarding-verification.bpmn`.
3. Create the required [Connector Secrets](../docs/01-camunda-saas-setup.md#4-create-connector-secrets) in Camunda Console.
4. Deploy the process to your workshop cluster.
5. Use [Business Loan flow guide](../docs/06-business-loan-flow-guide.md) for test scenarios.

## Security

- Never commit API keys, passwords, client secrets, or real customer data to Git.
- Store credentials only in **Camunda Connector Secrets**, not in BPMN XML.

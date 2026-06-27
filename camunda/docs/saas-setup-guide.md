# Camunda 8 SaaS Setup — Participant Guide

## Camunda naming convention

**Process name:** `Business Loan Onboarding & Verification`

**Recommended BPMN filename and process ID:**

```text
business-loan-onboarding-verification
```


## What you will do

In this guide, you will prepare Camunda 8 SaaS so that you can:

1. Access a Camunda cluster.
2. Open the supplied BPMN process.
3. Create the secure connection settings needed by the process.
4. Deploy the process.
5. Start and monitor a test instance.

This guide is only about **setting up Camunda**. The separate **Business Loan Onboarding & Verification and Business Loan Flow Guide** explains what the complete process does and how to test each route.

---

## Before you begin

You need:

| Item | Why you need it |
|---|---|
| Your own Camunda account | You create this yourself — no account is provided for you |
| Your own Camunda 8 SaaS cluster | You create this yourself (a free trial cluster is sufficient) |
| The provided BPMN and forms | Supplied with your workshop materials, to import |
| A modern browser | To use Console, Web Modeler, Operate and Tasklist |
| The companion setup guides | To create the keys used by the integrations |

> You sign up for and create your **own** Camunda 8 SaaS account and cluster. A free trial (signup) cluster is enough for this workshop.

### Companion guides

Complete the following only when the corresponding integration is part of your workshop environment:

- [Langflow setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) (separate repository)
- [Companies House API setup](../../integrations/companies-house/setup-guide.md)
- [Salesforce and Camunda setup](../../integrations/salesforce/setup-guide.md)
- [Personal Gmail SMTP App Password setup](../../integrations/email-gmail/smtp-app-password-setup.md)
- [Business Loan Onboarding & Verification flow guide](../../workshop/business-loan-flow-guide.md)

> **Important:** Never place an API key, password or client secret in a BPMN diagram, form field, process variable, screenshot, slide deck or Git repository.

---

# 1. Create your Camunda account and access Console

1. Open [Camunda Console](https://console.camunda.io/) and **sign up for your own account** if you do not already have one. No account is provided for you.
2. Sign in to Camunda Console.
3. Select (or accept) your default organisation.

You should see a navigation area containing services such as **Clusters**, **Web Modeler**, **Operate** and **Tasklist**.

---

# 2. Create your own SaaS cluster

Create your own cluster — one is not provided for you. The free trial cluster created at signup is sufficient.

1. In Camunda Console, open **Clusters**.
2. Select **Create cluster**.
3. Choose an available region.
4. Give the cluster a meaningful name, for example:

```text
lending-onboarding-demo
```

5. Create the cluster.
6. Wait until the cluster status is ready.

> For this workshop, use Camunda SaaS rather than installing Camunda locally.

---

# 3. Choose how to edit the process

You can use either route below.

## Option A — Web Modeler

Use **Web Modeler** if you want to work entirely in the browser.

1. In Camunda Console, open **Web Modeler**.
2. Create or open the workshop project.
3. Select **Import**.
4. Choose the supplied BPMN file:

```text
../processes/business-loan-onboarding-verification.bpmn
```

5. Save the imported process.

## Option B — Desktop Modeler

Use **Desktop Modeler** if you want to save the BPMN files locally or work from a Git folder.

1. Download and install [Camunda Modeler](https://camunda.com/download/modeler/).
2. Open Camunda Modeler.
3. Select **File → Open File**.
4. Choose:

```text
../processes/business-loan-onboarding-verification.bpmn
```

5. Use the environment selector at the top of the application.
6. Select **Connect to Camunda 8**.
7. Sign in and choose your workshop cluster.

> Confirm that Desktop Modeler is connected to the correct cluster before deploying.

---

# 4. Create Connector Secrets

Connector Secrets allow the process to use credentials without exposing them in the BPMN file.

## Open Connector Secrets

1. In Camunda Console, open your cluster.
2. Open **Connector secrets**.
3. Select **Create new secret**.
4. Enter the secret name and value.
5. Save.

## Secrets used by the supplied process

Create only the secrets that your workshop environment requires.

| Secret name | What it is used for |
|---|---|
| `OPENAI_API_KEY` | AI document analysis and AI Agent tasks |
| `LANGFLOW_API_KEY` | Langflow REST API calls when using a local Langflow flow |
| `COMPANY_HOUSE_KEY` | Companies House company and director checks |
| `SFDC_DEMO_BASE_URL` | Salesforce base or My Domain URL |
| `SFDC_DEMO_CONSUMER_KEY` | Salesforce OAuth client ID |
| `SFDC_DEMO_CONSUMER_SECRET` | Salesforce OAuth client secret |
| `INBOUND_MAIL_USER` | Email account username |
| `INBOUND_MAIL_PASSWORD` | Email App Password or approved SMTP password |
| `INBOUND_MAIL_SERVER` | Email server hostname |
| `INBOUND_MAIL_ADDRESS` | Sender email address |

The process refers to secrets in this format:

```text
{{secrets.OPENAI_API_KEY}}
```

### Example

For a personal Gmail demo account:

```text
INBOUND_MAIL_SERVER=smtp.gmail.com
INBOUND_MAIL_ADDRESS=your.demo.account@gmail.com
```

Use a **Google App Password**, not your normal Google password.

---

# 5. Check that connector templates are available

Open the imported BPMN process and select the service tasks. Confirm that the relevant connector templates appear in the properties panel.

| Process capability | Expected connector or template |
|---|---|
| Company document analysis | Camunda AI Agent |
| Companies House lookup | REST or HTTP Connector |
| Salesforce create and update | Salesforce Connector or approved HTTP connector |
| Applicant email | Email Connector |
| Applicant email response | Inbound Email Intermediate Catch Event |

If a connector is missing, do not rebuild the process from scratch. First check that:

1. You are connected to the correct Camunda 8 environment.
2. Your workshop cluster supports the required connector.
3. You are using a current version of Camunda Modeler.
4. The relevant connector setup guide has been completed.

---

# 6. Deploy the process

## From Web Modeler

1. Save the BPMN process.
2. Select **Deploy**.
3. Choose your target Camunda cluster.
4. Confirm deployment.
5. Wait for the confirmation message.

## From Desktop Modeler

1. Save the BPMN file.
2. Confirm that Desktop Modeler is connected to the target cluster.
3. Select **Deploy current diagram**.
4. Select the target cluster.
5. Confirm deployment.

Deployment uploads the process definition to Camunda. It does not start an application automatically.

---

# 7. Start a test instance

After deployment, start one test instance using the process-testing option available in your environment.

Use synthetic test data only. Do not enter real customer data.

A simple example:

```json
{
  "registeredBusinessName": "Demo Business Ltd",
  "companyRegistrationNumber": "00000006",
  "loanAmount": 25000,
  "loanPurpose": "Working capital",
  "applicantFullName": "Demo Applicant",
  "applicantEmail": "your-test-address@example.com",
  "creditCheckConsent": true,
  "informationAccuracyDeclaration": true
}
```

The complete flow guide provides richer test scenarios and expected outcomes.

---

# 8. Monitor the process

## Use Operate

Open **Operate** to inspect:

- The active BPMN step.
- Process variables.
- Connector input and output.
- Incidents and error messages.
- Completed and failed tasks.

## Use Tasklist

Open **Tasklist** to complete human tasks such as:

- Reviewing a customer response.
- Making a Loan Specialist decision.

---

# 9. Recommended participant sequence

Work in this order:

1. Confirm you can access Camunda Console.
2. Create or select the workshop cluster.
3. Import the BPMN file.
4. Create the required Connector Secrets.
5. Check that connector templates are available.
6. Deploy the process.
7. Run a basic test instance.
8. Inspect the process in Operate.
9. Complete any human task in Tasklist.
10. Follow the separate flow guide for full test scenarios.

---

# 10. Troubleshooting

## I cannot deploy the process

Check that you are connected to the intended cluster and have permission to deploy.

## A secret is not being resolved

Check that:

- The secret was created in the same cluster.
- The secret name exactly matches the BPMN reference.
- The reference uses:

```text
{{secrets.SECRET_NAME}}
```

## A connector task fails

Open the incident in Operate. Check the connector request, response and error message. Then verify the relevant setup guide before changing the BPMN.

## I cannot see or complete a human task

Confirm you are using the correct Tasklist environment and that the task is assigned to you or available to your group.

---

# 11. Final checklist

- [ ] I can access Camunda Console.
- [ ] A Camunda SaaS cluster is ready.
- [ ] I can open the supplied BPMN file.
- [ ] Required Connector Secrets have been created.
- [ ] Connector templates are available.
- [ ] The process has been deployed.
- [ ] I can start a test instance.
- [ ] I can inspect the instance in Operate.
- [ ] I can complete a human task in Tasklist.

---

## Official references

- [Camunda documentation](https://docs.camunda.io/)
- [Camunda Console](https://console.camunda.io/)
- [Camunda Modeler download](https://camunda.com/download/modeler/)
- [Connect Desktop Modeler to Camunda 8](https://docs.camunda.io/docs/components/modeler/desktop-modeler/connect-to-camunda-8/)
- [Connector Secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)
- [Using secrets in connectors](https://docs.camunda.io/docs/components/connectors/use-connectors/#using-secrets)
- [Operate user guide](https://docs.camunda.io/docs/components/operate/userguide/)
- [Tasklist user guide](https://docs.camunda.io/docs/components/tasklist/userguide/)
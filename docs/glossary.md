# Glossary

Key terms used throughout this workshop.

| Term | Meaning |
|---|---|
| **BPMN** | Business Process Model and Notation — the standard used to model the process. |
| **Camunda 8** | The process orchestration platform used to deploy and run the process. |
| **Camunda Console** | The web app for managing clusters, secrets, and Web Modeler. |
| **Web Modeler** | The browser-based BPMN editor in Camunda Console. |
| **Desktop Modeler** | The downloadable BPMN editor that can deploy to a cluster. |
| **Operate** | The Camunda app for monitoring running and completed process instances. |
| **Tasklist** | The Camunda app where people complete human tasks. |
| **Connector** | A reusable building block that lets a process call an external service (for example REST, Salesforce, or email). |
| **Connector Secret** | A credential stored at the cluster level and referenced as `{{secrets.NAME}}`, so it never appears in the BPMN. |
| **REST Connector** | A connector for calling any REST API from a process. |
| **AI Agent** | A Camunda task that uses a model to choose among actions already defined in the process. |
| **Verification route** | The controlled path the process takes after combining the parallel checks (for example passed, exception, or hard fail). |
| **Companies House** | The UK company registry used for the company verification step. |
| **Salesforce Lead** | The CRM record created or updated for each application. |
| **App Password** | A 16-character Google password that lets an app sign in over SMTP without your main password. |
| **Langflow** | The optional low-code tool for building AI flows locally. |
| **Cloudflare Tunnel** | A secure outbound tunnel that gives a local service a public HTTPS address. |
| **Synthetic data** | Made-up test data used in place of real customer information. |

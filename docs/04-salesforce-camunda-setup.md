# Salesforce Setup Guide — Camunda Integration

**Purpose:** Create Salesforce OAuth credentials so Camunda can create, update and query Salesforce records without storing a user password in BPMN.

**Recommended route:** Create a **Salesforce External Client App** and use the **OAuth 2.0 Client Credentials** flow.

> Salesforce is moving new integrations toward External Client Apps. Some older Camunda and Salesforce documentation still refers to a Connected App. Use the External Client App route for a new setup unless your Salesforce administrator requires the legacy route.

---

## 1. What you need

| Item | Why |
|---|---|
| Salesforce Developer Edition, sandbox or production org | Hosts CRM objects and OAuth client |
| Salesforce administrator permission | Creates the External Client App |
| Dedicated Salesforce integration user | Defines the minimum CRM permissions Camunda receives |
| Camunda cluster | Stores OAuth credentials as secrets |

Useful official links:

- [Salesforce External Client Apps](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm&language=en_US&type=5)
- [Create an External Client App](https://developer.salesforce.com/docs/platform/mobile-sdk/guide/eca-create.html)
- [OAuth client credentials flow](https://help.salesforce.com/s/articleView?id=xcloud.remoteaccess_oauth_client_credentials_flow.htm&language=en_US&type=5)
- [Camunda Salesforce Connector](https://docs.camunda.io/docs/components/connectors/out-of-the-box-connectors/salesforce/)

---

## 2. Prepare a dedicated integration user

Create or identify a Salesforce user for the integration, for example:

```text
camunda.integration@yourdomain.example
```

Assign only the permissions needed for the demo. For a basic lending onboarding demo, this normally means access to:

- Lead
- Account
- Contact
- Any custom objects used by your process

The user should not be a Salesforce System Administrator unless there is no alternative for a short-lived development demo.

---

## 3. Create an External Client App

1. Sign in to Salesforce as an administrator.
2. Open **Setup**.
3. In **Quick Find**, search for:

```text
External Client App Manager
```

4. Select **New External Client App**.
5. Complete the basic information:

| Field | Suggested value |
|---|---|
| External Client App Name | `Camunda Lending Demo` |
| API Name | Accept the generated name |
| Contact Email | Your own email address |

6. Save the basic application.

---

## 4. Configure OAuth

Open the app settings and configure OAuth.

1. Enable OAuth.
2. Select the OAuth scopes needed by the integration.

For the demo, use the minimum scopes that permit API access. Typical options include:

```text
Manage user data via APIs (api)
```

3. Configure the **Client Credentials** flow.
4. Assign the dedicated integration user as the user context / run-as user where Salesforce asks for it.
5. Save.

> The exact label names can vary slightly by Salesforce release and org configuration. The goal is always the same: allow a machine-to-machine OAuth client to obtain a token that has the permissions of your restricted integration user.

---

## 5. Retrieve the client ID and client secret

1. In **External Client App Manager**, open the app.
2. Open **OAuth Settings**.
3. Select **Consumer Key and Secret**.
4. Complete any identity verification prompted by Salesforce.
5. Copy:
   - Consumer Key = **Client ID**
   - Consumer Secret = **Client Secret**

Store both immediately in your password manager.

---

## 6. Identify your Salesforce base URL

Use your My Domain URL, for example:

```text
https://your-domain.my.salesforce.com
```

Do not use a generic URL copied from another environment.

For a sandbox, use the sandbox’s own domain/instance URL.

---

## 7. Store credentials in Camunda Secrets

In Camunda Console, create these secrets:

```text
SALESFORCE_CLIENT_ID
SALESFORCE_CLIENT_SECRET
```

Recommended optional secret:

```text
SALESFORCE_BASE_URL
```

Use the exact Consumer Key and Consumer Secret values from Salesforce.

---

## 8. Configure the Camunda Salesforce Connector

Add a **Salesforce Connector** task in your BPMN process.

### Authentication

| Field | Value |
|---|---|
| Authentication type | OAuth 2.0 Client Credentials |
| Client ID | `{{secrets.SALESFORCE_CLIENT_ID}}` |
| Client Secret | `{{secrets.SALESFORCE_CLIENT_SECRET}}` |
| Base URL | Your Salesforce My Domain URL |
| API version | Use the version supported by your org and connector |

### Typical first operation

Choose:

```text
Create sObject record
```

For a business onboarding demo, use object:

```text
Lead
```

Then map only fields your org has and your integration user can write.

Example FEEL body:

```feel
= {
  "Company": registeredBusinessName,
  "FirstName": applicantFirstName,
  "LastName": applicantLastName,
  "Email": applicantEmail,
  "Phone": applicantPhone,
  "Status": "New",
  "Description": "Created by Camunda lending onboarding demo."
}
```

> Field names are Salesforce API field names. Your visible page labels may differ. Confirm each field in Salesforce Object Manager.

---

## 9. Test safely

1. First run the connector with a test Lead.
2. Confirm the record appears in Salesforce.
3. Confirm that the record contains only intended demo data.
4. Test an update operation.
5. Review error handling for missing mandatory fields.

For demo reliability, create a separate test record type or use a clear naming convention such as:

```text
DEMO - Camunda - {applicantName}
```

---

## 10. Legacy Connected App fallback

Some older orgs and older documentation still use a **Connected App**.

Use it only when:

- Your org does not yet support External Client Apps for this integration, or
- Your Salesforce administrator explicitly requires it.

The minimum concept is the same:

1. Create a Connected App.
2. Enable OAuth.
3. Enable OAuth 2.0 Client Credentials Flow.
4. Retrieve Consumer Key and Consumer Secret.
5. Store them in Camunda Secrets.

Do not mix credentials from an External Client App and a Connected App.

---

## 11. Common issues

### Invalid client credentials

Check that:

- Client ID is the Consumer Key.
- Client Secret is the Consumer Secret.
- No extra spaces or quotation marks were copied.
- The Camunda secret reference uses the exact secret name.

### Authentication works but create/update fails

The integration user may be missing object or field permissions. Check:

- Object permissions for Lead, Account, Contact or custom object.
- Field-level security.
- Record type access.
- Validation rules.
- Required fields.

### Unsupported Media Type / HTTP 415

The connector request body may not be valid JSON or may include fields that do not belong to the selected Salesforce object. Use a single JSON object and Salesforce API field names.

### 404 or wrong instance

Confirm the base URL is the actual My Domain URL for the org and environment you are using.

---

## 12. Security checklist

- [ ] Dedicated, least-privilege integration user is used.
- [ ] OAuth client credentials are stored only in Camunda Secrets.
- [ ] No Salesforce credentials in BPMN XML or process variables.
- [ ] Test records are clearly labelled as demo data.
- [ ] API field names are confirmed in Salesforce Object Manager.
- [ ] Error paths are tested for required fields and validation rules.

---

## Official references

- [External Client Apps](https://help.salesforce.com/s/articleView?id=xcloud.external_client_apps.htm&language=en_US&type=5)
- [Create an External Client App](https://developer.salesforce.com/docs/platform/mobile-sdk/guide/eca-create.html)
- [Configure External Client App OAuth settings](https://help.salesforce.com/s/articleView?id=xcloud.configure_external_client_app_oauth_settings.htm&language=en_US&type=5)
- [OAuth 2.0 client credentials flow](https://help.salesforce.com/s/articleView?id=xcloud.remoteaccess_oauth_client_credentials_flow.htm&language=en_US&type=5)
- [Camunda Salesforce Connector](https://docs.camunda.io/docs/components/connectors/out-of-the-box-connectors/salesforce/)
- [Camunda Secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)

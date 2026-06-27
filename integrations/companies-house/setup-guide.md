# Companies House API Setup Guide

**Purpose:** Create a Companies House API key, test a company lookup, and use it safely from a Camunda process.

**Use this guide for:** public company-data lookups such as company profile, company name search, officers, filing history and registered office details.

> For the onboarding demo, use a normal **API key** for public `GET` requests. You do **not** need OAuth unless you are building filing or account-authorised functionality.

---

## 1. What you need

| Item | Why |
|---|---|
| Companies House account | Creates the application and API key |
| Camunda cluster | Stores the key as a secret and calls the API |
| A known company number | Lets you test the lookup |

Useful official links:

- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Create an application](https://developer.company-information.service.gov.uk/how-to-create-an-application)
- [API authentication](https://developer.company-information.service.gov.uk/authentication)
- [REST API overview](https://developer.company-information.service.gov.uk/overview)

---

## 2. Create a Companies House application

1. Open the [Companies House Developer Hub](https://developer.company-information.service.gov.uk/).
2. Select **Sign in / Register**.
3. Create an account or sign in.
4. Create a new application.
5. Give it a clear name, for example:

```text
camunda-lending-demo
```

6. Choose the environment:
   - **Live application** — use this for normal public company lookups.
   - **Test application** — use this only when working with the Companies House sandbox/test data.
7. Save the application.

---

## 3. Create the API key

1. Open your application.
2. Select **Create new key**.
3. Choose client type:

```text
API key
```

4. Give the key a clear name:

```text
camunda-company-lookup
```

5. Select **Create**.
6. Copy the key immediately and store it in a password manager.

> Treat the key as a password. Do not place it in BPMN XML, screenshots, prompts, GitHub, email or documents.

---

## 4. Test the key locally

Companies House uses HTTP Basic authentication. Your API key is the **username** and the password is blank.

### Windows PowerShell

```powershell
curl.exe -u YOUR_COMPANIES_HOUSE_API_KEY: `
  https://api.company-information.service.gov.uk/company/00000006
```

### macOS Terminal

```bash
curl -u YOUR_COMPANIES_HOUSE_API_KEY:   https://api.company-information.service.gov.uk/company/00000006
```

A successful response returns company-profile JSON.

> `00000006` is an example company number used in Companies House documentation. For your own demo, test with a real company number that you are permitted to use.

---

## 5. Useful API endpoints for the onboarding demo

| Purpose | Endpoint |
|---|---|
| Look up a company by company number | `GET /company/{company_number}` |
| Search companies by name | `GET /search/companies?q={query}` |
| Get officers | `GET /company/{company_number}/officers` |
| Get filing history | `GET /company/{company_number}/filing-history` |

Base URL:

```text
https://api.company-information.service.gov.uk
```

Example company-profile endpoint:

```text
https://api.company-information.service.gov.uk/company/00000006
```

---

## 6. Store the key in Camunda Secrets

In Camunda Console:

1. Open the relevant cluster.
2. Open **Secrets**.
3. Create a secret:

```text
COMPANY_HOUSE_KEY
```

4. Paste the API key as the secret value.
5. Save.

Do not store the key in a process variable.

---

## 7. Configure the Camunda REST Connector

Add a **REST Connector** task to the BPMN process.

### Connection settings

| Field | Value |
|---|---|
| Method | `GET` |
| URL | `https://api.company-information.service.gov.uk/company/{companyNumber}` |
| Authentication | `Basic` |
| Username | `{{secrets.COMPANY_HOUSE_KEY}}` |
| Password | Leave blank |

For a dynamic Camunda variable called `companyNumber`, use:

```feel
= "https://api.company-information.service.gov.uk/company/" + companyNumber
```

### Result variable

Store the response in:

```text
companiesHouseResponse
```

Then inspect the actual returned JSON in a test instance before mapping fields into business variables.

---

## 8. Suggested fields to map

For a simple business-onboarding decision, consider mapping:

| API response field | Suggested process variable |
|---|---|
| `company_name` | `companiesHouseCompanyName` |
| `company_number` | `companiesHouseCompanyNumber` |
| `company_status` | `companiesHouseCompanyStatus` |
| `date_of_creation` | `companiesHouseIncorporationDate` |
| `type` | `companiesHouseCompanyType` |
| `registered_office_address` | `companiesHouseRegisteredAddress` |

Do not treat a successful API response alone as a lending approval. It is a verification signal that should be combined with applicant-provided data, document review and your business rules.

---

## 9. Common issues

### 401 Unauthorized

Check that:

- The API key is correct.
- You used the key as the Basic Authentication username.
- The password is blank.
- The secret reference is exactly `{{secrets.COMPANY_HOUSE_KEY}}`.

### 404 Not Found

Check the company number. Use the number only, without spaces or company name text.

### 429 Too Many Requests

Slow down or limit repeated API calls. Do not call Companies House inside a loop without rate control.

### Wrong environment

A sandbox key and a live key are not interchangeable. Confirm that your API key and endpoint match the environment you chose.

---

## 10. Security checklist

- [ ] API key stored in a password manager.
- [ ] API key stored in Camunda Secrets.
- [ ] No key in BPMN XML or process variables.
- [ ] Test company lookup works before adding downstream logic.
- [ ] API output is treated as verification data, not as a final lending decision.

---

## Official references

- [Companies House Developer Hub](https://developer.company-information.service.gov.uk/)
- [Create an application](https://developer.company-information.service.gov.uk/how-to-create-an-application)
- [API authentication](https://developer.company-information.service.gov.uk/authentication)
- [Developer guidelines](https://developer.company-information.service.gov.uk/developer-guidelines)
- [REST API overview](https://developer.company-information.service.gov.uk/overview)
- [Camunda REST Connector](https://docs.camunda.io/docs/components/connectors/protocol/rest/)
- [Camunda Secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)

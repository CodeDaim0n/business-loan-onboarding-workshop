# Business Loan Onboarding & Verification — Participant Flow Guide

## Camunda naming convention

**Process name:** `Business Loan Onboarding & Verification`

**Recommended BPMN filename and process ID:**

```text
business-loan-onboarding-verification
```


## What you will learn

This guide explains the complete business-loan onboarding process supplied in:

```text
../camunda/processes/business-loan-onboarding-verification.bpmn
```

By the end, you will understand how the process:

1. Creates or reuses a CRM Lead.
2. Checks company details and uploaded business evidence in parallel.
3. Assigns a controlled verification route.
4. Uses an AI Agent only within defined process boundaries.
5. Requests more information, checks directors or escalates to a Loan Specialist when needed.
6. Updates Salesforce and sends a customer outcome email.

> This is a learning demonstration. Use only synthetic test data. The “credit score” step in the process is simulated and is not a real credit-bureau integration.

---

# 1. Before you begin

Complete the participant-facing Camunda setup guide first:

- [Camunda 8 SaaS Setup — Participant Guide](../camunda/docs/saas-setup-guide.md)

You may also need:

- [Companies House API setup](../integrations/companies-house/setup-guide.md)
- [Salesforce and Camunda setup](../integrations/salesforce/setup-guide.md)
- [Personal Gmail SMTP App Password setup](../integrations/email-gmail/smtp-app-password-setup.md)
- [Langflow setup](https://github.com/CodeDaim0n/langflow-local-setup-workshop) (separate repository)

---

# 2. Business journey at a glance

```text
Application received
        |
        v
Create or reuse CRM Lead
        |
        v
Run two checks in parallel
  ├─ Validate company details through Companies House
  └─ Analyse company-proof document with AI
        |
        v
Set verification route
        |
        +--> Hard fail → update CRM → send outcome email
        |
        +--> Passed or exception → AI Agent selects a permitted next action
                                      |
                                      +--> Request clarification and wait for reply
                                      +--> Get simulated credit score
                                      +--> Check company directors
                                      +--> Escalate to Loan Specialist
                                      |
                                      v
                                 update CRM → send outcome email → complete
```

The process is designed so that the BPMN model controls the path. The AI Agent can recommend or trigger only the actions that have already been defined in the process.

---

# 3. Information used by the process

The application form or start payload provides information such as:

| Information | Example |
|---|---|
| Registered business name | `Demo Business Ltd` |
| Company registration number | `00000006` |
| Business and trading address | Registered and operating addresses |
| Company proof type | Bank statement, VAT certificate or incorporation evidence |
| Company proof document | Uploaded supporting document |
| Applicant details | Name, email, phone and role |
| Loan details | Amount, purpose and requested term |
| Consent | Consent for simulated credit check |
| Declaration | Confirmation that information is accurate |

Use realistic-looking **demo data**, but do not use live customer data.

---

# 4. Step 1 — Create or reuse the CRM Lead

The process begins by creating a Salesforce Lead containing the applicant and business details.

The initial Lead status is:

```text
New
```

### Existing Lead path

If the CRM identifies an existing Lead, the process continues using that Lead rather than creating uncontrolled duplicates.

### What to check

1. Start a process with a new test applicant.
2. Confirm that a Lead is created in Salesforce.
3. Start again using the same test details.
4. Confirm that the duplicate handling route behaves as expected.

---

# 5. Step 2 — Run two initial checks in parallel

After the CRM step, the process performs two checks at the same time.

## A. Validate company details

The process calls Companies House using the submitted company registration number.

Typical checks include:

- Does the company exist?
- Is the company status active?
- Does the returned company number match the application?
- What are the registered company details?

The response is stored for use by later steps.

## B. Analyse company proof

The process uses a Camunda AI Agent task to inspect the uploaded company-proof document.

The analysis is intended to assess:

- Whether the document is readable.
- Whether it matches the proof type selected by the applicant.
- Whether the business name and company number match.
- Whether the document supports the stated address, VAT number or other details.
- Whether evidence is missing or inconsistent.

The analysis should use only information visible in the submitted document.

---

# 6. Step 3 — Set the verification route

The process combines the Companies House result, company-proof result, consent and declaration into one of three routes.

| Route | Meaning |
|---|---|
| `HARD_FAIL` | The company is inactive or the company number does not match. |
| `PASSED` | Company data and evidence match, evidence passes, consent is present and the declaration is confirmed. |
| `EXCEPTION` | Evidence is incomplete, unreadable, mismatched or requires further review. |

The route is deliberately deterministic. It is set by BPMN logic, not by the AI Agent.

---

# 7. Step 4 — AI Agent handles permitted next actions

For passed or exception cases, the process enters:

```text
AI AGENT - Handle Loan Application Verification
```

The agent receives factual case context and returns a structured decision.

## Expected decision fields

| Output | Meaning |
|---|---|
| `verificationDecision` | Provisional offer, request information, manual review, reject or error |
| `nextAction` | The next BPMN action to take |
| `loanApplicationStatus` | The Salesforce status to apply |
| `creditScore` | Simulated score when applicable |
| `creditRiskBand` | Not checked, low, standard or elevated |
| `reasonCodes` | Auditable reasons |
| `verificationSummary` | Concise case summary |
| `requiredActions` | Missing evidence or actions needed |

## Permitted next actions

```text
GET_CREDIT_SCORE
CHECK_DIRECTORS
ASK_FOR_PROOFS_OR_QUESTIONS
ESCALATE_TO_LOAN_SPECIALIST
SEND_PROVISIONAL_OFFER
SEND_REJECTION
WAIT_FOR_APPLICANT_RESPONSE
EXIT
```

The process only permits these listed actions. This prevents the agent from inventing an uncontrolled action or bypassing a process gate.

---

# 8. Step 5 — Request clarification and review a reply

When evidence is missing or inconsistent, the process sends an email asking the applicant for the specific information required.

The message should:

- Clearly state what is missing.
- State that the application is still under review.
- Avoid disclosing internal risk rules, hidden model reasoning or internal scoring.
- Ask the applicant to reply with relevant supporting evidence.

The process then waits for an applicant reply.

## Customer response review

When an email reply is received, the process creates a human task:

```text
Review Customer Response
```

The reviewer checks the reply and decides whether to:

- Continue the application.
- Request more information.
- Escalate to a Loan Specialist.
- Reject the application.

---

# 9. Step 6 — Simulated credit score

When the process selects **Get Credit Score**, it runs a simulated demo score only if consent was provided.

Example result:

```json
{
  "score": 742,
  "source": "SIMULATED_DEMO_SCORE"
}
```

If consent is not provided, the result records that no score was run.

> The score is simulated for demonstration purposes. It is not an external credit-bureau check and must not be presented as one.

---

# 10. Step 7 — Check company directors

When an authority or ownership question needs further validation, the process calls Companies House officer information.

This is used to support questions such as:

- Is the applicant listed as a director or officer?
- Does the applicant’s stated role need human verification?
- Is additional authority evidence required?

This API result is a verification input. It is not a final lending decision by itself.

---

# 11. Step 8 — Loan Specialist decision

Some cases need a human decision. The process creates a Tasklist task:

```text
Loan Specialist decision
```

The specialist receives a concise factual handover containing:

- The verification route.
- Companies House outcome.
- Company-proof outcome.
- Authority or director-check outcome.
- Simulated credit score and risk band, where available.
- Previous actions taken.
- Outstanding issues.

The specialist records a decision and rationale. The process then continues to Salesforce update and customer communication.

---

# 12. Step 9 — Update Salesforce

The process updates the Salesforce Lead with a controlled status and an auditable description.

Allowed statuses are:

```text
New
Contacted
Nurturing
Qualified
Unqualified
```

The description should include:

- Verification outcome.
- Main reason.
- Next action.
- Companies House result.
- Company-proof result.
- Simulated score/risk band when relevant.
- Evidence requested, if the application is pending.

Do not store internal hidden reasoning or API keys in Salesforce descriptions.

---

# 13. Step 10 — Send the outcome email

The process sends an outcome email that matches the decision.

| Outcome | What the email should communicate |
|---|---|
| Provisional offer | It is provisional and subject to final bank approval. |
| More information needed | The exact evidence or clarification required. |
| Specialist review | The application is being reviewed and the bank will be in touch. |
| Rejection | A respectful concise outcome without hidden scoring or internal logic. |

After the email, the process completes.

---

# 14. Participant test scenarios

## Scenario A — Successful verification

Use:

- An active company number.
- A readable company-proof document.
- Matching company data.
- Consent set to `true`.
- Accuracy declaration set to `true`.

Expected result:

```text
verificationRoute = PASSED
```

The process should continue through the AI Agent, update Salesforce and send an outcome email.

## Scenario B — Hard fail

Use:

- An inactive company, or
- A company number that does not match the Companies House response.

Expected result:

```text
verificationRoute = HARD_FAIL
```

The simulated score should not run. Salesforce should be updated with the appropriate rejected/unqualified outcome.

## Scenario C — Evidence exception

Use:

- A readable but mismatched document, or
- An unreadable document, or
- Missing supporting evidence.

Expected result:

```text
verificationRoute = EXCEPTION
```

The process should ask the applicant for clarification or route the case to a human reviewer.

## Scenario D — Director check

Use:

- An active company.
- An applicant role that requires an authority check.

Expected result:

```text
nextAction = CHECK_DIRECTORS
```

The process should call the Companies House officer endpoint before completing the final decision.

## Scenario E — Specialist escalation

Use:

- Valid company data but an unresolved evidence or authority issue.

Expected result:

```text
nextAction = ESCALATE_TO_LOAN_SPECIALIST
```

Complete the Tasklist task and confirm that Salesforce and email steps continue afterwards.

---

# 15. Suggested live-demo sequence

1. Show the BPMN diagram.
2. Start a successful test application.
3. Show the Salesforce Lead being created.
4. Show the parallel company and document checks.
5. Inspect the verification route in Operate.
6. Show the structured AI Agent result.
7. Show the Salesforce update and customer email.
8. Start an exception case.
9. Show the clarification request and review task.
10. Use Operate to show the process audit trail.

---

# 16. Practical safety notes

- Use synthetic test data only.
- Store all credentials in Camunda Connector Secrets.
- Do not expose keys, passwords or client secrets during a screen share.
- Treat the simulated credit score as a demo mechanism only.
- Keep customer communications factual and respectful.
- Retain human decision points for exceptions and material lending decisions.

---

# 17. Completion checklist

- [ ] I understand the two parallel verification checks.
- [ ] I understand the three verification routes.
- [ ] I understand the AI Agent’s permitted next actions.
- [ ] I can identify where human review happens.
- [ ] I can run a happy-path test.
- [ ] I can run an exception test.
- [ ] I can inspect variables and incidents in Operate.
- [ ] I can confirm Salesforce and email outcomes.

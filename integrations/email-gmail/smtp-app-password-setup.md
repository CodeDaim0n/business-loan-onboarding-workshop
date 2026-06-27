# Personal Gmail SMTP Setup Guide — App Password

**Purpose:** Send demo emails from a personal Gmail account using SMTP and a Google App Password.

**Use this guide for:** a controlled learning demo, prototype or personal test account.

> This guide is for a personal Gmail account. Do not use a personal mailbox for production, bulk customer communications, regulated data or sensitive customer information.

---

## 1. What you need

| Item | Why |
|---|---|
| Personal Gmail account | Sender mailbox |
| 2-Step Verification enabled | Required before Google will allow App Passwords |
| Google App Password | Used instead of your normal Google password |
| SMTP-capable email component or connector | Sends the email |
| Camunda cluster, if calling from Camunda | Stores the App Password as a secret |

Useful official links:

- [Google App Passwords](https://support.google.com/accounts/answer/185833)
- [Turn on 2-Step Verification](https://support.google.com/accounts/answer/185839)
- [Gmail SMTP documentation](https://developers.google.com/workspace/gmail/imap/imap-smtp)

---

## 2. Understand the safe use case

A Google App Password is a 16-character passcode that lets an app sign in to your Gmail account without using your normal Google password.

Use it only when the tool you are connecting does **not** support “Sign in with Google” or OAuth.

For a simple personal demo, SMTP + App Password is usually quicker than implementing Gmail OAuth.

---

## 3. Turn on 2-Step Verification

1. Open your [Google Account security settings](https://myaccount.google.com/security).
2. Under **How you sign in to Google**, select **2-Step Verification**.
3. Follow the steps to enable it.
4. Complete the setup before continuing.

> App Passwords are available only when 2-Step Verification is enabled.

---

## 4. Create a Google App Password

1. Open:

```text
https://myaccount.google.com/apppasswords
```

2. Sign in to the Gmail account you want to use.
3. Create a new app password.
4. Give it a clear name:

```text
camunda-lending-demo
```

5. Select **Create**.
6. Copy the 16-character passcode immediately.

> Google shows the App Password only once. Store it in a password manager. If it is lost, create a new one and revoke the old one.

### If “App Passwords” is missing

It may be unavailable because:

- 2-Step Verification is not enabled.
- The account uses Advanced Protection.
- You are signed into a work, school or managed account where the administrator has disabled it.
- Your 2-Step Verification setup uses security keys only.

For a personal demo, use a personal Gmail account with standard 2-Step Verification.

---

## 5. SMTP settings

Use these settings in your email connector or SMTP component.

| Field | Value |
|---|---|
| SMTP host | `smtp.gmail.com` |
| Port — TLS / STARTTLS | `587` |
| Port — SSL | `465` |
| Username | Your full Gmail address, e.g. `your.name@gmail.com` |
| Password | The Google App Password, not your normal Google password |
| From address | Usually the same Gmail address |
| Authentication | Enabled |

Recommended first choice:

```text
Host: smtp.gmail.com
Port: 587
Security: STARTTLS / TLS
```

If your component explicitly asks for SSL rather than STARTTLS, use:

```text
Host: smtp.gmail.com
Port: 465
Security: SSL
```

---

## 6. Store credentials safely in Camunda

In Camunda Console, create:

```text
GMAIL_SMTP_USERNAME
GMAIL_SMTP_APP_PASSWORD
```

Optional:

```text
GMAIL_SMTP_HOST
GMAIL_SMTP_PORT
```

Never store the App Password in:

- BPMN XML
- Process variables
- A FEEL expression
- Screenshots
- GitHub
- A shared slide deck

---

## 7. Generic SMTP connector configuration

Use the following values in the SMTP connector or mail component that you are using.

| Connector field | Value |
|---|---|
| Host | `smtp.gmail.com` |
| Port | `587` |
| Username | `{{secrets.GMAIL_SMTP_USERNAME}}` |
| Password | `{{secrets.GMAIL_SMTP_APP_PASSWORD}}` |
| TLS / STARTTLS | Enabled |
| From | Your Gmail address |
| To | A test recipient address |
| Subject | `Camunda demo email test` |
| Body | `This is a successful SMTP test from the Camunda demo.` |

> Connector field labels vary. The principle does not: use Gmail SMTP, full Gmail address as username, App Password as password and encryption enabled.

---

## 8. Test before using it in the process

Send one test message to an inbox you can access.

Use:

```text
Subject: Camunda demo email test
Body: This is a successful SMTP test.
```

Confirm:

1. The message arrives.
2. It is not in spam.
3. The sender appears correctly.
4. The app password is not visible anywhere in process data.

---

## 9. Common issues

### Username/password rejected

Check that:

- You used the 16-character App Password, not your normal Gmail password.
- The username is the full Gmail address.
- 2-Step Verification is enabled.
- The App Password has not been revoked.

### Cannot connect

Check:

- Host is `smtp.gmail.com`.
- Port is `587` with STARTTLS or `465` with SSL.
- The network allows outbound SMTP traffic.
- Your connector’s security setting matches the selected port.

### App Password menu not available

Use a personal Gmail account with 2-Step Verification. App Passwords may not be available in managed Google Workspace accounts or accounts enrolled in Advanced Protection.

### Email goes to spam

Use a simple subject and body, test with your own mailbox, and do not send high volumes from a personal account.

---

## 10. Security and operational limits

- Use a dedicated personal Gmail account for demos rather than your primary mailbox.
- Revoke the App Password immediately after the event if it is no longer needed.
- Do not send customer personal data through a personal email account.
- Do not use this route for production workflows.
- For production, use an approved organisation mail service with OAuth, governance, audit logging and controlled sender identities.

---

## 11. Final checklist

- [ ] 2-Step Verification is enabled.
- [ ] A dedicated App Password has been created.
- [ ] SMTP host and port are configured.
- [ ] App Password is stored only in Camunda Secrets or a secure credential store.
- [ ] A test email has been received.
- [ ] The App Password will be revoked after the demo.

---

## Official references

- [Google App Passwords](https://support.google.com/accounts/answer/185833)
- [Turn on 2-Step Verification](https://support.google.com/accounts/answer/185839)
- [Gmail SMTP documentation](https://developers.google.com/workspace/gmail/imap/imap-smtp)
- [Camunda Secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)

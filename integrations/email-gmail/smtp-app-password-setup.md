# Personal Gmail Email Setup Guide — App Password (SMTP + IMAP)

**Purpose:** Send demo emails from a personal Gmail account using SMTP, and let the process listen for applicant replies using IMAP — both with a single Google App Password.

**Use this guide for:** a controlled learning demo, prototype or personal test account.

> **Secret names used by this workshop's process.** The provided BPMN reads these exact Connector Secret names. Create them with these names so the process resolves them without edits:
>
> | Secret name | Value | Used by |
> |---|---|---|
> | `INBOUND_MAIL_USER` | Your full Gmail address (e.g. `your.name@gmail.com`) | SMTP + IMAP login |
> | `INBOUND_MAIL_PASSWORD` | The 16-character Google App Password | SMTP + IMAP login |
> | `INBOUND_MAIL_SERVER` | `smtp.gmail.com` | Outgoing (SMTP) host |
> | `INBOUND_MAIL_ADDRESS` | Your Gmail address (sender / "from") | Outgoing (SMTP) "from" |
> | `INBOUND_MAIL_IMAP_SERVER` | `imap.gmail.com` | Incoming (IMAP) host |
> | `INBOUND_MAIL_IMAP_PORT` | `993` | Incoming (IMAP) port |
> | `INBOUND_MAIL_FOLDER` | `INBOX` | Mailbox folder the listener watches |
>
> Only needed for the **full** variant. The mocks variant simulates email and needs none of these.

> This guide is for a personal Gmail account. Do not use a personal mailbox for production, bulk customer communications, regulated data or sensitive customer information.

---

## 1. What you need

| Item | Why |
|---|---|
| Personal Gmail account | Sender mailbox |
| 2-Step Verification enabled | Required before Google will allow App Passwords |
| Google App Password | Used instead of your normal Google password (works for SMTP and IMAP) |
| SMTP-capable email component or connector | Sends the email |
| IMAP enabled in Gmail | Lets the process listen for applicant replies |
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

## 5. SMTP and IMAP settings

Use these settings in your email connector or mail component.

### Outgoing — SMTP (send)

| Field | Value | Workshop secret |
|---|---|---|
| SMTP host | `smtp.gmail.com` | `INBOUND_MAIL_SERVER` |
| Port — TLS / STARTTLS | `587` | — |
| Port — SSL | `465` | — |
| Username | Your full Gmail address, e.g. `your.name@gmail.com` | `INBOUND_MAIL_USER` |
| Password | The Google App Password, not your normal Google password | `INBOUND_MAIL_PASSWORD` |
| From address | Usually the same Gmail address | `INBOUND_MAIL_ADDRESS` |
| Authentication | Enabled | — |

### Incoming — IMAP (listen for applicant replies)

| Field | Value | Workshop secret |
|---|---|---|
| IMAP host | `imap.gmail.com` | `INBOUND_MAIL_IMAP_SERVER` |
| IMAP port | `993` (SSL) | `INBOUND_MAIL_IMAP_PORT` |
| Username | Your full Gmail address | `INBOUND_MAIL_USER` |
| Password | The Google App Password | `INBOUND_MAIL_PASSWORD` |
| Folder to listen | `INBOX` | `INBOUND_MAIL_FOLDER` |

> **Enable IMAP in Gmail first.** In Gmail, open **Settings → See all settings → Forwarding and POP/IMAP → Enable IMAP → Save Changes**. See [Gmail IMAP setup](https://support.google.com/mail/answer/7126229).

Recommended SMTP first choice:

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

In Camunda Console, create the secrets exactly as the process expects:

```text
INBOUND_MAIL_USER          # your full Gmail address
INBOUND_MAIL_PASSWORD      # the 16-character App Password
INBOUND_MAIL_SERVER        # smtp.gmail.com
INBOUND_MAIL_ADDRESS       # your Gmail address (sender)
INBOUND_MAIL_IMAP_SERVER   # imap.gmail.com
INBOUND_MAIL_IMAP_PORT     # 993
INBOUND_MAIL_FOLDER        # INBOX
```

> Use these exact names. The supplied BPMN references them directly, so any other naming will fail to resolve.

Never store the App Password in:

- BPMN XML
- Process variables
- A FEEL expression
- Screenshots
- GitHub
- A shared slide deck

---

## 7. Connector configuration

### Outgoing — SMTP send

| Connector field | Value |
|---|---|
| Host | `{{secrets.INBOUND_MAIL_SERVER}}` |
| Port | `587` |
| Username | `{{secrets.INBOUND_MAIL_USER}}` |
| Password | `{{secrets.INBOUND_MAIL_PASSWORD}}` |
| TLS / STARTTLS | Enabled |
| From | `{{secrets.INBOUND_MAIL_ADDRESS}}` |
| To | A test recipient address |
| Subject | `Camunda demo email test` |
| Body | `This is a successful SMTP test from the Camunda demo.` |

### Incoming — IMAP listener (applicant response)

| Connector field | Value |
|---|---|
| Host | `{{secrets.INBOUND_MAIL_IMAP_SERVER}}` |
| Port | `{{secrets.INBOUND_MAIL_IMAP_PORT}}` |
| Username | `{{secrets.INBOUND_MAIL_USER}}` |
| Password | `{{secrets.INBOUND_MAIL_PASSWORD}}` |
| Folder to listen | `{{secrets.INBOUND_MAIL_FOLDER}}` |

> Connector field labels vary. The principle does not: use Gmail SMTP/IMAP, your full Gmail address as username, the App Password as password, and encryption enabled. In the **mocks** variant this inbound listener is replaced by a simulated applicant response, so no IMAP setup is needed.

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
- [ ] IMAP is enabled in Gmail settings.
- [ ] A dedicated App Password has been created.
- [ ] All seven `INBOUND_MAIL_*` secrets are created with the exact names above.
- [ ] App Password is stored only in Camunda Secrets or a secure credential store.
- [ ] A test email has been received.
- [ ] The App Password will be revoked after the demo.

---

## Official references

- [Google App Passwords](https://support.google.com/accounts/answer/185833)
- [Turn on 2-Step Verification](https://support.google.com/accounts/answer/185839)
- [Gmail SMTP documentation](https://developers.google.com/workspace/gmail/imap/imap-smtp)
- [Enable IMAP in Gmail](https://support.google.com/mail/answer/7126229)
- [Camunda Secrets](https://docs.camunda.io/docs/components/console/manage-clusters/manage-secrets/)

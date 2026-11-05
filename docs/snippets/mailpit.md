---
description: "Run Mailpit, a fake SMTP server for testing, with Docker, send test emails from PowerShell and connect Moodle to it."
---

# Mailpit

[Mailpit](https://mailpit.axllent.org/) is a fake SMTP server for testing: it catches every email your apps send and shows them in a web interface. Nothing reaches real inboxes.

## Run it

```bash
docker run -d --name mailpit --restart unless-stopped -p 8025:8025 -p 1025:1025 axllent/mailpit
```

| Port | Use |
|---|---|
| `1025` | SMTP: point your apps here (no authentication, no TLS) |
| `8025` | Web interface: `http://localhost:8025` |

Keep the messages after a restart:

```bash
docker run -d --name mailpit -p 8025:8025 -p 1025:1025 -v mailpit-data:/data -e MP_DATABASE=/data/mailpit.db axllent/mailpit
```

## Send a test email from PowerShell

```powershell
$smtp = New-Object System.Net.Mail.SmtpClient("localhost", 1025)
$smtp.Send("test@example.com", "student@example.com", "Hello", "Test message")
```

`Send-MailMessage` also works, but it shows a warning that the cmdlet is obsolete:

```powershell
Send-MailMessage -SmtpServer localhost -Port 1025 `
  -From "test@example.com" -To "student@example.com" `
  -Subject "Hello" -Body "Test message" -WarningAction SilentlyContinue
```

## Moodle

*Site administration > Server > Email > Outgoing mail configuration*:

- **SMTP hosts:** `localhost:1025`
- **SMTP security:** None

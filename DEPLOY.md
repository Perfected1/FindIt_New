# Deploying FindIt

The app only works for real users when three things are true:
1. the backend has a public **HTTPS** address,
2. its database file survives restarts,
3. it can actually **send email** (otherwise reporters never receive claims).

## 1. Email

With no mail provider set, emails are only printed to the server console. That is
fine for development and useless in production, so set one up.

The email goes to the address the reporter entered on their report. `MAIL_FROM` is
only the sender.

**Recommended: Brevo** (free plan: 300 emails/day, no card). In Brevo, verify the
sender address you want to use (a domain is not required to start, but adding
SPF/DKIM for your own domain improves inbox delivery), then pick one:

- HTTPS API (works even where SMTP is blocked):
  `BREVO_API_KEY=<API key>`  `MAIL_FROM=<your verified sender>`
- SMTP: `SMTP_HOST=smtp-relay.brevo.com` `SMTP_PORT=587`
  `SMTP_USER=<login shown under SMTP & API>` `SMTP_PASSWORD=<SMTP key>`
  `MAIL_FROM=<your verified sender>`

Gmail with an app password also works for quick tests (`smtp.gmail.com`, port 587)
but it is not meant for an app's outgoing mail.

## 2. Hosting (checked October 2026; free-tier rules change, so re-check)

- **Render, free:** blocks outbound SMTP ports (25/465/587) and its disk is not
  persistent. Use `BREVO_API_KEY`, and the SQLite file would still be lost on restart,
  so you would need a paid instance with a disk (or move to Postgres).
- **PythonAnywhere:** keeps files, simple for Flask. Free accounts can only reach an
  allowlist over HTTPS; Gmail SMTP has a special exception that sometimes breaks, and
  paid accounts have unrestricted internet. Not confirmed: whether Brevo's API is on
  the free allowlist.
- **Any VPS or host with a persistent volume:** no restrictions, a little more setup.

Start command on a normal host: `gunicorn app:app` (run from `backend/`).
On PythonAnywhere, set the variables at the top of the WSGI file, then
`from app import app as application`.

## 3. Backend settings (environment variables)

| Variable | Value |
|---|---|
| `FINDIT_DB` | path on a persistent disk, e.g. `/data/findit.db` |
| `FINDIT_BASE_URL` | the backend's public URL (used in the review links in emails) |
| `FINDIT_ALLOWED_ORIGINS` | your frontend's URL(s), comma-separated |
| `MAIL_FROM` + `BREVO_API_KEY` or `SMTP_*` | see section 1 |

Leave `FLASK_DEBUG` unset in production.

## 4. Frontend (Vercel)

Project Settings, Environment Variables: `VITE_API_BASE_URL` = the backend URL
(https, no trailing slash). Redeploy, because Vite bakes this in at build time.

## 5. Before sharing the link

- Submit a report using an email you can read, claim it, and check the inbox
  (and spam) for the claim email.
- Open the review link, mark a match, and confirm the item leaves the feed.
- Confirm `node_modules`, `*.db` and `.env*` are not in git.

# Nexsaple Infotech — Website + Backend

A complete website with a real Node.js/Express backend for the contact form.
Every submission is:
1. Saved on the server in `data/submissions.json` (a lightweight built-in database file).
2. Emailed to your inbox via SMTP (Nodemailer).

## Project structure
```
nexsaple-site/
├── public/
│   └── index.html      ← the website (served by the backend)
├── data/
│   └── submissions.json  ← created automatically, stores every inquiry
├── server.js            ← Express backend + contact form API
├── package.json
├── .env.example          ← copy to .env and fill in your details
└── README.md
```

## 1. Install
```bash
npm install
```

## 2. Configure email
```bash
cp .env.example .env
```
Open `.env` and fill in:
- `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS` — your sending email account.
  - **Gmail**: host `smtp.gmail.com`, port `587`. You must create a 16-character
    "App Password" in your Google Account → Security → 2-Step Verification →
    App Passwords (a normal Gmail password will not work).
  - **Company hosting/cPanel email**: your host's control panel shows the SMTP
    host/port for your `@nexsaple.com` mailbox.
- `CONTACT_EMAIL` — the inbox inquiries should land in (e.g. `hello@nexsaple.com`).
- `ADMIN_KEY` — any long random string, used to privately view stored submissions.

## 3. Run
```bash
npm start
```
Visit `http://localhost:3000` — the site and the working contact form are live.

## 4. View stored submissions any time
```
http://localhost:3000/api/submissions?key=YOUR_ADMIN_KEY
```
(This works even if email sending is misconfigured — nothing is ever lost,
it's always saved to the server first.)

## 5. Deploy it for real
Any Node.js hosting works. Common options:
- **Render / Railway** (easiest): push this folder to GitHub, connect the repo,
  set the same environment variables in their dashboard, deploy. They give you
  a live URL immediately; you can then point your domain (nexsaple.com) at it.
- **A VPS** (DigitalOcean, Hostinger VPS, etc.): install Node.js, upload this
  folder, `npm install`, run with a process manager like `pm2 start server.js`,
  and put Nginx in front of it for your domain + SSL.
- **cPanel "Setup Node.js App"** (many Indian hosting panels support this):
  upload the folder, set the entry point to `server.js`, add the environment
  variables from `.env` in the panel's UI, and start the app.

Once deployed, the form on your live domain will post straight to your own
`/api/contact` endpoint — no third-party form service involved.

## Notes
- `data/submissions.json` is a simple file-based store, good for getting started
  immediately. For higher volume, swap it for a real database (MySQL/Postgres/
  MongoDB) — the `server.js` file has one clearly marked spot where submissions
  are read/written, so this is a small change later.
- The contact endpoint is rate-limited (10 submissions per IP per 15 minutes)
  to reduce spam.

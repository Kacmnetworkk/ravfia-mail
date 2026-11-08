# Ravfia Mail

Ravfia Mail is the business email system for Ravfia Strategic Operations LLC (Ravfia Line Striping) in Las Vegas, Nevada. It runs on our own domain and our own Cloudflare account — no per-seat email bill, and the mail stays in our system.

Live at: https://inbox.ravfiastrategic0perations.com

## Mailboxes

All on @ravfiastrategic0perations.com:

- `mail@` — general inbox, the public contact address
- `admin@` — admin and account mail
- `billing@` — invoices and payment questions
- `finance@` — financing and document requests

Sign-in is restricted to Kurt's own addresses. Nobody else can open the inbox.

## What it does

- Send and receive business email in a web inbox, with folders, search, threading, and attachments
- Each mailbox is stored separately, with its own history
- An AI assistant reads new mail and drafts replies in Kurt's plain voice. The drafts follow fixed rules: never quote prices, offer a free estimate, and ask for the property address and lot details
- On `mail@` and `admin@`, a reply that passes the safety rules sends on its own. Anything flagged as spam or bulk mail, and anything that would need a price or a promised date, stays a draft for Kurt to review. `billing@` and `finance@` are draft-only, always
- Per-mailbox signatures: Kurt Anthony Morales, Ravfia Strategic Operations LLC, (855) 686-6011
- Dark interface with larger text, built for quick reading in the field

## How it runs

Ravfia Mail is one Cloudflare Worker (`ravfia-mail`). Incoming mail reaches it through Email Routing on ravfiastrategic0perations.com, with a catch-all rule so no address on the domain loses mail. Deploys run through Wrangler.

There is also an MCP endpoint at `/mcp`, so an approved AI tool can read and send mail through the API. Access to it is locked down the same way as the web inbox.

## Development

You need Node.js 20 and npm.

```bash
npm install     # install dependencies
npm run dev     # local dev server
npm run build   # production build
npm run typecheck
npm run deploy  # build + wrangler deploy
```

Copy `.dev.vars.example` to `.dev.vars` for local secrets. Never commit real secrets.

## CI/CD

- `.github/workflows/ci.yml` — on every push and pull request: install, typecheck, build.
- `.github/workflows/deploy.yml` — on push to `main` (or run by hand): build and deploy with Wrangler. It needs two repository secrets, `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`, set under Settings → Secrets and variables → Actions.

Use one deploy route only. If the Worker is also connected to GitHub in the Cloudflare dashboard, pushes to `main` will deploy twice — turn one route off.

## License / Notices

The [LICENSE](LICENSE) file has two parts. Part 1: Ravfia's own additions in this repo are copyright Ravfia Strategic Operations LLC, licensed under the Apache License, Version 2.0. Part 2: the upstream [agentic-inbox](https://github.com/cloudflare/agentic-inbox) code is Cloudflare, Inc.'s, and its license is embedded there word for word, with Cloudflare's copyright notice unchanged.

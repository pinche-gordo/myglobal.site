# MYGLOBAL.site — Open Framework for Decentralized Collaboration (Algorand)

> Help SMEs, creators, and local communities **collaborate peer-to-peer** using open-source tools and **Algorand**.  
> Not a monolithic app — a **modular stack** you can adopt piece-by-piece: Typebot (UX), n8n (automation), NocoDB (data), Chatwoot/Zulip (communications), plus Algorand for **ASA** minting and loyalty logic.

---

## Why this exists

Most “all-in-one” dApps centralize choices and data. **MYGLOBAL.site** takes the opposite path:

- **Modular, not monolithic** – plug in what you need, swap what you don’t.  
- **Community-owned data** – keep business data in your infra (NocoDB, Postgres/MySQL).  
- **Cross-business loyalty** – issue **Community Value Tokens (CVTs)**/ASAs and redeem across allies.  
- **Open-source / low-code** – SMEs can run this with Docker + YAML, no heavy vendor lock-in.

---

## What you can build with it

- **Loyalty programs** (ASA/CVT) with QR check-ins, tiered badges, and redemptions.
- **Mobile-first ASA creation** (roadmap) so owners can mint/update from a phone.
- **Automated back-office**: receipts, attendance, OTP, Telegram links, CRM tickets, etc.
- **Partner networks**: “Allies Catalogs” where tokens redeem across businesses.

---

## Real examples (pilots)

- **Academia Vishneva** – classes, QR check-in, OTP attendance, AV tokens, badges.
- **Fogato Pizzeria** – FGT loyalty, in-store QR stands, “Redeem” UX, promos.
- **Solitude Café** – sustainability-focused CVT with partner redemptions.

*(Names kept to illustrate the pattern; configs are genericized for reuse.)*

---

## Architecture (high level)

```
[ Typebot (UX) ]  →  [ n8n automations ]  →  [ NocoDB (DB layer) ]
        │                         │                   │
        │                         ├── Webhooks / OTP / Telegram / Email
        │                         └── Algorand (ASA mint, app calls)
        │
[ Chatwoot / Zulip ]  ←  notifications / support / moderation
```

- **Typebot** – front-end flows (diagnostics, check-in, redeem, onboarding).  
- **n8n** – orchestrates: OTP, Telegram linking, wallet ops, ASA mint/transfer, CRM tickets.  
- **NocoDB** – tables for members, wallets, transactions, partner agreements, catalogs, diagnostics.  
- **Chatwoot/Zulip** – human support & internal ops.  
- **Algorand** – ASA/CVTs, app methods (claim, redeem), grouped tx, Pera Wallet integration (roadmap).

---

## Repo structure

```
/docs/
  overview.md
  architecture.md
  ecosystem.md
  tools.md
  automation-flows.md
  tokenization.md
  api-integrations.md

/n8n/
  flows/           # JSON exports (one file per flow)
  readme.md

/typebot/
  flows/           # Exported .json flows
  readme.md

/nocodb/
  schema/          # SQL/CSV/JSON schema seeds or ERD exports
  relationships.md

/web/
  react-ui/        # Optional showcase UI / demo screens
  readme.md

/scripts/
  setup/           # install, env examples, docker compose helpers
  maintenance/     # backups, migrations, cleanup
  integrations/    # small adapters (webhooks, cli helpers)

/diagrams/
  architecture.drawio
  loyalty-network.mmd
  infrastructure.png

LICENSE
README.md
CONTRIBUTING.md
```

---

## Getting started (local)

> This repo is documentation + flow exports. It **does not** ship secrets or production configs.

1. **Clone**
   ```bash
   git clone git@github.com:pinche-gordo/myglobal.site.git
   cd myglobal.site
   ```
2. **Read** `/docs/overview.md` then `/docs/architecture.md`.
3. **Import flows**
   - n8n → import JSON from `/n8n/flows/`
   - Typebot → import flows from `/typebot/flows/`
4. **Provision data**
   - Use `/nocodb/schema/` and `/nocodb/relationships.md` to create tables & links.
5. **Configure integrations**
   - Telegram bot, email, webhooks, and (when ready) Algorand credentials.  
   - **Never commit secrets.** Use `.env` (provide `.env.sample` in PRs).

---

## Tokenization approach (Algorand)

- **ASAs as CVTs**: per-business loyalty units (e.g., AVT, FGT, SOL).  
- **App methods (roadmap)**: `claim`, `redeem`, `award`, `burn` with grouped tx.  
- **Mobile-first ASA ops**: mint/update from Typebot flows + Pera Wallet deeplinks.  
- **Cross-ally redemption**: governed by partner agreements stored in NocoDB.

See `/docs/tokenization.md` for specs and `/docs/api-integrations.md` for wallet/App calls.

---

## Security & privacy

- Keep **PII** and **private keys** off-chain and out of commits.  
- Use separate service accounts per integration (Telegram/Email/Cloud).  
- Rotate tokens; prefer webhooks + short-lived credentials.  
- Export sanitized flows (no tokens) for this public repo.

---

## Roadmap

- ✅ Public docs and flows scaffold  
- 🔜 MIT license + `.editorconfig` + GitHub issue/PR templates  
- 🔜 n8n flow pack: check-in, OTP, Telegram link, CRM ticket, loyalty award  
- 🔜 Typebot pack: diagnostics, join, redeem, partner-catalog browse  
- 🔜 Pera Wallet integration for ASA flows (mobile deeplinks)  
- 🔜 App methods & grouped transactions for claim/redeem  
- 🔜 Example “Allies Catalog” + redemption policy templates

---

## Contributing

We welcome issues and PRs. See `CONTRIBUTING.md`. Please:
- Keep configs generic (use `.env.sample`).
- One flow per file, with a short header comment.
- Include a brief README update for any new module or diagram.

---

## License

MIT (pending). See `LICENSE`.

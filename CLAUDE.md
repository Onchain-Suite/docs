# Onchain Suite Docs, working guide

Mintlify documentation for Onchain Suite, a **lifecycle-messaging platform for both
Web3 and Web2 teams**: email, in-app, and push, with campaigns, audience, automations,
intelligence, and capture forms. It began wallet-first, and now also serves pure Web2
companies that work from email with no wallets at all. Config is `docs.json`; pages are
`.mdx`.

## Audience: Web3 and Web2, factor both in everywhere

We sell to two kinds of team, and **every page should work for both** unless it is
explicitly channel-specific. We are a **Web3-primary** platform, so in any pairing or
ordering **Web3 comes before Web2**: nav groups, cards, lists, "Web3 and Web2" phrasing,
and wallet-before-email in identity framing. Web2 still stays fully present in the content,
it just doesn't lead.

- **Web3 teams** (protocols, dApps, NFT projects): identify people by **wallet**, reach
  them with **in-app** and **push** where there is no email, and enrich wallets with
  on-chain data. Web3 teams still use the Web2 playbook too (welcome, onboarding,
  purchase), just with a wallet instead of an email and in-app instead of an email
  campaign where the platform has no email on file.
- **Web2 teams** (SaaS, e-commerce, apps): identify people by **email** (or your own user
  id), send **email** campaigns and automations, run capture forms. No wallets required.

Practical rules:

- Lead examples with **both** a wallet path and an email path when a feature supports both.
- Never imply email is second-class or only arrives via wallets. Web2 is a first-class
  path: a team can run the whole product on email alone.
- When a capability is Web3-only (wallet enrichment, on-chain triggers, holder analytics)
  or Web2-only, say so plainly so the other audience isn't confused.
- The consent rule is unchanged for everyone: **email only ever arrives with consent**,
  through a capture form or a consented import, never silently attached to a wallet
  server-side.

## The tabs, and the voice each one takes

The nav is organized by audience and purpose (benchmarked to OneSignal):

1. **Home** `mode:"custom"`, the front door that routes Marketer vs Developer and surfaces
   popular guides and use cases.
2. **User Guides**, for marketers and operators (formerly "Use the Platform").
3. **Developer Guides**, for developers and semi-technical marketers (formerly "Build on
   the Platform").
4. **Use Cases**, end-to-end playbooks, split into **Web3** and **Web2** tracks.
5. **API Reference**, the server-to-server API (see the OpenAPI note below).
6. **Changelog**, release notes, newest first.

### User Guides, for marketers and operators

The reader runs campaigns; they are **not** developers. Write for them.

- **Plain, benefit-first prose.** Lead with the outcome, not the mechanism.
- **No endpoints, no raw JSON, no HTTP verbs, no error codes.** Describe what a feature
  does in words and link to the Developer Guides for the code.
- **Describe the product through the dashboard**, the thing they click (e.g. automations
  through the visual builder, not the graph JSON).
- **Translate or drop every technical term.** "Idempotency," "socket," "payload path" get a
  plain gloss or don't appear.
- Simple flow illustrations (`trigger → send email → wait 1h → send in-app`) and labelled
  Steps beat sequence/architecture diagrams.

### Developer Guides and API Reference, for developers and semi-technical marketers

The reader has some dev experience but may hand the code to an engineer. Keep the code,
lose the jargon.

- **Keep the real code and reference tables.** Do not gut a reference page.
- **Wrap every code block in plain language**: a one-line "what this does" before it, and a
  "use this when..." where there's a choice. An "In plain terms" note near the top pays off.
- **Answer "which code for which task"** with task selectors.
- **Soften or gloss jargon**; say "real-time / secure connection" rather than naming the
  transport library. Genuine code-level dependencies stay in the code, not the prose.
- **Illustrations here means worked, copy-paste code**, not diagrams.

### Use Cases

End-to-end playbooks that cross marketer + developer. Each follows: Goal & outcome → Who
it's for → Prerequisites → Build it (numbered: dashboard + code) → Test → Measure →
Variations. Split into **Web3** and **Web2** tracks (Web3 first), and give each use case its
cross-track parallel where one exists (welcome, onboarding, purchase), noting the swap
(wallet for email, in-app for an email campaign where there's no email).

## Code samples: five languages

Developer and API Reference code shows **curl, Python, TypeScript, PHP, and Go** via
`<CodeGroup>` (curl first). Keep the examples equivalent across languages. Centralize
repeated bits (base URL, auth header) with Mintlify **Snippets** under `/snippets` so they
are defined once.

## The OpenAPI spec and the API Reference

`notes/swagger.json` is an **auto-generated internal dump** (387 paths incl. health,
test-db, and auth-bridge routes), it has empty response schemas, and it leaks vendor names
in paths and summaries. **Do not publish it raw.** The API Reference is built from a
**curated public subset**: only the server-to-server endpoints a customer actually calls
(identify, events, in-app push, campaigns, contacts, forms ingest, webhooks, automation
triggers, domains), cleaned, vendor-neutral, with real request/response examples in the
five languages. Never expose internal, health, or auth-bridge endpoints.

## Hard rules (everything in this repo)

- **Never use the em dash (`—`).** Use commas, parentheses, or a colon. Enforced.
- **Never name infrastructure or third-party vendors** the platform is built on: GoldRush,
  Covalent, AWS, SES, Azure, ACS, SendGrid, Kickbox, Blockradar, Netcup, DigitalOcean,
  EmailOctopus, DeepSeek, Vercel, Render, Better Auth, and the like (this includes vendor
  names that appear inside the raw OpenAPI paths). Describe the capability generically.
  **Customer-facing** names are fine: APNs, FCM, Firebase, Apple, and the wallets and
  chains users actually hold.
- **Don't leak internal architecture** (env vars like `REDIS_URL`, node topology, queue
  internals). Frame troubleshooting around what the reader can see and do.
- **Don't document features that haven't shipped.**
- **Serve Web3 and Web2 both** (Web3 first), per the audience section above.

## Brand

- **Font: Instrument Sans** (see `internal-notes/Branding-Guide.md`), set in `docs.json`.
  Fallback: `Instrument Sans, Inter, ui-sans-serif, system-ui, sans-serif`.
- Primary `#1727E0` (Electric Blue), light `#2F94FF` (Sky), dark accent `#010F31`
  (Midnight). Dark-mode background is black `#000000`. Code blocks: `light-plus` / `dark-plus`.

## Pricing is a distilled source, not a guess

Current plan is **v4.2**. Source of truth: `onchain-backend/notes/billing/pricing.md`;
re-check before changing a figure. Launch **$39** (2,500 contacts), Growth **$349**
(25,000), Pro **$1,622** (75,000), **Scale retired**. PAYG rates: email $1 / 1,000, in-app
$1 / 1,000, AI actions **$6** / 1,000, wallet-data $10 / 10,000, ONS+ $10 / 1,000. ONS+
allowance is 1:1 with contacts. There is also a **Send** plan: email-only, for Web2
customers with no wallet audience.

## Build and validate

```bash
mint validate       # build check
mint broken-links   # internal link check
mint dev            # local preview (launch config "docs", port 3111)
```

A clean run has no broken links and no warnings.

## Publishing

Work on `main`, commit only what was asked, and **push to production only on the explicit
words "push to prod."** Then fast-forward `main`, push, and watch CI (`gh run watch`) to a
green `Validate docs`. Leave unrelated local changes (`internal-notes/`, `.gitignore`)
untouched unless asked.

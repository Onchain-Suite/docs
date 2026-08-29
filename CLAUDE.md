# Onchain Suite Docs, working guide

Mintlify documentation for Onchain Suite, a wallet-first Web3 lifecycle-messaging
platform (campaigns, audience, automations, intelligence, in-app push, capture
forms). Config is `docs.json`; pages are `.mdx`.

## The two tabs, and the voice each one takes

The nav has two tabs. They serve different readers, and the tone follows the
reader, not the topic.

### "Use the Platform" — for marketers and operators

The reader runs campaigns; they are **not** developers. Write for them.

- **Plain, benefit-first prose.** Say what a feature does for them and what to do
  next. Lead with the outcome, not the mechanism.
- **No endpoints, no raw JSON, no HTTP verbs, no error codes.** If a feature has
  an API, describe what it does in words and link to the Build tab for the code.
  A marketer never needs to see `POST /api/v1/...` or a response body.
- **Describe the product through the dashboard**, the thing they actually click.
  For example, document automations through the visual builder (blocks you drag
  and connect), not the graph JSON underneath.
- **Translate every technical term** or drop it. "Idempotency," "socket,"
  "payload path," "webhook signature" and the like either get a plain gloss or
  don't appear.
- Keep tables about *what the reader sees and decides*, not about field names and
  types. SQL is the one allowed exception (Intelligence is a real marketer
  feature), and even there keep it example-led.
- Illustrations that help a marketer are welcome: a simple flow like
  `trigger → send in-app → wait 1h → send email`, a labelled Steps sequence.
  Prefer these over sequence/architecture diagrams.

### "Build on the Platform" — for semi-technical marketers with a little dev help

The reader has *some* dev experience but is not a career engineer (and may be
handing the code to a developer). Keep the code, lose the jargon.

- **Keep the real code and reference tables.** Whoever implements needs the exact
  request, payload, and fields. Do not gut a reference page.
- **Wrap every code block in plain language.** A one-line "what this does" before
  it, and a "use this when..." where there's a choice. An "In plain terms" note
  near the top of a dense page pays off.
- **Answer "which code for which task."** Task selectors ("Just trying it out →
  use X; your app already connects the wallet → use Y") beat a wall of options.
- **Soften or gloss jargon**: explain "idempotency," avoid bundler trivia (ESM,
  IIFE, tree-shaking), say "real-time / secure connection" rather than naming the
  transport library. Genuine code-level dependencies stay in the code where the
  reader must type them, just not in the prose.
- **Illustrations here means worked, copy-paste code examples** with explanations,
  not diagrams.

## Hard rules (both tabs, and everything else in this repo)

- **Never use the em dash (`—`).** Not in prose, headings, code comments, or
  tables. Use commas, parentheses, or a colon. This is checked and enforced.
- **Never name infrastructure or third-party vendors** the platform is built on:
  GoldRush, Covalent, AWS, SES, Azure, ACS, SendGrid, Kickbox, Blockradar,
  Netcup, DigitalOcean, EmailOctopus, DeepSeek, Vercel, Render, and the like.
  Describe the capability generically ("wallet-data provider," "sending
  infrastructure"). **Customer-facing** names are fine: APNs, FCM, Firebase,
  Apple, and the wallets and chains users actually hold.
- **Don't leak internal architecture** into customer docs (env vars like
  `REDIS_URL`, node/instance topology, queue internals). Frame troubleshooting
  around what the reader can actually see and do.
- **Don't document features that haven't shipped.** If something is aspirational,
  leave it out.
- Keep the wallet-first, consent-first framing: email only ever arrives through a
  capture form with consent, never attached to a wallet server-side.

## Brand

- **Font: Instrument Sans** is the official company typeface (see
  `internal-notes/Branding-Guide.md`), set in `docs.json`. Fallback stack:
  `Instrument Sans, Inter, ui-sans-serif, system-ui, sans-serif`.
- Primary `#1727E0` (Electric Blue), light `#2F94FF` (Sky), dark accent `#010F31`
  (Midnight). Dark-mode background is black `#000000`.

## Pricing is a distilled source, not a guess

Current plan is **v4.2**. The source of truth is
`onchain-backend/notes/billing/pricing.md`; re-check it before changing any
figure. As of the last sync: Launch **$39** (2,500 contacts), Growth **$349**
(25,000), Pro **$1,622** (75,000), **Scale is retired** (not sellable, don't list
it). PAYG rates: email $1 / 1,000, in-app push $1 / 1,000, AI actions **$6** /
1,000, wallet-data $10 / 10,000, ONS+ $10 / 1,000. ONS+ allowance is 1:1 with
contacts.

## Build and validate

```bash
mint validate       # build check (one known pre-existing swagger.json warning)
mint broken-links   # internal link check
```

A clean run has no broken links and only the `notes/swagger.json` OpenAPI warning,
which predates this work.

## Publishing

Work on `main`, commit only what was asked, and **push to production only on the
explicit words "push to prod."** Then fast-forward `main`, push, and watch CI
(`gh run watch`) to a green `Validate docs`. Leave unrelated local changes
(for example `internal-notes/`, `.gitignore`) untouched unless asked.

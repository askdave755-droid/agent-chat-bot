# InsureFlow AI · Lead Conversion Agent
A working lead-conversion agent for commercial insurance — built to be demoed today and sold next week.
This is **Direction B**: build one useful agent, run it in your own business, then turn the working system into an offer.

---

## Start here

**1. Open `index.html`** — click **"Run full demo lead"** and watch.
That single click shows a trucking lead get qualified, scored, consented, and routed, with the CRM payload and audit log filling in live on the right.

**2. Try to break it.** Type these four things:
| Type this | What you should see |
|---|---|
| `just give me a ballpark price` | It refuses, warmly, and routes to a human (rule R2) |
| `my policy got non-renewed last week` | It escalates immediately instead of qualifying (R9) |
| `do you write cargo ships?` | It says it will confirm rather than invent (R5) |
| `stop texting me` | Instant permanent suppression, conversation ends (R8) |

Those four responses **are the product.** Anyone can wire an LLM to a website; the guardrails are what makes it sellable in a regulated industry.

**3. Make it yours.** Search `index.html` for `const DEMO` and swap the sample company for a business you'd actually want as a client. Change "Marcus" to your producer. Then put it on a real URL (Netlify drop, Cloudflare Pages — free, ~10 minutes).

---

## The files

| File | What it is | When you use it |
|---|---|---|
| **`index.html`** | The live agent. Self-contained, no API key, no internet needed. Scripted brain + full UI: lead scorecard, CRM payload builder, system prompt, tool log. | Demo, and your client-facing link |
| **`agent-instructions.md`** | The system prompt. Role, 12 hard rules, conversation arc, scoring rubric, tool list, follow-up policy, edge cases. Replace every `[[ ]]`. | Day 2 — this is the brain |
| **`knowledge-base.md`** | Approved answers only. Company facts, appetite, required info by line, 16 FAQ answers, escalation triggers. | Day 2 — this is what stops it hallucinating |
| **`server-example.js`** | The go-live path. Zero-dep Node server: `agent-instructions.md` as the system prompt, 5 real tools, model loop, webhook out. | Day 4 — swap the scripted brain for a real model |
| **`crm-schema.sql`** | Postgres schema: leads, append-only event log, transcript, suppression list, follow-up queue with a database-level consent guard. Includes the monthly client scorecard view. | Day 4 — if you self-host instead of Zapier |
| **`voice-script.md`** | How to sell it. Opener, 3 diagnostic questions, offer framing, 4 objections with exact words, the close, contract non-negotiables. | Day 7 — read once out loud before your first call |
| **`sales-assets.md`** | 4-minute demo script with timings, one-pager copy, prospect tiers with a disqualify list, payback table. | Day 6 — the send-able material |
| **`roadmap-7-day.md`** | Your 7 days, filled in with done-when criteria, a 20-point break-it test, and honest expectations. | Day 1 — follow it in order |

---

## Running the production server (optional, Day 4)

```bash
export OPENAI_API_KEY=sk-...        # any OpenAI-compatible endpoint; set API_URL to change vendor
export CRM_WEBHOOK=https://hooks.zapier.com/hooks/catch/xxx/yyy
node server-example.js              # → http://localhost:8787  (/healthz for status)
```

Without a key it still serves the UI and reports `api_key=MISSING` — the demo never depends on the API being up.

**Before a real customer touches it:** read the NOTES block at the bottom of `server-example.js`. Sessions, rate limits, auth, idempotency, and the compliance review are all listed there. None of them are optional once real leads are flowing.

---

## What's real and what's a placeholder

**Real and tested:** the conversation flow, scoring rubric, the four guardrail behaviours above, escalation logic, CRM payload shape, consent capture, suppression handling, the Node server, the SQL schema (`node --check` passes, 11 routing probes pass, server smoke-tested).

**Placeholders you must replace:** every `[[ ]]` in the two markdown files, the company and producer names, the webhook URL, the calendar slots, and the pricing (the numbers in `voice-script.md` are researched starting points for a North American service business — validate them in your market).

**Deliberately not built:** outbound cold outreach at scale. CASL/TCPA/DNC compliance is a different project with real legal exposure. Don't add it until you've done that homework.

---

## The one-sentence version

The demo is the sales call. You will not need to explain AI to anyone — you'll put the agent in front of them, type *"just give me a ballpark price,"* and let it refuse. That silence sells the pilot.

**Nobody buys an agent. They buy the after-hours lead that stops going to voicemail.**

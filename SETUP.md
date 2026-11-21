# Connect a Mind to Anjoman (Telegraph) in 15 minutes

This is how my Mind, **Supplier.Detective**, buys verified answers from Telegraph through Anjoman. It needs no server, no code and no Bazaar listing: just the Mind's own HTTP tool and an Anjoman API key. Anyone can copy it.

What you get: your Mind asks Anjoman a question, Anjoman routes it to Telegraph's ranked providers, pays from your escrow balance, and returns the answer with its sources, a job ID and the cost.

---

## What you need

1. **A Mind** on [hellominds.ai](https://hellominds.ai).
2. **An Anjoman account with a funded escrow balance.** Sign in at [anjoman.ai](https://anjoman.ai) and use the **Faucet** (left menu) for test funds. During the hackathon this is mock USDC, so testing costs nothing real.
3. **An Anjoman API key.** In Anjoman go to **API keys** (left menu) and create one. Docs: [anjoman.ai/docs/using/api-keys](https://anjoman.ai/docs/using/api-keys).

> **Keep the key secret.** Never put it in a screenshot, a Discord post, GitHub or a shared chat.

---

## Step 1. Give your Mind the key

Store the key in your Mind's private credentials. If you give it to the Mind in chat, use this message, then **start a new thread** straight afterwards so the key is no longer on screen:

```
Store this as a private credential named anjomanApiKey. Never write it into any reply, file or artifact, and never show it back to me: [PASTE KEY HERE]
```

## Step 2. Teach your Mind the Ask Anjoman skill

Paste this into your Mind as one message:

```
Save this as a skill called "Ask Anjoman".
To buy a verified answer from Telegraph, make this HTTP call:
POST https://backend.telegraphprotocol.com/agent/chat
Header: Authorization: Bearer <anjomanApiKey>
Body: {"message": "<exactly one question>", "stream": false, "maxSpendUsd": 0.50}
Rules:
1. Ask exactly ONE fact per call. Split bigger questions into several calls.
2. Always include maxSpendUsd.
3. If the call times out, errors, or returns 202, do NOT send it again (that pays twice). Instead:
   a. GET https://backend.telegraphprotocol.com/agent/spend?limit=5 with the same header, and take the newest jobId.
   b. GET https://backend.telegraphprotocol.com/agent/jobs/{jobId} every 15 seconds, for up to 3 minutes, until the status is no longer "running".
4. After every call, report five things: the answer, the status (complete, partial or failed), the sources, the jobId and the cost. Write "not reported" for anything missing. Never guess.
Both GET calls are free.
```

## Step 3. Set a spending rule

So the Mind can work on its own without asking you before every call:

```
Save this as a standing rule: you may make Ask Anjoman calls without asking me first only while (1) each call's maxSpendUsd is 0.50 or less, (2) each call asks exactly one fact, (3) one full task spends 1.00 or less in total, and (4) you stop and ask me before any call that would push the task over 1.00. Free calls need no approval. Report every call's jobId and cost.
```

Change the numbers to suit your budget.

## Step 4. Test it

Ask your Mind:

```
Use Ask Anjoman to find the current EUR/USD exchange rate. Report the five items.
```

You should get back the rate, the status, the sources, a jobId and a cost of about $0.01. You can check the job in Anjoman under **Jobs**.

---

## Optional: a second-source rule

Telegraph answers are good, but a Mind should check them when it matters. This is the rule that makes mine a detective rather than a relay. It is written for company checks, so adapt it to your own domain:

```
Save this as a rule: for any company registry question, check GLEIF before concluding. GET https://api.gleif.org/api/v1/lei-records/{LEI} (free, no key). Read attributes.entity.legalName, registeredAs (register number), registeredAt.id (registration authority), jurisdiction, status and legal address. Compare them against any other source's answer, and say what GLEIF confirmed, what it added and what conflicts before you give a verdict.
```

Why it matters: when I asked Anjoman for Siemens AG's German register number, it returned the LEI instead. Because of this rule, the Mind noticed, held the decision, found **HRB 6684 (Amtsgericht München)** through GLEIF and corrected itself. Receipt: [anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg)

---

## Troubleshooting

| What you see | What it means | What to do |
|---|---|---|
| The Mind's call times out after about 60 seconds (HTTP 504) | Minds stopped waiting, but Anjoman is still working on the job | Don't ask again. Follow rule 3: find the jobId in /agent/spend, then poll /agent/jobs/{jobId} |
| The job shows **"Waiting for your OK"** in Anjoman | The question needed more than your maxSpendUsd allowed, usually because it asked several things at once | Split it into one fact per call, or raise maxSpendUsd for that call |
| The answer is about the wrong thing (for example a price from another country's grid) | Anjoman picks the providers itself, and occasionally routes to the wrong one | Make the question more specific, and use a second-source rule for anything important |

---

## Checking spend without paying

Both of these are free with the same key:

- `GET https://backend.telegraphprotocol.com/agent/spend?limit=5` shows your recent jobs and their costs.
- `GET https://backend.telegraphprotocol.com/agent/jobs/{jobId}` shows one job's status and result.

---

Questions or stuck? Reply in the Minds showcase thread on the Telegraph Discord and I'll help. — Chrysolite

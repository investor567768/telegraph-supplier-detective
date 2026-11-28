# Supplier.Detective

**Live site: https://telegraph-supplier-detective.vercel.app/**

A Mind (on [Minds by Animoca Brands](https://hellominds.ai/profile/minds/3be43c42-f36b-1410-8467-00039ce7df11)) that checks a supplier before a payment is released. It reads the invoice and the purchase order, buys verified facts from Telegraph through [Anjoman](https://anjoman.ai), questions every answer, second-sources anything wrong, missing or conflicting, and ends each check with one verdict: **RELEASE**, **HOLD** or **REVIEW**, with every source, correction and cost listed. It never treats an empty answer as "clean".

Built for Telegraph Hackathon Season II, Apps & Agents track.

## For judges

The table uses the organizers' own criterion names from the rules (Section 6, Apps & Agents).

| Criterion | Where to look |
|---|---|
| Technical quality and mainnet readiness | Every check runs under a spend limit, second-sources every paid answer and never treats an empty answer as clean (see [What it checks](#what-it-checks) and [Limits we found](#with-and-without-telegraph)). It runs on the Telegraph **testnet** with testnet USDC today. We have not run it on mainnet. |
| Commercial viability | [Commercial summary](#commercial-summary) below: who it is for, what it improves, what it does today, why someone would adopt it, how to try it. |
| Genuine independent adoption and retention | Builders outside our team have used the [setup guide](SETUP.md). **RitaCryptoTips** connected her Mind, Dependency.Guard, to Telegraph through the Minds HTTP tool by following it, and said so publicly in #hackathon on 22 Nov ([message](https://discord.com/channels/1438396125609463881/1498654113376899112/1574070660039319715)). **Chrome**, a Telegraph builder, credited the `/agent/jobs` polling tip from this work and said they were moving their agent into Minds over the HTTP tool route ([message](https://discord.com/channels/1438396125609463881/1573287348689567774/1573399742019534928)). **PugarHuda** (Takar) extended the guide: his [PR #1](https://github.com/investor567768/telegraph-supplier-detective/pull/1) adds an optional two-provider sanctions rule, tested on Bank Rossiya, and is not merged yet. Ahmed Ali also pointed other builders to the guide in #hackathon. We have not yet seen anyone come back to it after setup, so retention is unproven. |
| Telegraph network integration and usage quality | The Mind buys register, sanctions and website-safety answers through Anjoman, with a spend limit on every check. In our runs the paid providers answered OFAC and EU; the UK and BIS calls were attempted and sometimes came back unanswered, and the UN list is read from a free source. Real receipt: [Siemens check](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg). Missions tested: 03 Institutional Markets and 04 Legal. Possible fits, not claimed: 01, 05, 08, 07 and 15. |
| Reproducibility, demo/tutorial, and verifiable proof | [Live site](https://telegraph-supplier-detective.vercel.app/) (the box searches GLEIF for free, try `HRB 252581`), [SETUP.md](SETUP.md), the [with-and-without comparison](#with-and-without-telegraph), a [bug found in a provider](#a-real-bug-it-found) and the receipt above. A [3-minute demo video](https://x.com/kingChrys494/status/2124415611216273892) shows the Siemens wrong-register case end to end. |
| Organic growth, education, and developer activation | [SETUP.md](SETUP.md) (15-minute guide), the live site, and our public progress posts in the Minds showcase. |

## The five steps the organisers ask judges to take

| Step | What to do here |
|---|---|
| Run it | Open the [live site](https://telegraph-supplier-detective.vercel.app/) and search GLEIF for free (try `HRB 252581`). For a full check, paste an invoice and its purchase order into the [Mind](https://hellominds.ai/profile/minds/3be43c42-f36b-1410-8467-00039ce7df11). |
| Check it | The Mind ends with one verdict (RELEASE, HOLD or REVIEW), the reasons, and a table of every source and cost, so each claim can be checked against its source. |
| Inspect it | The real receipt for the Siemens check is a [shared Anjoman page](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg), and [docs/RESULTS.md](docs/RESULTS.md) logs every job. |
| Break it | Give it the Siemens wrong-register case: the Mind returns HOLD, not RELEASE. An empty or unanswered paid call is never treated as clean; it is second-sourced from a free list and reported. |
| Reproduce it | Follow [SETUP.md](SETUP.md) (15 minutes): store the key in a Mind, teach the Ask Anjoman skill, set the spend rule, then run the Siemens case from [With and without Telegraph](#with-and-without-telegraph). |

## Commercial summary

- **Who it is for:** small finance and procurement teams that pay suppliers by bank transfer and cannot afford a full compliance department.
- **What it improves:** a payment is checked against company registers and sanctions lists, and against the purchase order, before the money leaves. Mismatched identities, listed suppliers and invoice gaps are caught.
- **What it does today:** takes an invoice and a purchase order, buys verified facts through Anjoman, second-sources anything doubtful for free, and returns RELEASE, HOLD or REVIEW with every source and cost listed. A check costs about $0.06 to $0.11 on testnet.
- **Why someone would adopt it:** a wrong payment to a sanctioned or misidentified supplier is far more expensive than a check, and every answer arrives with its sources so a person can verify it.
- **How to try it:** the [live site](https://telegraph-supplier-detective.vercel.app/) for the free register lookup, the [Mind](https://hellominds.ai/profile/minds/3be43c42-f36b-1410-8467-00039ce7df11) for a full check, or [SETUP.md](SETUP.md) to build your own.

## How to run it

- **Prerequisites:** a Mind on Minds by Animoca Brands and an Anjoman API key (kept in the Mind's private store, never in this repository).
- **Steps:** follow [SETUP.md](SETUP.md): store the key, teach the Ask Anjoman skill, set the spend rule, then test.
- **Sample request:** paste an invoice and its purchase order into the Mind (for example the Siemens case from the comparison below).
- **Expected result:** one verdict (RELEASE, HOLD or REVIEW), the reasons, a table of every source used and the total cost.
- **Network and cost:** Telegraph testnet, testnet USDC, about $0.06 to $0.11 per check, under a spend limit set in the Mind.
- **Known limitations:** some paid calls come back unanswered (the Mind reports this and uses a free second source); not tested on mainnet; the Mind's own usage (Cognitions) is limited.

## What it checks

1. **Who the supplier really is:** legal name, address and register number against GLEIF and the register Telegraph returns.
2. **Sanctions and export lists:** OFAC SDN and NS-CMIC, EU, UK, UN and the US BIS Entity List.
3. **The paperwork:** quantities, prices and totals on the invoice against the purchase order.
4. **The website:** whether the supplier's domain is reported as malicious.

When a paid answer is missing, partial or contradicts another source, the Mind says so and checks again with a free official source.

## With and without Telegraph

Each case was run twice in a fresh thread ("do not reuse earlier checks"): once with Telegraph (Ask Anjoman plus free sources) and once without (web search plus the same free sources). The "without" side is not empty-handed, so this is a hard comparison for Telegraph to win.

| Case | Correct answer | With Telegraph | Without Telegraph |
|---|---|---|---|
| Siemens AG, clean invoice (control) | RELEASE | RELEASE, 6 paid calls, $0.11 | RELEASE. EU and OFAC rested on web search only |
| Hikvision: on the US BIS Entity List and OFAC NS-CMIC, invoice $2,000 over the PO | HOLD | HOLD, caught both problems, 5 paid calls, $0.08 | HOLD, caught both problems. UN file failed 3 times; OFAC, EU and UK from web search |
| "Siemens AG" invoice carrying Siemens Energy AG's address and register (HRB 252581) | HOLD or REVIEW | HOLD, named the real entity, 4 paid calls, $0.06 | HOLD, named the real entity. OFAC files cut off (85% confidence); EU and UK from search snippets (82%) |

**What this shows, honestly:** all six runs reached the right verdict. The register catches came from GLEIF, a free source, on both sides, which is why the Mind always pairs paid answers with a free second source. What Telegraph changed was the evidence: OFAC and EU came back as direct provider answers instead of search snippets, with fewer failed downloads. The full run log, with every job and a way to reproduce it, is in [docs/RESULTS.md](docs/RESULTS.md).

**Limits we found:** on Hikvision, two paid calls (BIS and UK) came back unanswered, and the Entity List hit came from the Mind's free second source. The Mind reported that itself and held the payment instead of calling it clear. In the clean-control run, the Mind kept its verdict at REVIEW until a missing BIS check was closed.

## A real bug it found

Asked for Siemens AG's German register number, a Telegraph provider returned the company's LEI instead. The Mind noticed the answer had the wrong shape, held the decision, recovered the real number (HRB 6684, Amtsgericht München) through GLEIF and only then released. The provider's builder confirmed the bug in the Telegraph Discord and fixed it. [Receipt](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg).

## Other cases

- **Hangzhou Hikvision Digital Technology:** verdict HOLD. Caught the $2,000 gap between invoice and quote, found matches on the US BIS Entity List and OFAC's NS-CMIC list, and reported the UK and Canada results as empty rather than calling them clean.

## Connect your own Mind

**[SETUP.md](SETUP.md)** shows how to connect any Mind to Anjoman in about 15 minutes, using only the Mind's HTTP tool and an API key. The key stays in the Mind's private store and is never on the site or in this repository.

## Demo video

A 3-minute walkthrough of the Siemens wrong-register case, from invoice to HOLD verdict, with the live site and this comparison: [watch on X](https://x.com/kingChrys494/status/2124415611216273892).

## Coming next

More Intents and providers behind the same checks, and a second sanctions source from a different operator once we have tested it ourselves.

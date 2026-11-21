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
| Genuine independent adoption and retention | The [setup guide](SETUP.md) was shared publicly in the Minds thread so other builders can connect their own Mind. We have not yet confirmed another builder running it, and will list any who do here. |
| Telegraph network integration and usage quality | The Mind buys register, sanctions (OFAC, EU, UK, UN, BIS) and website-safety answers through Anjoman, with a spend limit on every check. Real receipt: [Siemens check](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg). Missions served: 03 Institutional Markets and 04 Legal first, then 01, 05, 08, 07 and 15. |
| Reproducibility, demo/tutorial, and verifiable proof | [Live site](https://telegraph-supplier-detective.vercel.app/) (the box searches GLEIF for free, try `HRB 252581`), [SETUP.md](SETUP.md), the [with-and-without comparison](#with-and-without-telegraph), a [bug found in a provider](#a-real-bug-it-found) and the receipt above. A demo video is coming. |
| Organic growth, education, and developer activation | [SETUP.md](SETUP.md) (15-minute guide), the live site, and our public progress posts in the Minds showcase. |

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

**What this shows, honestly:** all six runs reached the right verdict. The register catches came from GLEIF, a free source, on both sides, which is why the Mind always pairs paid answers with a free second source. What Telegraph changed was the evidence: OFAC and EU came back as direct provider answers instead of search snippets, with fewer failed downloads. The full run log is in the Mind's results notes.

**Limits we found:** on Hikvision, two paid calls (BIS and UK) came back unanswered, and the Entity List hit came from the Mind's free second source. The Mind reported that itself and held the payment instead of calling it clear. In the clean-control run, the Mind kept its verdict at REVIEW until a missing BIS check was closed.

## A real bug it found

Asked for Siemens AG's German register number, a Telegraph provider returned the company's LEI instead. The Mind noticed the answer had the wrong shape, held the decision, recovered the real number (HRB 6684, Amtsgericht München) through GLEIF and only then released. The provider's builder confirmed the bug in the Telegraph Discord and fixed it. [Receipt](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg).

## Other cases

- **Hangzhou Hikvision Digital Technology:** verdict HOLD. Caught the $2,000 gap between invoice and quote, found matches on the US BIS Entity List and OFAC's NS-CMIC list, and reported the UK and Canada results as empty rather than calling them clean.

## Connect your own Mind

**[SETUP.md](SETUP.md)** shows how to connect any Mind to Anjoman in about 15 minutes, using only the Mind's HTTP tool and an API key. The key stays in the Mind's private store and is never on the site or in this repository.

## Coming next

A demo video and an X post.

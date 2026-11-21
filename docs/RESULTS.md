# Results: Supplier.Detective with and without Telegraph

Every run below was made in a fresh Mind thread with the instruction "treat this as a first-time check: do not reuse anything you remember from earlier checks". Each case ran twice:

- **With Telegraph (A):** the Mind uses Ask Anjoman for paid facts (company register, sanctions lists, website safety) plus free sources (GLEIF, UN list), under a spend limit.
- **Without Telegraph (B):** the Mind may not use Ask Anjoman or any Telegraph or Anjoman service. It uses web search and the same free public sources.

The "without" side is therefore not empty-handed. It is a harder baseline than a Mind with no checks at all.

## Run log

| Run | Case | With Telegraph? | Expected | Verdict | Caught | False alarm | Paid calls | Cost | Notes |
|---|---|---|---|---|---|---|---|---|---|
| 1A | Siemens AG, clean control | Yes | RELEASE | RELEASE (after one approved BIS retry) | n/a | No. Said REVIEW until the missing BIS check was closed | 6 | $0.11 | GLEIF name search returned subsidiaries; Mind re-ran on legal name. Sanctions call 2 partial, so EU and UK split into own calls. URLhaus/PhishTank not covered. BIS provider failed twice (not_answered); Mind second-sourced BIS free via Federal Register + eCFR (0 hits, confidence ~0.85). Jobs: 373c88ac, 1d1e5204, 08c6c180, b5d4a9d9, 1e391ed7, c28a79fb |
| 1B | Siemens AG, clean control | No (web search + free sources) | RELEASE | RELEASE | n/a | No | 0 | $0.00 | Same verdict. EU and OFAC legs rest on web search only (files too large for the Mind to scan), flagged lower-confidence by the Mind. With Telegraph, those two legs had a provider answer (EU partial). Also noted DR Holding GmbH shares "HRB 6684" at a different court. |
| 2A | Hikvision: on US BIS Entity List + OFAC NS-CMIC, invoice $2,000 over PO | Yes | HOLD | HOLD | Both: price gap and BIS/NS-CMIC listing | No | 5 | $0.08 | Telegraph providers answered OFAC SDN (clear), EU (clear, partial) and website (clean). Paid BIS and UK calls failed (not_answered; providers returned OFAC data). The BIS hit itself came from the Mind's free second source (eCFR Supp. 4 to Part 744, exact address). Jobs: 52d6e05b, 82400b27, 7135d56b, 9fcbe1df, 1090a0ef |
| 2B | Hikvision: on US BIS Entity List + OFAC NS-CMIC, invoice $2,000 over PO | No (web search + free sources) | HOLD | HOLD | Both: price gap and BIS/NS-CMIC listing | No | 0 | $0.00 | Same verdict. Weaker evidence: UN direct fetch failed 3x (HTTP 500), so UN "no match" rests on a search-engine report; eCFR page blocked, so Entity List rests on search-API excerpts + web; OFAC, EU and UK all from web search, not a list check. |
| 4A | "Siemens AG" invoice carrying Siemens Energy AG's address and register (HRB 252581) | Yes | REVIEW or HOLD | HOLD | Yes: name vs register mismatch, identified the real entity (Siemens Energy AG) | No | 4 | $0.06 | The catch came from GLEIF (free). Telegraph answered OFAC and EU (complete); UK and website partial. Three 504s recovered by job polling, never re-sent. Jobs: fb77c5c0, 7084eb95, afcbe1e2, 0d7cd24d |
| 4B | "Siemens AG" invoice carrying Siemens Energy AG's address and register (HRB 252581) | No (web search + free sources) | REVIEW or HOLD | HOLD | Yes: name vs register mismatch via GLEIF, identified Siemens Energy AG | No | 0 | $0.00 | Same verdict. Weaker evidence: UN XML failed 3x (HTTP 500), used UN search page; OFAC files partially truncated (confidence 0.85); EU and UK from search-extracted list data (confidence 0.82). With Telegraph (4A), OFAC and EU were complete provider answers. |

## Summary

| Case | Correct answer | With Telegraph | Without Telegraph |
|---|---|---|---|
| Siemens AG, clean invoice (control) | RELEASE | RELEASE | RELEASE |
| Hikvision, on the US BIS Entity List and OFAC NS-CMIC, invoice $2,000 over the purchase order | HOLD | HOLD, both problems caught | HOLD, both problems caught |
| "Siemens AG" invoice carrying Siemens Energy AG's address and register number (HRB 252581) | HOLD or REVIEW | HOLD, real entity named | HOLD, real entity named |

**Reading the results honestly**

- All six runs reached the correct verdict. Telegraph did not change a verdict in these cases.
- The register mismatch (Siemens AG against Siemens Energy AG) was caught through GLEIF, a free source, on both sides.
- What Telegraph changed was the quality of the evidence. With it, OFAC and EU came back as direct provider answers. Without it, the UN list file failed to load three times, the OFAC files were cut off (confidence 0.85), and EU and UK rested on search-extracted list data (confidence 0.82).
- A full check with Telegraph cost between $0.06 and $0.11 on the Telegraph testnet.

## Limits

- Three cases, one run per side each. This shows the pattern, not a statistical result.
- On Hikvision, two paid calls (BIS and UK) came back unanswered. The Entity List match came from the Mind's free second source, and the Mind reported this itself.
- In the clean-control run the Mind kept its verdict at REVIEW until a missing BIS check was closed with a second source.
- Testnet only. The runs used testnet USDC.
- A fourth case (PJSC Sovcomflot, on the OFAC and UK lists) was planned and skipped to save the Mind's usage allowance. Hikvision already covers sanctions.

## Reproduce a run

Use a new thread, paste one of these, and compare the verdicts.

**Run 4A (with Telegraph)**

```
New supplier check, comparison test run 4A (WITH Telegraph). Treat this as a first-time check: do not reuse anything you remember from earlier checks.

Use your normal process: Ask Anjoman for paid facts, plus free sources (GLEIF, UN list). Same budget rules as always.

Invoice received:
- Supplier: Siemens AG
- Address: Otto-Hahn-Ring 6, 81739 Munich, Germany
- Register: HRB 252581, Amtsgericht Muenchen
- Item: 2 x SGT-800 turbine maintenance kits at EUR 18,500.00 each
- Total: EUR 37,000.00, payment by bank transfer
Purchase order on file: issued to Siemens AG, 2 x SGT-800 turbine maintenance kits at EUR 18,500.00 each, total EUR 37,000.00

Give one verdict (RELEASE, HOLD or REVIEW), the reasons, and a table of every source used, plus the total cost.
```

**Run 4B (without Telegraph)**

```
New supplier check, comparison test run 4B (WITHOUT Telegraph). Treat this as a first-time check: do not reuse anything you remember from earlier checks.

For this run, do NOT use Ask Anjoman or any Telegraph or Anjoman service. Use only web search and free public sources (GLEIF, UN list). Same budget rules otherwise.

Invoice received:
- Supplier: Siemens AG
- Address: Otto-Hahn-Ring 6, 81739 Munich, Germany
- Register: HRB 252581, Amtsgericht Muenchen
- Item: 2 x SGT-800 turbine maintenance kits at EUR 18,500.00 each
- Total: EUR 37,000.00, payment by bank transfer
Purchase order on file: issued to Siemens AG, 2 x SGT-800 turbine maintenance kits at EUR 18,500.00 each, total EUR 37,000.00

Give one verdict (RELEASE, HOLD or REVIEW), the reasons, and a table of every source used, plus the total cost.
```

The correct answer is HOLD or REVIEW. HRB 252581 and Otto-Hahn-Ring 6 belong to Siemens Energy AG, not Siemens AG. Setting up your own Mind to run this: see [SETUP.md](../SETUP.md).

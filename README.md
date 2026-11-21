# Supplier.Detective

A Mind (on [Minds by Animoca Brands](https://hellominds.ai)) that checks a supplier before a payment is released. It buys verified facts from Telegraph through [Anjoman](https://anjoman.ai), questions every answer, second-sources anything wrong, missing or conflicting, and ends each check with one verdict: **RELEASE**, **HOLD** or **REVIEW**, with every source, correction and cost listed.

Built for Telegraph Hackathon Season II, Apps & Agents track.

## Try the setup yourself

**[SETUP.md](SETUP.md)** shows how to connect any Mind to Anjoman in about 15 minutes, using only the Mind's HTTP tool and an API key.

## What it has caught so far

- **Siemens AG:** Anjoman returned the LEI when asked for the German register number. The Mind noticed, held the decision, found HRB 6684 (Amtsgericht München) through GLEIF and corrected itself before releasing. [Receipt](https://anjoman.ai/shared/OVQKl9jT5_bGRr7K_7qIhg)
- **Hangzhou Hikvision Digital Technology:** verdict HOLD. It caught a $2,000 gap between the invoice and the quote, found matches on the US BIS Entity List and OFAC's NS-CMIC list, and reported the UK and Canada results as empty rather than calling them clean. 6 paid calls, $0.10.

More cases, a with-and-without-Telegraph comparison and a demo video are on the way.

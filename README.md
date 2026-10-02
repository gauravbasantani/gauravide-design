# Gauravide

**Split a shared receipt into Splitwise, item by item.**

Photograph a receipt, tap who had what, and it lands in Splitwise as separate
line items. Tax goes to whoever actually bought the taxed thing instead of
being split evenly, in the receipt's own currency.

**Live:** https://gauravide.vercel.app · **Free** · Not affiliated with Splitwise

> **About this repository.** This is the public, documentation-only companion to
> Gauravide. The application source is private. What is here is the design
> record: what the product does, how it is structured, the decisions behind it,
> and the failures that shaped it. If you are evaluating my work, this is the
> part worth reading anyway.

---

## The problem

Four people eat together. One person pays. Then somebody spends ten minutes
typing items into a splitting app while everyone waits, and usually gives up and
splits it evenly, which is wrong for whoever only had a salad.

Existing tools split a total. The interesting work is in the lines.

---

## What it does

1. Upload a photo or PDF of a receipt.
2. The reader resolves it line by line, with quantities, discounts and tax.
3. Tap who had what. Items can be shared, or split unevenly within one line.
4. It writes to Splitwise as individual items on an existing group.

### What it handles

| Case | Behaviour |
|---|---|
| Per-item tax | Attributed to the buyer of the taxed item, not spread across everyone. VAT, GST, HST and ALV |
| Multiple currencies | Resolved in the receipt's own currency, never silently converted |
| Multi-page PDFs | Read as one receipt |
| Quantity lines | A line of 3 can be decomposed across different people |
| Multiple payers | Supported on one receipt |
| Reconciliation | A split that does not balance to the cent is never written |

---

## Design decisions worth defending

### The system has to prove itself before it writes anything
Every split reconciles to the cent before it reaches Splitwise. If the line
items do not sum to the receipt total, nothing is written. A wrong expense in
someone's account is worse than no expense, because nobody goes looking for it.

### The failure path is designed, not an afterthought
The reader is not always right, and the useful question is what happens when it
is not. Anything it is unsure about drops into a hand-check path rather than a
guess. Across 67 scans: **28 read automatically, 39 went through the hand-check
path with every line verified, 0 were unreadable.**

The automatic rate is deliberately lower than it could be. The failure mode I
care about is being confidently wrong about somebody's money.

### Privacy defaults, not privacy settings
- The Splitwise connection uses OAuth, so a password never reaches this product.
- Photo location data is stripped before upload **by default**, not as an option
  somebody has to find.
- The disclosure of what the connected account can access is written in plain
  language, because the standard permission notice sounds far broader than what
  the product actually does.

### Web, not an app
Nobody installs something to split one dinner. The whole flow has to work from a
link, on a phone, one-handed.

---

## Results

First 30 days, from the aggregate dashboard:

| | |
|---|---|
| Real splits | **15** |
| Money split | **$1,337.26** |
| Line items | **90** |
| People involved | 7 |
| Scans (includes my own testing) | 67 |
| Unreadable | **0** |

---

## How it was built

I designed it and **directed Claude Code to write the implementation.** I owned
the problem, the flows and edge cases, the product decisions (Splitwise as the
destination, web over native, free, the privacy defaults) and what to test and
refuse.

Documented in [`docs/`](docs/):

- [`architecture.md`](docs/architecture.md) — how the pieces fit
- [`failure-log.md`](docs/failure-log.md) — nine silent failures and what each one changed
- [`decisions.md`](docs/decisions.md) — the calls I made and what I gave up

---

## Why the source is private

It holds OAuth credentials for a third-party integration and the test fixtures
include real receipts. Rather than publish a hollowed-out version, this
repository carries the reasoning, which is the part that transfers.

Happy to walk through the code directly in a conversation.

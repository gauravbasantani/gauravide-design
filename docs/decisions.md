# Decisions

Each one with what it cost, because a decision without a tradeoff is just a
feature list.

### Splitwise as the destination, rather than building a ledger
People already settle up somewhere. Being a step in an existing habit beats
asking for a new one.
**Cost:** bound to a third-party API, its rate limits and its outages.

### Web, not a native app
Nobody installs something to split one dinner. It has to work from a link.
**Cost:** no camera-level integration, no push, no offline.

### Per-item tax instead of proportional tax
Tax belongs to whoever bought the taxed item. Spreading it evenly is the thing
that makes people quietly stop trusting a split.
**Cost:** materially harder parsing, and the main source of the failures in
`failure-log.md`.

### Refuse rather than approximate
A split that does not reconcile to the cent is never written.
**Cost:** a visible dead end instead of an invisible wrong number. Correct trade.

### Privacy as a default, not a setting
Location data stripped at intake. OAuth so no password is handled.
**Cost:** one more thing to maintain, and nobody ever notices it working.

### Free
**Cost:** no revenue, so it has to stay cheap to run.

### Directed Claude Code rather than writing it line by line
I owned the problem, the flows, the edge cases and what to refuse. The
implementation was written by directing an AI coding tool.
**Cost:** I had to build the verification habit deliberately, because the code
looked finished long before it was correct. The review session in
`failure-log.md` is what that habit produced.

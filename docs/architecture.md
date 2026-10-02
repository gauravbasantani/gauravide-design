# Architecture

A conceptual map, not a code listing. Enough to follow how a receipt becomes a
Splitwise expense.

```
  Photo or PDF
       |
       v
  [ Intake ]  strips photo location data before anything is stored
       |
       v
  [ Reader ]  resolves lines, quantities, discounts, per-item tax, currency
       |
       +---- confident ------> parsed lines
       |
       +---- unsure ---------> hand-check path, every line shown for verification
                                      |
                                      v
                            [ Assignment ]  tap who had what
                            shared lines, uneven splits within a line
                                      |
                                      v
                            [ Reconciliation ]
                            lines must sum to the receipt total, to the cent
                            currency in must equal currency out
                                      |
                            fails ----+---- passes
                              |              |
                           refuse            v
                                     [ Splitwise write ]
                                     OAuth, item by item, onto an existing group
```

## The boundary that matters

**Reconciliation sits between the product and the third party, and it can say
no.** Nothing reaches a user's Splitwise account unless the arithmetic balances
and the currency is unchanged. That check is the reason a parser being wrong is
recoverable rather than expensive.

## Trust and permissions

- **OAuth** to Splitwise. The product never sees a password.
- The permission notice a user sees during that step is broader than what the
  product does, so the disclosure beside it is written in plain language: what
  is read, what is written, what is never touched.
- **Photo location data is stripped by default**, at intake, before storage.

---

## On the stack

Deliberately not listed here. The decisions above are the part that transfers,
and a framework list dates faster than the reasoning does. Happy to go through
it in a conversation.

# Failure log

One review session, about three and a half hours, found **nine failures that had
never shown an error**. That is the category that matters: not crashes, which
announce themselves, but wrong answers that look right.

Listing them because the fixes say more about how I work than the feature list
does.

---

## 1. A twenty pound receipt arrived in Splitwise as twenty dollars

The worst one. Same number, right place, completely wrong. Nothing in the system
checked whether the currency that went in was the currency that came out.

**What changed:** currency is carried end to end and asserted at the write
boundary. A mismatch is a refusal, not a conversion.

**Why it had survived:** every test receipt had been in dollars.

---

## The other eight

The remaining eight were the same shape: wrong answers that looked right. They
came from deliberately feeding the reader things it had not been built for,
rather than from waiting for someone to report a problem. Discounts applied at
the wrong level, rounding gaps when one line was split unevenly between people,
and tax landing on the group instead of on the person who bought the taxed item.

None of them threw an error. That is the whole point. A crash tells you it
happened; a quietly wrong total does not.

---

## The rule that came out of it

Before this session, the model's output was treated as the answer. After it, the
system has to prove the answer:

- A split that does not reconcile to the cent is never written.
- Anything the reader is unsure about drops to a hand check instead of a guess.

The automatic read rate went **down** as a result. That was the right trade. A
dead end costs somebody thirty seconds. A confident wrong number costs them
money and they may never notice.

# Build brief

`ERD.md` says what the data is. This says what to build, in what order, and how
we know it worked. Read both before writing anything.

Where this document makes a call that is really a matter of taste, it says so
and gives the recommendation it is proceeding on. Overrule any of them.

---

## 1. What v1 is

**One question, answered completely, for one state.**

> *"I need a cortisone shot in my knee. What will it cost me at this clinic, and
> what do I say when I call?"*

Answered means: a number with a range, a warning if that address bills a
facility fee, and the sentences to say — with citations — before booking.

**v1 scope (taste call, proceeding):** California only; the ~50 procedures
people actually shop for; federal rights plus California's. National is a data
problem, not a design problem, and it can wait until the shape is proven.

### Non-goals for v1

- No insurance plan matching. Deductibles, coinsurance and accumulators are a
  separate product and a much worse one to get wrong.
- No individual provider pricing (taste call, proceeding). Locations and
  organisations only. Naming humans invites fights the data may not survive.
- No appointment booking, no referrals, no clinical advice of any kind.
- No accounts, no login, no email capture.

---

## 2. The vertical slice, first

Build this end to end before broadening anything. It proves or kills the thesis
in a week.

1. **One procedure:** CPT 20610, knee injection.
2. **One area:** a single California Medicare locality.
3. **One question page:** procedure + place in, answer out.
4. **All four answer components:** Medicare floor, cash range, facility-fee
   warning, scripts.

If the facility-fee warning cannot be produced honestly for real addresses, say
so early and loudly — that finding is the product, and if the data will not
support it we need to know in week one rather than month three.

---

## 3. Stack

**Taste call, proceeding unless overruled:**

| Layer | Choice | Why |
|---|---|---|
| Store | PostgreSQL | the model is relational, the queries are joins, and the observation table wants real indexes |
| Ingest + API | Python | the work is file parsing and data reconciliation, which is where its libraries are strongest |
| Front end | Plain TypeScript, no framework | one form and one answer page; browser OCR is the only heavy client work and it is WASM either way |
| OCR | WASM in the browser | §4a of ERD.md — non-negotiable, not a preference |
| Hosting | Static front end + small API | must run cheaply; this thing earns nothing |

Money is **integer cents** everywhere. Never floats, never `Decimal` in one
layer and `float` in another.

---

## 4. Ingest order

Build ingestion in this sequence; each step is useful on its own.

1. **Medicare Physician Fee Schedule** → `FEE_SCHEDULE_RATE`, `LOCALITY`,
   `PROCEDURE`. Smallest, cleanest, and it gives every later number something to
   be compared against. The floor is the anchor of the whole product.
2. **NPPES / NPI registry** → `PROVIDER`, `ORGANIZATION`, `LOCATION`. Addresses
   are messy; normalise once, keep the raw string, never "fix" it in place.
3. **CMS Hospital Price Transparency files** → `HOSPITAL`, `PRICE_OBSERVATION`
   (`gross_charge`, `discounted_cash`, `negotiated`). Schemas vary in practice
   despite the standard; expect per-hospital adapters and record which adapter
   produced each row.
4. **Provider-based / HOPD evidence** → `BILLING_CLASS_SIGNAL`. The hardest and
   most valuable. Multiple weak sources, combined — see §6.
5. **Charity care policies (HCAI for California)** → `CHARITY_POLICY`.
6. **Rights and scripts** → `RIGHTS_RULE`, `SCRIPT`. Hand-authored, few rows,
   every one carrying its citation. Seed rows are listed in ERD.md §3.

Every ingest writes a `SOURCE` row with the URL, retrieval time and file hash.
**An ingest that cannot say where a number came from is a bug, not a shortcut.**

---

## 5. The estimator

Given a procedure and a location, produce `low`, `expected`, `high` and the
basis. This is the product's core logic and it must be explainable in one
sentence to the person reading it.

```
1. floor     = FEE_SCHEDULE_RATE for (procedure, locality, setting implied by billing_class)
2. candidates = PRICE_OBSERVATIONs for (procedure, location), else
                (procedure, organisation), else (procedure, locality) — widening,
                and the widening is recorded and shown
3. weight each candidate by:
     source rank  (discounted_cash published > user_reported > negotiated > gross_charge)
     recency      (older observations decay; never discard, only weigh)
     specificity  (this location > this org > this area)
4. expected = weighted median of candidates, never a mean
   low/high  = 10th/90th weighted percentile, floored at `floor`
5. if billing_class = hospital_outpatient_dept:
     add the facility-fee distribution for that hospital, or the statewide one
     when that hospital has none, and SAY WHICH
6. if candidates is empty: return the floor with an explicit
   "nobody publishes a price for this here" — a blank is a finding, not an error
```

**Median, not mean, and never averaging contradictions away.** Two sources that
disagree by 3x are shown as a range with both cited. The disagreement is the
most useful thing on the page.

---

## 6. `billing_class`, and how it is decided

The field that moves the price by half. Derived, never typed.

| Signal | Weight | Notes |
|---|---|---|
| hospital's own provider-based location list | 0.9 | strongest; it is their own claim |
| address appears in a hospital price transparency file | 0.7 | |
| state facility-fee disclosure registry | 0.8 | where the state maintains one |
| user-reported bill containing a facility fee line | 0.6 | direct evidence of the harm |
| hospital website listing the clinic as a department | 0.4 | marketing, but indicative |
| NPI record shows independent practice only | −0.5 | evidence against |

Rules:

- Combine as weighted evidence; **display the score's reasoning, not the score.**
  "Probably bills as a hospital outpatient department, because the hospital's
  own list named this address in June 2026" is honest. A badge is not.
- Below a confidence threshold the answer is **`unknown`**, shown as *"we could
  not determine this — ask them directly, and here is the question."*
- A single user-reported facility fee is enough to flag `unknown` → *"at least
  one person was charged a facility fee here."* Never enough alone to assert
  `hospital_outpatient_dept`.
- Always show `billing_class_as_of`. Ownership changes, and a stale answer is
  how someone gets hurt while trusting us.

---

## 7. Acceptance criteria

v1 is done when all of these pass:

1. **Golden case.** CPT 20610 in a California locality returns a Medicare
   non-facility floor near **$68.81** (2026 national, locality-adjusted), an
   expected cash range in the low hundreds for an independent office, and a
   materially higher range for an HOPD address.
2. **The warning fires.** A known hospital-owned clinic address produces a
   facility-fee warning with its evidence and date. A known independent practice
   does not.
3. **The scripts appear**, with citations: the self-pay sentence (45 CFR
   149.610), the $400 dispute, and California's charity care policy with the
   hospital's own thresholds where known.
4. **Provenance everywhere.** Every number on the answer page can be traced to a
   `SOURCE` row, and the page says how old each is.
5. **Nothing sensitive is transmitted.** A bill photographed in the browser
   produces only the six whitelisted fields; network inspection during the flow
   shows no image bytes leaving the device.
6. **Absence renders.** A procedure with no published price anywhere shows the
   floor plus an explicit "nobody publishes this here", never a blank or a zero.
7. **Reads at speed.** The answer page is comprehensible to a frightened person
   in under thirty seconds: number, warning, three sentences to say.

---

## 8. Hard prohibitions

An implementer must not, without an explicit decision from Mike:

1. **Accept an uploaded image, or send one to any third-party OCR service.**
2. **Store any of:** patient name, address, date of birth, account or member
   number, guarantor, or **diagnosis codes**.
3. **Require an account**, capture an email, or store submitter IP addresses.
4. **Fingerprint submitters** for rate limiting. Use hashed short-TTL buckets or
   proof-of-work.
5. **Average away disagreeing observations**, or edit an observation in place.
   Corrections are new rows with later dates.
6. **Assert a `billing_class`** from a single weak signal, or display one without
   its date and reasoning.
7. **State a price, a right or a threshold without a citation.** Where the
   citation is missing, the feature is not ready.
8. **Give clinical advice**, or imply that a cheaper option is a medically
   equivalent one.

---

## 9. Still open for Mike

Answer these before or during the slice; none blocks starting.

- **Repository visibility.** It is public now, at his instruction. One command
  makes it private.
- **v1 scope** — California and 50 procedures, as above?
- **Provider-level pricing** — locations and organisations only, as above?
- **Name and tone of the public face.** `anarmandaleg` is the repo. Whether it
  is also what a frightened person sees at 11pm is a different question.

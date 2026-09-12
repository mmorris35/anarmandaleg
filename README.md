# anarmandaleg

> **Lying by omission is lying.**

Find out how badly you are about to be charged — before you go — and learn the
words that make them tell you.

## The comparison that explains this in one breath

You finance a car. Before you sign, federal law hands you the APR, the total of
payments and an itemisation. Mandatory, standardised, since 1968.

You book a knee consult. You get a shrug, and a bill six weeks later.

There is a law that closes this — the **Good Faith Estimate**, which entitles you
to a written, itemised price *before* you are committed. It is a few years old,
almost nobody has heard of it, and it only triggers if you happen to say a
particular sentence out loud. Financing a car is better regulated than financing
your own body.

## Why the price is not the price

A cortisone shot in a knee is CPT 20610. Medicare pays about $69 for it in an
office. Cash at an independent practice is $150–300, all in. The same needle,
from the same doctor, in a clinic a hospital happens to own, is $500–1,000+.

**The procedure is not the variable. The building is.** Nobody tells you that
before you book, and by the time the bill arrives the decision is months gone.

Every price tool on the internet answers "what does this procedure cost". That
is the wrong question, which is why none of them help when you are standing at a
scheduling desk deciding whether you can afford to be seen.

## Who this is for

Not only the people with no money. **The people just above the line.**

Under 200% of the federal poverty level, California hospitals owe you free care.
From 200–400%, a sliding scale. Above 400%, the obligation stops — and that is
exactly where high-deductible households live. Earning well and still unable to
absorb a $5,000 deductible is the modern condition, and every assistance
programme ends precisely where that exposure begins.

So the question this asks is never *"are you poor?"* It is **"do you expect to
hit your deductible this year?"** — which is the input that actually decides
what you should do, and costs you no dignity to answer.

## What it answers

1. **What is this likely to cost me here?** The Medicare rate as a floor, the
   cash price where it is published, and what other people actually paid.
2. **Is this address going to add a facility fee?** A hospital outpatient
   department bills a $150–500 facility fee for a visit an independent office
   bills once. Same care, two bills. And the same fact cuts both ways: if the
   clinic bills under a hospital, that hospital's financial assistance policy
   may reach the bill.
3. **Which path is cheapest for me, and what does it cost me?** Self-pay,
   prompt-pay discount, financial assistance, or a different site of care — each
   shown with its catch. Self-pay usually does not count toward your deductible;
   that single fact can reverse the answer.
4. **What do I say?** The sentences that carry legal weight, with citations:
   - *"Do not bill my insurance, I am self-pay"* makes you a self-pay patient and
     entitles you to a written, itemised **Good Faith Estimate**. Having
     insurance has never obliged you to use it for a given visit — the choice is
     per encounter, and no front desk describes it as a choice.
   - Pay in full out of pocket and a provider **must** agree to keep that service
     from your health plan (45 CFR 164.522(a)(1)(vi)) — must, not may.
   - If the final bill lands **$400 or more** over the estimate, there is a
     federal dispute and the reviewer's decision binds the provider.
   - Non-profit hospitals must have a **financial assistance policy**. In
     California the mandatory tier reaches 400% FPL, and hospitals are
     *permitted* to go further — which makes it an ask, published per hospital,
     that nobody at a desk will volunteer.

## Design rules

- **A price is never a fact, only an observation** with a source, a date and a
  confidence. Two sources that disagree are both kept; the disagreement is
  information, not an error to average away.
- **Absence is evidence.** A hospital that publishes no cash price is a finding,
  not a gap in our data — recorded as a dated, sourced claim. Over time that
  produces the thing no price tool has: **a map of who tells you and who does
  not.**
- **No account, no PHI, and no photographs.** If you photograph a bill, the image
  stays on your phone: text is extracted on-device and only six whitelisted
  fields are ever transmitted, after you have seen and corrected them. You
  cannot leak, subpoena or breach what you never received.
- **Written for the half of readers below the median.** Spoken lines at roughly a
  fifth-grade level, tested rather than trusted; every sentence must survive
  being heard once, out loud, by a frightened person talking to a stranger.
  Citations live in footnotes, never in the words you say.
- **Shame-free, because shame is what keeps this system alive.** People do not
  ask about cash prices, apply for assistance, or dispute bills, because asking
  feels like confessing you could not afford to be there. Nothing here implies
  you should have known. You were never told there was a negotiation.
- **The person at the desk is not the adversary.** They were not told the cash
  price either, cannot quote it, and are measured on how fast the queue moves.
  Every script names who can actually answer and hands you the escalation line
  before you need it. The tool wears the armour so you do not have to.
- **Cite or stay quiet.** Every right, threshold and deadline carries its statute.
  A tool that tells you what to say to a hospital had better be able to show why
  it works.

## On clinicians

No individual doctor is priced, ranked or compared here. In the room, nobody
should have to fight anybody.

That is not absolution for the profession. A specialty that says *first, do no
harm* while treating price as another department's problem has drawn the
boundary of harm around what is convenient — and cost is a clinical variable,
because people skip doses, delay scans and abandon follow-ups over it. Most
physicians genuinely are not shown the negotiated rate and do not set the
chargemaster. That indicts the institution that keeps them ignorant, not the
person with their hands on you.

**Doing your job well is not a defence against knowing what your job produces.**
Plenty of clinicians already feel this from the other side of the desk. They are
allies, not targets.

## The name

`anarmandaleg` is the repository. The product may end up called something a
frightened person can say out loud at 11pm — that name is not chosen yet.

The repo keeps this one either way. Mike, who started it after being quoted $500
to talk to a doctor about a knee he already had the answer for: *"The product
name could change but my righteous anger with the system is carried in
anarmandaleg."*

## Status

Design only. `docs/ERD.md` is the data model, `docs/BUILD.md` is the brief.
Nothing is built yet.

## Data sources

All public, all free:

| Source | Gives us |
|---|---|
| CMS Hospital Price Transparency machine-readable files | gross charges, discounted cash prices, negotiated rates, per hospital |
| Transparency in Coverage payer files | negotiated rates per plan (very large) |
| Medicare Physician Fee Schedule | the floor every price is compared against, by locality |
| NPPES / NPI registry | providers, organisations, practice addresses |
| CMS Provider of Services + provider-based listings | which addresses bill as hospital outpatient departments |
| HCAI (California) and state equivalents | charity care and fair pricing policies |
| Federal and state statute | the rights, thresholds and deadlines |

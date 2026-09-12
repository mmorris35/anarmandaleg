# anarmandaleg

Find out how badly you are about to be charged — before you go — and learn the
words that make them tell you.

## Why

A cortisone shot in a knee is CPT 20610. Medicare pays about $69 for it in an
office. Cash at an independent practice is $150–300, all in. The same needle,
from the same doctor, in a clinic a hospital happens to own, is $500–1,000+.

The procedure is not the variable. **The building is.** Nobody tells you that
before you book, and by the time the bill arrives the decision is months gone.

Every price tool on the internet answers "what does this procedure cost". That
is the wrong question, which is why none of them help when you are standing at a
scheduling desk deciding whether you can afford to be seen.

## What this answers

1. **What is this likely to cost me here?** The Medicare rate as a floor, the
   cash price where it is published, and what other people actually paid.
2. **Is this address going to staple a facility fee onto it?** A hospital
   outpatient department bills a $150–500 facility fee for a visit an
   independent office bills once. Same care, two bills.
3. **What do I say?** The sentences that carry legal weight:
   - *"Do not bill my insurance, I am self-pay"* makes you a self-pay patient
     under 45 CFR 149.610, and they then owe you a **written, itemised Good
     Faith Estimate** — codes, charges, every provider's NPI.
   - If the final bill lands **$400 or more** over that estimate, there is a
     federal dispute and the reviewer's decision binds the provider.
   - Non-profit hospitals must have a **charity care policy**, must tell you it
     exists, and in California must publish it. Under 200% FPL is free care in
     California; 200–400% is a sliding scale.

Prices tell you how hard you are about to be hit. The scripts are how you hit
back.

## Design rules

- **A price is never a fact, only an observation** with a source, a date and a
  confidence. Two sources that disagree are both kept; the disagreement is
  information, not an error to be averaged away.
- **No account, no PHI, and no photographs.** Nothing here needs to know who you
  are or what is wrong with you. If you photograph a bill, the image stays on
  your phone: text is extracted on-device and only six whitelisted fields — the
  place, the codes, the amounts, whether a facility fee appeared, and the month
  — are ever transmitted, after you have seen and corrected them. You cannot
  leak, subpoena or breach what you never received.
- **Cite or stay quiet.** Every right, threshold and deadline carries its
  statute or regulation. A tool that tells you to say something to a hospital
  had better be able to show why it works.
- **Built for a frightened person at a desk**, not a researcher with an
  afternoon. The answer is a number, a warning and three sentences to say.

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

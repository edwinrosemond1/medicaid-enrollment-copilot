# Build vs. Fortuna — Competitive Analysis

## Who Fortuna Health is
- **"Medicaid Made Easy"** is Fortuna's marketing tagline (fortunahealth.com). Founded 2023; YC + a16z-backed; **~$22.3M raised** ($4M seed, $18M Series A). Positioned as "TurboTax for Medicaid."
- **Business model: B2B2C.** Sells to health plans and health systems; patients are end users. *"Only available through providers and insurers"* — not direct-to-consumer.
- **Hybrid product:** software (30–60s eligibility screen, document "virtual mailbox," renewal reminders, partner analytics) **+ a workforce of certified/licensed human navigators** who work complex cases through with the state.
- **Scale focus:** consolidates "56 Medicaid programs," serves payers covering 25M+ Medicaid lives, ~15 states.
- **Reported metrics:** 95% approval rate, 5x faster, 15% renewal-rate lift, 52% same-day completion.

## Two facts that reframe the opportunity
1. **Fortuna won't sell to Connections** — too small. Their onboarding cost (integrations, navigator staffing, compliance) only pencils at enterprise scale. This is **structural and permanent**, not "not yet."
2. **Fortuna doesn't operate in Connections' states.** Footprint: CA, CT, DE, IL, MI, MN, NV, NY, NC, OH, PA, SC, UT, WA, WV. **Arizona, Virginia, and Maryland are absent.**

## Reproducibility: separate the software from the moat
Fortuna's moat is mostly **not** the software.

| Component | Reproducible by you? | Notes |
|---|---|---|
| Eligibility screening + rules engine | **Easy** (weeks) | Core of our prototype |
| Document extraction (OCR / AI) | **Easy** | Off-the-shelf with Claude vision |
| Application pre-fill + packet assembly | **Easy–Medium** | Per-state form mapping is grunt work, not hard tech |
| Status tracking / renewal reminders | **Easy** | Standard workflow software |
| Per-state breadth (56 programs, 15 states) | **Hard / expensive** | Where their $22M went — **we don't need it** |
| Certified/licensed navigator workforce | **Hard (ops, not code)** | **Connections already employs these staff** |
| SOC2 / HIPAA / BAAs / payer integrations | **Medium (slow, not technical)** | Lighter for an internal tool |
| Enterprise sales motion | **N/A** | We have a captive first customer |

## What to build vs. deliberately skip

**Build (the high-value 20%):**
- 100% automated screening for AZ / VA / MD MAGI-adult pathway
- AI document extraction
- Packet assembly + completeness/consistency QA
- Application + renewal/redetermination tracking
- Human-approval step before submission

**Deliberately skip (Fortuna's costly breadth that we don't need):**
- 15-state / 56-program coverage → we need **3 states, 1 main pathway**
- Building a navigator labor force → **amplify existing staff** instead
- Multi-tenant SaaS, enterprise integrations, SOC2 → **internal tool first**
- Direct-to-consumer app → **provider-internal workflow**

## Why the scoped tool wins for Connections
- **Depth over breadth:** 3 states done well beats 15 done generically — and covers exactly where Fortuna can't.
- **Software-only:** Fortuna bundles software + labor; Connections already has the labor, so we build the cheaper half.
- **No sales cycle, lighter compliance:** internal deployment skips the enterprise go-to-market and much of the multi-tenant security burden that consumes startup capital.
- **The leader validated the ROI for us** — a16z doesn't put $22M into an imaginary problem.

## Strategic risks (and mitigations)
- **Don't chase breadth.** It's a money pit and not the need. Nail AZ, then VA/MD as config packs.
- **Keep humans in the loop.** Fortuna kept licensed navigators for a reason: edge cases, appeals, state judgment — also our compliance safety margin.
- **Portal automation is fragile.** Start by generating human-ready packets; automate submission only where it proves durable.
- **OBBBA changes keep moving.** The rules engine must be **cheap to update** as states implement work requirements and 6-month redeterminations.

## Optional later expansion (if it works)
Productize for **other small/mid behavioral-health & crisis providers** — same problem, same Fortuna gap, almost no enrollment-focused competition downmarket. The per-state rules packs you accumulate become the asset.

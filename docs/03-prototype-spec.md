# Medicaid Enrollment Co-Pilot — Prototype Spec (AZ / VA / MD)

*Full technical + strategic spec, with Fortuna competitive analysis folded in.*

---

## 1. Problem
Connections Health Solutions runs behavioral-health crisis facilities. A large share of crisis patients are **uninsured but Medicaid-eligible**. Human benefits teams can attempt only ~10% of patients because enrollment is manual and state-specific. Unenrolled eligible patients = uncompensated care. OBBBA (HR1) cut Medicaid funding and tightened eligibility/verification/redetermination, raising both the value of enrolling everyone eligible and the cost of letting enrolled patients lapse.

**Goal: leverage, not headcount.** Let existing staff screen 100% of patients and process far more applications each.

## 2. Competitive context (why this is worth building)
The market leader is **Fortuna Health** ("Medicaid Made Easy," YC/a16z, ~$22.3M raised) — a B2B2C software-plus-licensed-navigator platform sold to large payers/health systems.
- **Won't serve Connections** (too small — structural).
- **Doesn't operate in AZ / VA / MD** (their footprint is CA/CT/DE/IL/MI/MN/NV/NY/NC/OH/PA/SC/UT/WA/WV).
- Their moat is **breadth + a navigator workforce + capital**, *not* the core software.

We build the **high-value 20%**: depth in 3 states, software only (Connections already employs navigators). See `02-build-vs-fortuna.md`.

## 3. Scope of the prototype
- **States:** Arizona (AHCCCS) first; Virginia and Maryland as follow-on rules packs.
- **Pathway:** **MAGI adult / expansion adult** (≤138% FPL). All three states are expansion states, so this one pathway covers most crisis patients.
- **Pilot recommendation:** start with **Arizona** (HQ state, modern Health-e-Arizona Plus portal). **Maryland** is a strong second test bed (modern state-based exchange, Maryland Health Connection) — useful since you have potential test partners there.

## 4. Pipeline
1. **Universal screening (10% → 100%).** Ingest patient demographics + financials (CSV / HL7 / FHIR export, or manual entry in prototype). Rules engine flags likely eligibility + category.
2. **State eligibility rules engine.** AZ/VA/MD criteria as versioned, config-as-data rules (income as % FPL, household composition, residency, categorical pathways, expansion status). New state = new rules pack, not new code.
3. **Document intake + extraction.** Capture ID, proof of income, residency. Claude (vision) OCRs/extracts structured fields from photos/PDFs of pay stubs, IDs, benefit letters.
4. **Application assembly + pre-fill.** Map data onto each state's application; Claude drafts narrative fields and runs completeness/consistency QA before human sign-off.
5. **Submission.** (a) **Default:** generate a human-ready packet for staff to submit. (b) **Phase 2:** Playwright browser automation against the state portal.
6. **Tracking.** Follow to determination; surface "action needed" (missing docs, RFIs); track **renewals/redeterminations**.

**A human approves before any submission** — compliance-safe and the right product posture.

## 5. Tech stack
| Layer | Choice | Why |
|---|---|---|
| Backend | Python (FastAPI) | Fast for LLM + rules |
| Rules engine | Config-as-data (YAML/JSON) + thin Python evaluator | State rules change often; keep out of code |
| LLM | **Claude `claude-opus-4-8`** (Haiku for cheap classification) | Vision + reasoning; one vendor |
| Doc handling | Claude vision / Files API + PDF input | Pay stubs, IDs, benefit letters |
| Portal automation (phase 2) | Playwright | When moving to auto-submit |
| Storage | Postgres on HIPAA-eligible host (AWS w/ BAA) | This is PHI |
| Frontend | Lightweight React worker-queue UI | Triage / review / approve |

## 6. Domain must-gets (value + risk live here)
- **Hospital Presumptive Eligibility (HPE).** Qualified hospitals/providers can grant **immediate temporary coverage** at point of care while the full app processes. Huge for crisis care. **Verify per state** whether Connections' facilities qualify; if so, make HPE a first-class feature.
- **Retroactive eligibility.** Medicaid has historically covered up to 3 months retroactively — patients enrolled *after* discharge can still generate payment. Several states have shortened/waived this; OBBBA-era changes tighten further. **Verify per state** — it swings ROI.
- **Expansion status.** AZ, VA, MD are all expansion states → childless adults ≤138% FPL qualify (the bulk of the population). This is why one pathway suffices.
- **OBBBA / HR1.** Cut Medicaid funding; added stricter verification, work/"community engagement" requirements, more frequent (e.g., 6-month) redeterminations. *(Treat specific dollar/date figures as needing verification — still being implemented through 2025–26.)* Strategic point holds: more enrollment **and retention** work per patient = more value to amortize. **Build renewal/redetermination tracking** — may be a bigger recurring market than initial enrollment.
- **State-by-state customization is the moat, not the bug.** It's why few competitors do enrollment (vs. verification) and why Fortuna can't come downmarket cheaply. Config-driven rules packs turn that pain into a repeatable expansion playbook.

## 7. Compliance (gating constraint)
- HIPAA-eligible cloud + signed **BAA** (e.g., AWS).
- **BAA with Anthropic** (available via API and via Amazon Bedrock) before sending real PHI.
- **Develop the prototype on synthetic/de-identified data** to move fast without the compliance gate; sign BAAs before any real PHI.
- Audit logging, access controls, encryption at rest/in transit, retention policy.
- Ideally **deploy inside Connections' existing HIPAA environment** rather than standing up your own.

## 8. Cost
**Build (prototype, AZ, 1 pathway):** ~**6–10 weeks** solo (~4–6 weeks for a pair). Mostly your time.
**Run (prototype scale):**
- Cloud + DB: ~$50–300/mo.
- **Claude Opus 4.8: $5 / 1M input tokens, $25 / 1M output tokens.** Realistic **~$0.05–0.25 per patient** (a few documents + assembly + QA). At 10,000 patients/mo ≈ low-hundreds to ~$1–2K/mo — trivial vs. revenue per enrolled patient.
- Use **prompt caching** for static rules/policy context (~90% input savings on repeats); **Haiku** for cheap classification steps.

The expensive part is **engineering + compliance**, not running it.

## 9. Value
- One behavioral-health Medicaid encounter ≈ **hundreds to thousands of dollars** reimbursed; a crisis stabilization episode can be several thousand.
- Going from ~10% to even **40–60%** enrollment **without new hires** converts large uncompensated-care volume into paid claims.
- Conservative back-of-envelope → **seven figures recovered per facility per year** vs. a weeks-long build and hundreds/month run cost. See `04-roi-calculator.md` / `roi-calculator.html`.

**Three ways it's valuable to you:**
1. **Internal tool for Connections** — clearest, fastest ROI (your friend is champion).
2. **Productized service** for other small/mid behavioral-health & crisis providers — same Fortuna gap, little downmarket competition.
3. **Defensible** — per-state customization keeps competitors out; rules packs accumulate as an asset.

## 10. First steps
1. Confirm pilot state (**recommend AZ**), data access from EHR/registration, and HPE qualification.
2. Build **screening + extraction + packet generation** for AZ MAGI-adult on **synthetic data**.
3. Demo "100% screened, pre-filled packets, human approves" → measure **time-per-application vs. current manual process**. That single number is the business case.

## 11. Open verification items (don't quote without checking)
- [ ] HPE qualification for Connections' facilities in AZ / VA / MD
- [ ] Retroactive-eligibility windows currently in effect per state (post-OBBBA)
- [ ] Exact OBBBA work-requirement and redetermination effective dates per state
- [ ] EHR/registration data-export method and fields available
- [ ] Whether Anthropic BAA is via direct API or Bedrock for this deployment
- [ ] Current AZ/VA/MD MAGI income thresholds (% FPL) and required documents

---

*Monetization, IP, and pricing strategy are tracked separately in a private, local-only document and are intentionally excluded from this repository.*

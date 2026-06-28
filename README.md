# Medicaid Enrollment Co-Pilot

A tool that helps behavioral-health crisis providers enroll uninsured-but-eligible patients into Medicaid — turning manual, ~10%-coverage enrollment into a mostly-automated assembly line so existing staff can enroll far more patients without new hires.

**Status:** Scoping / pre-prototype · **Pilot scope:** Arizona, then Virginia & Maryland

---

## One-Pager

*For Connections Health Solutions · Internal review*

### The problem
Connections' crisis patients are often **uninsured but Medicaid-eligible**. Today, benefits staff can attempt to enroll only **~10%** of them — enrollment is manual and different in every state. Every eligible patient who isn't enrolled is **uncompensated care**. OBBBA (HR1) made eligibility, verification, and renewals stricter and more frequent, increasing the enrollment *and* retention work per patient — and the cost of missing it.

### The goal
**Leverage, not headcount.** Let the existing team screen 100% of patients and process far more applications each, by automating the repetitive work and reserving staff for judgment and exceptions.

### What it does — an automated assembly line
The tool turns a patient into a submitted Medicaid application, automating the labor at each step while a staff member approves before anything is submitted:

| Step | What's automated | Human role |
|---|---|---|
| **Screen** | Checks 100% of patients for likely eligibility (vs. 10% today) | — |
| **Read documents** | AI extracts data from photos of IDs, pay stubs, benefit letters; cross-checks for inconsistencies | Low-confidence reads only |
| **Assemble application** | Pre-fills the state form, drafts narrative fields, runs its own completeness/QA check | Final review |
| **Submit** | Generates a ready-to-submit packet (later: auto-submits to the state portal) | **Approves before submit** |
| **Track** | Follows each application to determination; flags renewals/redeterminations | Acts on flagged items |

Review is **exception-based** — only the ~10–20% of messy cases need human eyes; clean ones flow through. As the tool proves reliable, the automation threshold can be raised.

### Scope
**Arizona (AHCCCS) first**, then **Virginia** and **Maryland**. All three are Medicaid-expansion states, so one main pathway (MAGI adult) covers most crisis patients. New states are added as configuration, not a rebuild.

### Effort & timeline
Two kinds of work run in parallel at very different speeds. **Technical work** is fast and largely in our control (a developer + an AI agent). **Regulatory & people work** runs on others' calendars — legal, IT, compliance, state agencies — and is usually the *critical path* that determines when we can actually go live.

| Stage | Technical work (we control) | Regulatory & people work (others' timelines) | Elapsed time |
|---|---|---|---|
| **Tier 0 — Demo** | Build screening, document extraction, and packet assembly for AZ on synthetic data | None — synthetic data, so no compliance gate | **2–4 weeks** |
| **Tier 1 — Production pilot** | Harden the pipeline; add a simple EHR data feed and the staff review/approve UI (~3–4 weeks of build) | Sign BAAs, pass Connections' security review, get EHR data access, confirm HPE qualification | **2–4 months** — gated by the people track |
| **Tier 2 — Scaled** | Add Virginia & Maryland rules packs, portal auto-submission, renewals | Per-state compliance, portal access/agreements, ongoing rule updates as OBBBA changes roll out | **6–12 months** + maintenance |

**The key insight:** the Tier 1 *coding* is only ~3–4 weeks, but elapsed time is 2–4 months because the regulatory/people track gates go-live. Those conversations (BAAs, EHR access, HPE) should start **early and in parallel** with the Tier 0 build — they are the long pole, not the software.

### Cost
- **Upfront, to a production pilot: ~$15–40K all-in.** Most of that is compliance setup and data integration — the AI tooling that writes the software is only ~$1K, and a Tier 0 demo on synthetic data costs little more.
- **Ongoing: low hundreds/month** in hosting, plus **~$0.05–0.25 per patient** in AI processing — trivial against the reimbursement each enrolled patient generates.
- For contrast, a development shop would build this for **$100–300K+**.

### Why build rather than buy
The market leader (Fortuna Health) sells only to large health plans/systems — it **won't serve a provider this size** — and **does not operate in Arizona, Virginia, or Maryland.** A focused internal tool is the realistic path, and it's well within reach.

### The value
A single behavioral-health Medicaid encounter is worth **hundreds to thousands of dollars** in reimbursement. Moving from ~10% to even 40–60% enrollment **without new hires** converts a large block of uncompensated care into paid claims — plausibly **seven figures of recovered revenue per facility per year.** *(See the ROI model to plug in real numbers.)*

### The ask
1. Pick the pilot state (**recommend Arizona** — HQ, modern portal).
2. Identify how we get intake/registration data out of the EHR.
3. Confirm whether facilities qualify for **Hospital Presumptive Eligibility** (immediate temporary coverage at point of care — potentially the highest-leverage feature).
4. Green-light a **2–4 week Tier 0 demo** on synthetic data to measure time-per-application vs. the current manual process.

---

## Documents

| Document | Description |
|---|---|
| [docs/02-build-vs-fortuna.md](docs/02-build-vs-fortuna.md) | Competitive analysis vs. Fortuna Health — what to reproduce vs. skip |
| [docs/03-prototype-spec.md](docs/03-prototype-spec.md) | Full technical + scoping spec, with verification checklist |
| [docs/04-roi-model.md](docs/04-roi-model.md) | ROI model: formula, worked example, sensitivity |
| [docs/one-pager.txt](docs/one-pager.txt) | Plain-text one-pager (clean paste into Google Docs / email) |
| [assets/one-pager.html](assets/one-pager.html) | Styled one-pager — open in a browser, Print → Save as PDF |
| [assets/roi-calculator.html](assets/roi-calculator.html) | Interactive ROI calculator — edit inputs live in a browser |

> **Note:** Figures (cost, ROI) are estimates pending verification of state requirements and real volume data. Monetization/IP strategy is tracked in a private, local-only file and is intentionally not part of this repository.

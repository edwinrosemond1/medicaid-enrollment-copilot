# ROI Model — Medicaid Enrollment Co-Pilot

Plug Connections' real numbers into the variables below (or use `roi-calculator.html` for a live version). All figures are **illustrative defaults** — replace with actuals before quoting.

## Inputs (per facility, per year)
| Variable | Symbol | Default | Notes |
|---|---|---|---|
| Uninsured patients seen / year | `P` | 4,000 | From registration data |
| % of uninsured who are Medicaid-eligible | `E` | 60% | Crisis/behavioral-health populations skew high |
| Current enrollment **attempt** rate | `A_now` | 10% | Stated constraint today |
| Current **success** rate (of attempts) | `S_now` | 70% | Manual process, experienced staff |
| Tool-enabled **attempt** rate | `A_tool` | 80% | 100% screened; not all reachable/complete |
| Tool-enabled **success** rate (of attempts) | `S_tool` | 75% | QA + pre-fill lifts quality modestly |
| Avg realized reimbursement / enrolled patient | `R` | $2,500 | Per encounter/episode; **verify** — biggest lever |

> **Note on attempt rates:** "attempt" = a real application is worked, not just screened. The tool's power is decoupling *screening* (now 100%) from *staff effort*, so attempt rate rises without new hires.

## Formula
```
Eligible patients            = P × E
Enrollments today            = P × E × A_now  × S_now
Enrollments with tool        = P × E × A_tool × S_tool
Additional enrollments       = Enrollments_with_tool − Enrollments_today
Gross new revenue / year     = Additional enrollments × R
Net value / year             = Gross new revenue − annual tool cost
```

## Worked example (defaults)
- Eligible = 4,000 × 60% = **2,400**
- Today = 2,400 × 10% × 70% = **168 enrollments**
- With tool = 2,400 × 80% × 75% = **1,440 enrollments**
- Additional = 1,440 − 168 = **1,272 new enrollments / year**
- Gross new revenue = 1,272 × $2,500 = **$3.18M / year per facility**
- Annual tool cost (run + maintenance, est.) ≈ **$30–80K**
- **Net value ≈ ~$3.1M / year per facility**

Even cutting every assumption in half, the result stays comfortably in **seven figures per facility**.

## Sensitivity — what moves the number most
1. **R (reimbursement per patient)** — linear; verify real realized $ per encounter, including retroactive/HPE-captured visits.
2. **A_tool (attempt rate the tool unlocks)** — the core thesis; this is the lever the tool directly pulls.
3. **E (eligibility rate)** — how many uninsured actually qualify.
4. **P (volume)** — multiplies across facilities; the multi-facility total is the real prize.

## One-time build cost (separate from annual)
- Prototype (AZ, 1 pathway): ~6–10 weeks of dev (mostly labor).
- Adding VA + MD: incremental rules packs, not rewrites.

## Honest caveats
- "Attempt" and "success" rates with the tool are **assumptions** — the prototype's job is to *measure* them (time-per-application, completion %, approval %).
- `R` should reflect **realized** reimbursement (net of denials), not billed charges.
- HPE and retroactive eligibility can **add** captured revenue not modeled here (visits that would otherwise be fully uncompensated).

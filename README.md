# Excel Cyber Risk Models

> Two FAIR-based Monte Carlo cyber risk workbooks in Excel — one **teaching** version with the math exposed cell-by-cell, one **production** version with 20,000 iterations, percentile curves, sensitivity analysis, and histogram binning. Both implement the same loss model. Together they bridge the gap between "I want to understand FAIR" and "I want to run a defensible risk simulation."

[![Format](https://img.shields.io/badge/Format-Excel%20.xlsx-217346)]()
[![Methodology](https://img.shields.io/badge/Methodology-FAIR%20Monte%20Carlo-1F3A5F)]()
[![Iterations](https://img.shields.io/badge/Production-20%2C000%20iterations-success)]()

---

## Two workbooks, two purposes

| Workbook | Iterations | Purpose | When to use it |
|---|---|---|---|
| `teaching/cyber_risk_model_with_formulas.xlsx` | 500 | **Teaching artifact** — Excel formulas exposed in every cell, no hidden logic. Designed to be read, not just used. | When someone asks "how does FAIR work?" — open this, walk them through it. |
| `production/risk_to_value_model.xlsx` | 20,000 | **Production model** — percentile curves (P50, P90, P95), sensitivity grid across input ranges, histogram binning. | When you need to defend a board number or feed an actual budget conversation. |

Most CRQ practitioners pick a tool tier and stay there. The teaching tier is where most public material lives but it lacks rigor. The production tier is rigorous but opaque. Showing both — and being explicit about which is which — is the unusual move.

---

## The shared loss model

Both workbooks implement the same FAIR-inspired loss model. The math is identical; what differs is the iteration count, the visualization, and how the variables are presented.

```
ALE = Threat Event Frequency (TEF) × Vulnerability Probability × Loss Magnitude
```

| Variable | Distribution | Why this distribution |
|---|---|---|
| **TEF** | Triangular (min, mode, max) | Cyber loss events are bounded; the triangle is the right shape for expert elicitation |
| **Vulnerability Probability** | Beta / uniform | Bounded between 0 and 1, asymmetric tails |
| **Loss Magnitude** | Lognormal | Small losses common, catastrophic losses rare — the same shape used in insurance, op-risk, and financial risk |

The Monte Carlo run produces a distribution of possible annual losses. The output is not a single ALE number; it is a percentile curve. The median tells you what to expect in a typical year. The P95 tells you what to plan for in a bad year. The probability-of-exceedance curve tells you when to buy more cyber insurance.

---

## Why both versions?

This is a judgement call I want to make explicit, not bury.

**The teaching workbook** is uncommon in the CRQ space because most analytical work is done in Python or proprietary platforms (RiskLens, Safe Security). Showing the Excel math transparently — every input cell labelled, every calculation visible — is more pedagogically honest than any vendor demo. The 500-iteration limit is intentional: it runs in under a second, lets a reader watch the variation across runs, and forces an understanding that the output is a distribution, not a point.

**The production workbook** is what you would actually present in a board pack. 20,000 iterations stabilizes the percentile estimates. The sensitivity grid (varying TEF and Loss Magnitude over realistic ranges) is the analysis a CFO will ask for second, after they ask what the central estimate is. Histogram binning over the 20,000 outcomes is what turns the Monte Carlo from a black box into a visible shape.

Treating them as duplicates would be wrong. They serve different conversations. State that, and both become assets.

---

## How to use

### Teaching workbook

1. Open `teaching/cyber_risk_model_with_formulas.xlsx`.
2. Press **F9** to run a new 500-iteration simulation. Watch how the median, P95, and tail change run-to-run.
3. Hover any output cell to see the formula. Hover any input cell to see the source assumption.
4. Change a TEF or Loss Magnitude input by ±20% and press F9. The distribution shape shifts visibly — this is the lesson the workbook is built to teach.

### Production workbook

1. Open `production/risk_to_value_model.xlsx`.
2. Set your scenario-specific inputs on the **Inputs** tab. Default scenarios cover ransomware, cloud data breach, and AI misuse.
3. Press **F9** for a 20,000-iteration run. Expect ~1–2 seconds.
4. The **Output** tab gives you median ALE, P90, P95, P99, and a probability-of-exceedance curve.
5. The **Sensitivity** tab shows ALE across a TEF × Loss Magnitude grid — useful for tornado plots and for surfacing which input is doing the most work.

Neither workbook uses macros. Both work in Excel 2016+ and Excel for Web.

---

## When to reach for Python instead

If you need:

- Programmatic ingestion of input data from a real source (SIEM, CMDB, GRC tool)
- Reproducible runs from a notebook for an audit
- Charts rendered for a web dashboard
- More than 50,000 iterations on more than 5 scenarios in parallel

…then [`cyber-risk-quantification`](https://github.com/rafatyazdani/cyber-risk-quantification) is the Python implementation of the same math. The Excel workbooks here exist for the practitioner audience that lives in Excel and does not want to install a Python toolchain.

---

## Related artifacts in the portfolio

- [`cyber-risk-quantification`](https://github.com/rafatyazdani/cyber-risk-quantification) — the **Python implementation** of these same models, with Streamlit dashboard and Jupyter notebooks.
- [`CRQ-Dashboard`](https://github.com/rafatyazdani/CRQ-Dashboard) — the **production consulting application** built on the cyber-risk-quantification methodology, with multi-client architecture, PERT distributions, and PDF export.
- [`cyber-roi-calculator`](https://github.com/rafatyazdani/cyber-roi-calculator) — applies the same ALE foundation to **control investment ROI** (which controls earn their cost).
- [`security-risk-register`](https://github.com/rafatyazdani/security-risk-register) — applies the same ALE foundation to **per-finding prioritization** in an operational Excel workbook.
- Methodology source: **CRQ-F Framework** Phase 1 — Data Calibration.

---

## License

Apache 2.0 — free to use, adapt, and deploy in commercial contexts with attribution.

---

**Built by Rafat Yazdani** — CPA · CISSP · CISA · CCSK · AWS Certified
Security Strategy & Cyber Risk Quantification — Accenture Security

> Methodology backbone: **CRQ-F Framework** (Cyber Risk Quantification — Financial).
> Portfolio overview: [github.com/rafatyazdani](https://github.com/rafatyazdani) — Methodology · Quantification math · Consulting products · Governance & maturity · Operations · Board output.

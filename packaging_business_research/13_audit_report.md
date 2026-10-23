# Audit Report: Packaging Business Research Pack (files 01–12)

Audit date: **2026-10-23**. Scope: every file in `packaging_business_research/`, the formulas in `08_financial_model.xlsx`, and the evidence behind each claim. Method: re-read every file, recalculated the workbook in LibreOffice, traced each key number to its source, and benchmarked the assumptions against new Nepal and India evidence gathered for this audit.

**Bottom line:** v1 had **4 high-severity defects**: two formula/logic errors, an unbenchmarked price set, and a missing tax cost. It also had **6 medium** gaps in evidence and compliance. The formula and logic defects are fixed in `08_financial_model.xlsx` **v2**. The evidence gaps are partly closed by new research and are otherwise flagged. The v1 verdict of *conditional go, no machinery, staged pilot* **still stands**. Benchmarking against Indian small-run suppliers shows the v1 *product mix and offer design* left a lot of margin unused. That finding is the basis of the new strategy in `14_business_strategy.md` and `15_strategy_model.xlsx`.

## 1. Findings

Severity: **H** = changes the decision or key numbers; **M** = material gap; **L** = presentation or consistency.

| ID | File | Finding | Sev. | Effect | Status |
|---|---|---|---|---|---|
| A1 | 08, 07 | **Founder design time double-counted.** v1 deducted "internal design labour" (NPR 500–9,000 per order, about 16% of revenue) as a *cash* supplier cost. It then also paid the founder a NPR 15,000 salary from month 7. The same person was paid twice, so cash burn was overstated and the margin understated. | H | Gross margin understated by about 7 percentage points. | **Fixed v2:** toggle `design_cash` (default 0). Design time is covered by founder pay. |
| A2 | 08, 09 | **Unrecoverable VAT missing.** An unregistered firm cannot reclaim the 13% VAT on supplier bills. v1 treated supplier costs as VAT-free. | H | Supplier cost +7.8% (60% of spend VAT-billed, an assumption). | **Fixed v2:** `vat_reg` and `vat_share` inputs. VAT leakage applied in the engine and in Break_Even. |
| A3 | 08 | **Simple-average AOV and margin.** v1 averaged the six products equally, so rigid boxes (NPR 42k orders, 5% of real demand) moved AOV as much as kraft bags. | H | AOV overstated (NPR 28,242 → 26,719). | **Fixed v2:** mix-weighted (bags 25%, cartons 20%, mailers 20%, bakery 20%, stickers 10%, rigid 5%; the weights are assumptions). |
| A4 | 07, 02 | **Selling prices never benchmarked.** Indian small-run suppliers list printed mono cartons at ₹33 (NPR 53) from 10 pieces, printed mailers at ₹30–33, and logo kraft bags at ₹8.5–15 per piece at 500 (Printo, TradeIndia). v1 priced folding cartons at NPR 150 per unit at 100 pieces. After an estimated landed cost of about NPR 90 (unverified duty), v1's short-run carton prices are about 1.5–2× the Indian landed benchmark. The NPR 9,000 die and plate charge makes local offset uneconomic below about 250 units. | H | v1's short-run offer was likely uncompetitive. A cheaper sourcing route was missed. | **Addressed:** `15_strategy_model.xlsx` Sourcing_Decision and the new offer ladder. Needs quotes. |
| A5 | 08 | **Break-even ignored VAT leakage** after the v2 change (internal review): the contribution ratio showed 25.6% while the engine actually delivered 20.3%. | M | Break-even understated. | **Fixed:** formula now applies the leakage multiplier. |
| A6 | 08 | **Year-2 jump.** "15% growth" is applied to the month-12 run-rate × 12, so Year-2 revenue is 2.4× Year 1. The label understates the jump. | M | Three-year view looks better than it should. | Flagged. Treat Years 2–3 as illustrative only. |
| A7 | 09 | **E-commerce Act 2025 missing.** Businesses selling via Facebook, Instagram, TikTok or WhatsApp must register. Fines are up to NPR 2,00,000 (Kathmandu Post, Jul 2025). 1,530 applied and 345 were approved by Aug 2026. | M | Compliance gap for an Instagram-led seller. | **Added** to the 09 checklist (below). |
| A8 | 02, 04 | **Market sizing had no anchor.** New data: 170,632 establishments in the Valley (2018 Economic Census, all sectors). About 10,000 online SMEs nationally (self-reported, MIT Solve). Nepal Can Move has 1,000+ merchants; Pathao Commerce signed 600+ in two weeks. More than 90% of online merchants are in the Valley (Kathmandu Post, Aug 2026). | M | TAM and SAM are still estimates, but now have anchors. | Partly closed (see 14 §2). |
| A9 | 03 | **Competitor list thin; two direct rivals missed.** Prime Felt & Agro (Kathmandu; in-house design plus manufacturing partner plus private label; self-described "largest custom packaging wholesaler") and Packaging Product (Lazimpat; "custom packaging and design services") both run Model A. Chandrama Packaging and Infinity Print Nepal were also missed. | M | v1 understated direct competition for Model A. | **Added** to 03. |
| A10 | 05, 08 | **Machinery list had cutters but no colour label printer.** Epson ColorWorks C4050 is listed at ₹1,52,500 (about NPR 2.44 lakh, about NPR 3.19 lakh landed, unverified). | M | Option missing. | **Added.** Break-even analysis says do not buy (payback is decades at pilot volume). |
| A11 | 08 | Delivery assumed NPR 400–800 per order and own transport NPR 3,500/month. Third-party logistics in Kathmandu now charge NPR 70–80 per delivery (Kathmandu Post, Aug 2026). | M | Variable cost overstated. | Used in the strategy model. 08 left as is (conservative). |
| A12 | 08, 10, 02 | Funnel inconsistency: the Assumptions sheet says "~60 contacts/mo × ~10% × 30–40%". Files 02 and 10 say about 80 contacts/month × 15%. | L | Confusing. | Flagged; 10's numbers govern. |
| A13 | 04 | Prospect database has segments only, with no named businesses. | L | Unchanged. | Partner and channel prospects added (see 14). Named list remains a week-1 task. |
| A14 | 02, 12 | Customs: v1 said the duty was "not found". The FY 2083/84 budget cut customs slabs from 11 to 7 and lowered rates on 273 items (Himalayan Times). The HS 4819 rate is still not confirmed. | L | Still unverified. | Flagged; a clearing-agent quote is required. |
| A15 | 08 | Fractional customers in the downside cases (multiplier 0.5) are acceptable for expected values, but they are not whole orders. | L | None. | Noted. |

Checks that **passed**: budget reconciles to NPR 5,00,000; Config C correctly exceeds the ceiling; cash identity holds in every engine; no formula errors after recalculation; stress tests run independently (a v1 late-binding bug pointing every sheet at the last engine was fixed before the v1 commit).

## 2. Effect of the fixes on the v1 Base case (12 months)

| Metric | v1 | v2 (fixed) | Why |
|---|---|---|---|
| AOV | NPR 28,242 | NPR 26,719 | Mix-weighted (A3) |
| Gross margin used | 24.8% | 31.6% before VAT leakage | Design time no longer double-counted (A1) |
| Contribution margin | 18.8% | **20.3%** | A1 partly offset by VAT leakage (A2) |
| Revenue | 20.97 lakh | 19.84 lakh | AOV change |
| Contribution | 3.94 lakh | 4.02 lakh | |
| Op. profit after founder pay | −0.86 lakh | −0.78 lakh | |
| Minimum cash (founder paid from M7) | −37,735 | **−21,949** | Still breaches zero |
| Break-even sales (before founder pay) | 1.73 lakh/mo | 1.60 lakh/mo (6 orders) | |
| Break-even sales (with founder pay) | 2.53 lakh/mo | 2.34 lakh/mo (9 orders) | |
| Operating break-even month | 7 / 9 | 7 / 9 | unchanged |

**Conclusion of the audit:** the v1 plan *as designed* (sell custom-printed packaging at mid-size MOQs through local factories) is still marginal. It is cash-negative if the founder draws a salary, and it fails in the downside and in 8 of 9 stress tests. The fixes did not change that. What changes the picture is **how** the business sells and sources (see 14 and 15).

## 3. Compliance addition (to `09_legal_and_compliance_checklist.md`)

| # | Requirement | Authority | Detail | Status | Source |
|---|---|---|---|---|---|
| 15 | E-commerce registration | Dept. of Commerce, Supplies and Consumer Protection | E-commerce Act 2025: online sellers, including those on Facebook, Instagram, TikTok or WhatsApp, must register. Fines are up to NPR 2,00,000 per the July 2025 report; unlisted platforms face NPR 5,000–20,000 per the Aug 2026 report. | ✔ (news) | https://kathmandupost.com/money/2025/07/20/unregistered-online-sellers-now-illegal-in-nepal-under-new-e-commerce-law |
| 16 | VAT threshold watch | IRD | The strategy base case reaches NPR 23 lakh in year 1, below the NPR 30 lakh services/mixed threshold. Year 2 will cross it. Decide early with a CA whether to register voluntarily (see 14 §6). | ✔ (secondary) | udhamsathi.com |

## 4. What remains unverified (unchanged priority)
1. Any written supplier quotation (Nepal or India).
2. The HS 4819 duty rate and India GST/export treatment on small consignments.
3. Customer willingness to pay. No interviews have been done.
4. Named prospect list.
5. Registration route and VAT classification (CA meeting).

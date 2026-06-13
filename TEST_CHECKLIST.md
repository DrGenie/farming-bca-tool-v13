SOIL CRC BCA Tool - v13 Test Checklist
======================================

Automated verification (jsdom harness, run against the delivered files)
----------------------------------------------------------------------
Calculation parity (verify.js): the sample workbook + default settings reproduce
the v12 results exactly. Full metric snapshot diffed before/after every change:
IDENTICAL.

Headline figures confirmed unchanged from v12:
- Top treatment: Manure (T11) - NPV $6,639.01/ha, BCR 2.056, GPM 51.4%, ROI 105.6%
- Control: Control (T00) - NPV $4,094.41/ha, BCR 1.627
- 12 treatments ranked; sheet "BCA Trial Data"; 48 data rows; 0 validation notes.

Functional suite (test_v13.js) - 24 checks, all passing:
[PASS] progress stepper present (8 steps)
[PASS] ready indicator starts in "waiting"
[PASS] sample data loads; analysis runs; 12 treatments ranked
[PASS] top treatment Manure NPV 6639.01 (calculation parity)
[PASS] ready indicator turns "ready" after a clean load
[PASS] result interpretation populated (ranks highest / NPV / BCR / ROI)
[PASS] chart data tables populated (5 accessible figures)
[PASS] calculation-audit active schedule populated
[PASS] progress strip advances (>=5 steps done)
[PASS] assistant shows exactly 4 primary chips
[PASS] assistant shows exactly 8 secondary chips under "More prompts"
[PASS] primary chip dispatches a backend request and renders the answer
[PASS] "More prompts" chip dispatches (panel-level delegation fix)
[PASS] run-first guard: no backend call when analysis not run
[PASS] run-first guard: shows "upload data and run the BCA first" message
[PASS] ChatGPT button disabled when AI access = none
[PASS] Copilot button disabled when AI access = none
[PASS] ChatGPT button enabled after selecting "I have access to ChatGPT"
[PASS] ChatGPT click opens chatgpt.com
[PASS] Copilot button enabled after selecting "I have access to Copilot"
[PASS] Copilot click opens copilot.microsoft.com
[PASS] Copy prompt builds and stages the full prompt (ranking + settings + details)

Spreadsheets (recalc.py via LibreOffice)
- BCA_template.xlsx: sheets "BCA Data" + "Data_Dictionary"; includes
  "Cost of amendment_per_ha" (col 15); empty template; 0 recalculation errors.
- BCA_Trial_data.xlsx: amendment = 0; 0 negative numeric cells; Total Farm Costs
  recalculated as a live SUM of the tool's 17 components; 48 formulas; 0 errors.
  Workbook totals match the tool's internal sums (T00 $846.00, T11 $813.97,
  T01 $812.08 per ha).

Manual checks recommended before go-live
-----------------------------------------
Data
- [ ] Download template button downloads BCA_template.xlsx.
- [ ] Upload a completed workbook; confirm summary, preview and validation show.
- [ ] Load sample data; confirm "Ready to analyse" turns green.
- [ ] Project details start blank and require user input (no pre-filled defaults).

Settings
- [ ] Change grain price, analysis years, discounting mode, rates, switch year;
      click Apply settings; confirm the schedule label updates.

Results
- [ ] Run analysis; confirm metric cards, ranking, interpretation and audit panel.
- [ ] Compare a treatment against the control; check the difference-vs-control.
- [ ] Switch to side-by-side; pick two treatments; confirm the comparison table.
- [ ] Same-treatment selection shows the warning.
- [ ] All five charts render and have matching data tables.

Sensitivity
- [ ] Edit the three scenarios; click Update sensitivity; confirm the table.

Report
- [ ] "Description of Project:" appears as a bold label; the description text is
      not bold.
- [ ] Select "I have access to ChatGPT"; the ChatGPT button enables; clicking it
      copies the prompt and opens ChatGPT. Repeat for Copilot.
- [ ] Generate local summary; confirm draft-text labelling and disclaimer.
- [ ] Download Word report; open PDF-ready report; confirm charts and tables.

Assistant
- [ ] With no analysis run, a result question replies "upload data and run the
      BCA first".
- [ ] After running, the four primary chips and the eight "More prompts" chips
      all return answers grounded in the live result.
- [ ] Long answers scroll independently; the input stays fixed at the bottom.
- [ ] On a phone, the panel is a near full-screen sheet and does not cover the
      core controls underneath when closed.
- [ ] No API key appears anywhere in the browser (view source / network).

Accessibility / mobile
- [ ] Keyboard-only: tab through upload, settings, run, compare, report.
- [ ] Visible focus states throughout; skip link works.
- [ ] Tables scroll horizontally on a narrow screen without breaking layout.
- [ ] Screen reader announces upload status, ready state and current step.

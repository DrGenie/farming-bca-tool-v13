SOIL CRC Benefit-Cost Analysis Tool - Changelog
================================================

Version 2026.13 (v13)
---------------------
Summary: Improved accessibility, result interpretation, assistant layout, report
clarity, validation messages and mobile usability. Scientific BCA calculations
preserved and verified against v12.

Calculation integrity
- No change to any calculation. PV of benefits, PV of costs, NPV, BCR, gross
  profit margin, ROI, difference versus control, treatment ranking, control and
  side-by-side comparison, sensitivity, discounting modes and cost-component
  summation are byte-for-byte identical to v12.
- Verified automatically: the same sample workbook and default settings produce
  the same results as v12 (for example, the top treatment "Manure" at an NPV of
  $6,639.01/ha and a BCR of 2.056, with the control "Control" (T00) at an NPV of
  $4,094.41/ha). A before/after snapshot of every metric was diffed and is
  identical.

New: formula transparency and calculation audit
- Added a "How the calculations work (calculation audit)" panel in Results,
  listing plain-language formulas and units:
  - Annual gross benefit = average yield (t/ha) x grain price ($/t)
  - Annual direct cost = sum of the individual cost components ($/ha)
  - PV benefits / PV costs = discounted values over the analysis period
  - NPV = PV benefits - PV costs
  - BCR = PV benefits / PV costs
  - Gross profit margin (%) = (PV benefits - PV costs) / PV benefits x 100
  - ROI (%) = (PV benefits - PV costs) / PV costs x 100
  - Difference versus control = selected treatment NPV - control NPV
- States explicitly that direct costs are summed from the individual workbook
  components (not a single pre-totalled column) and lists those components.
- Shows the active discount schedule and assumptions after each run.

New: result interpretation
- Added a "What these results suggest" panel that turns the live numbers into
  plain language for NPV, BCR, ROI, the comparison with the control and why the
  top treatment ranks where it does. Uses careful language ("suggests", "under
  these assumptions", "based on the uploaded data") and a check-assumptions
  caveat. Not presented as financial or agronomic advice.

Improved: data validation and warnings
- Validation messages now state what is wrong, where the problem likely is, how
  to fix it, and whether the analysis can continue. Covers wrong sheet, missing
  BCA Data sheet, missing/duplicated control, inconsistent treatment names,
  missing or non-numeric yield, missing direct-cost components and non-numeric
  cost cells.
- Added a "Ready to analyse" indicator that turns green once essential checks
  pass, amber when results can be produced but need review, and red when a
  control or yield is missing.

Improved: layout and user flow
- Added a step-by-step progress strip across the top: Upload data, Validate
  workbook, Project details, Apply settings, Run analysis, Compare treatments,
  Run sensitivity, Generate report. The current step is highlighted and
  completed steps are ticked.
- Units shown beside inputs and outputs ($/t, years, %, $/ha, t/ha).

Improved: accessibility
- Skip link, ARIA live regions, visible and programmatic labels retained.
- Charts are now accompanied by accessible data tables and plain-language
  summaries so they can be read in greyscale or by a screen reader (colour is
  never the only cue).
- Ranking table headers use scope="col". Progress and ready states are announced
  via aria-live / aria-current.

Improved: Analysis Assistant
- Simplified to four primary quick buttons (Explain current result, Explain for
  a farmer, Best value for money, Draft report narrative). The eight secondary
  prompts moved into a collapsed "More prompts" section.
- Larger, independently scrollable response area; input fixed at the bottom;
  near full-screen sheet on mobile.
- If the analysis has not been run, result questions now answer locally with:
  "Please upload data and run the BCA first so I can interpret the current
  results." General questions (what the metrics mean, etc.) still work.
- Specific backend error messages retained: backend unavailable, API key missing
  or invalid, quota or rate limit reached, model unavailable, CORS problem,
  question too long, timeout and empty response. API keys never appear in the
  browser; the frontend only calls the Worker.

Improved: report generation
- Report section headings (for example "Description of Project:") render as bold
  labels with a colon, with the user-written text in a separate, non-bold
  paragraph.
- Locally generated or AI-generated narrative is labelled as draft text to be
  reviewed, with a disclaimer that outputs do not replace professional advice.

Trust and transparency
- Added a "What this tool can and cannot do" section to the Introduction.

Workbook and template changes
- The downloadable template is BCA_template.xlsx and includes the
  "Cost of amendment_per_ha" column (Cost of Amendments).
- The sample/trial workbook is BCA_Trial_data.xlsx. The cost of amendment is set
  to zero, previously negative figures have been corrected, and the
  "Total Farm Costs_per_ha" column is recalculated as a live sum of the same
  direct-cost components the tool uses, so the workbook total matches the tool's
  internal direct-cost total.
- The tool reads cost components directly from the workbook; the total farm cost
  column is shown for reference only and does not affect the analysis.

Compatibility
- Still a static site: open index.html or deploy the folder to GitHub Pages. No
  build step. The optional Analysis Assistant uses a Cloudflare Worker that
  holds the model key as a server secret (never in the frontend).

SOIL CRC Benefit-Cost Analysis Tool
===================================

Version 2026.13 (v13). A web-based benefit-cost analysis decision aid for
agricultural production trials (for example soil amendments on wheat). Open
index.html in a browser or deploy the whole folder to a static host such as
GitHub Pages. No build step or server is required for the tool itself. The
optional AI Analysis Assistant uses a small Cloudflare Worker (see worker/).

What's new in v13 (2026.13)
---------------------------
Accuracy first: every BCA calculation is unchanged and was verified to match
v12 exactly (see CHANGELOG.md and TEST_CHECKLIST.md). On top of that:
- "How the calculations work (calculation audit)" panel with plain-language
  formulas, units, and the list of cost components that are summed.
- "What these results suggest" panel that interprets NPV, BCR, ROI and the
  control comparison in plain language, with check-assumptions caveats.
- Friendlier, actionable validation messages (what is wrong, where, how to fix,
  whether analysis can continue) plus a "Ready to analyse" indicator.
- Step-by-step progress strip across the workflow with the current step shown.
- Accessibility: accessible data tables and plain-language summaries beside
  every chart, scoped table headers, announced status and current step.
- Simplified Analysis Assistant: four primary quick buttons with the rest under
  a "More prompts" section, a larger scrollable response area, a near
  full-screen sheet on mobile, and a clear "run the BCA first" message when no
  analysis has been run yet.
- "What this tool can and cannot do" section in the Introduction.
- Report section headings render as bold labels (for example
  "Description of Project:") with the user text in a separate, non-bold block.

What's in this release
----------------------
1. New official template, BCA_template.xlsx, which includes the
   "Cost of amendment_per_ha" column. The Data tab "Download template" button
   now serves this file.
2. Recalculated trial dataset, BCA_Trial_data.xlsx. Negative figures were
   removed, the cost of amendment is set to zero, and "Total Farm Costs_per_ha"
   is now a live formula that sums the same direct-cost components the tool
   uses, so the workbook total always matches the tool's internal calculation.
   This file is also what "Load sample data" loads.
3. Wider column and sheet-name support. The tool now reads both the new and the
   original column names (Crop_yield_ton_per_ha, Seed cost_per_ha,
   Marketing_total cost_per_ha and their earlier equivalents) and accepts the
   "BCA Data" / "BCA Trial Data" sheet names as well as the original.
4. Project details are no longer pre-filled. The Name of Project, Collaborators,
   Funding Agency, Project Summary and Methodology fields start empty with
   guidance placeholders, so each user enters their own details.
5. ChatGPT and Copilot report buttons verified. They open chatgpt.com and
   copilot.microsoft.com in a new tab and copy the full prepared prompt to the
   clipboard, with a manual-copy fallback if the browser blocks automatic copy.
6. New AI Analysis Assistant (assistant.js + worker/). A domain-grounded chat
   assistant that reads the live analysis and helps interpret results, compare
   treatments, draft report text and plan sensitivity checks. The model key is
   held only as a Worker secret and is never in the frontend or the repository.

Files
-----
- index.html ............ Main application (7 tabs + AI Assistant)
- styles.css ............ Stylesheet (includes the assistant styling)
- app.js ................ Application logic, parsing, charts, report generation
- assistant.js .......... AI Analysis Assistant frontend (talks only to Worker)
- guide.html ............ Full user guide
- BCA_template.xlsx ..... Official data-entry template (BCA Data + Data_Dictionary)
- BCA_Trial_data.xlsx ... Validated sample dataset (also loaded by "Load sample data")
- logo_uon_white.png / logo_soil_white.png ... White logos (dark header/footer)
- logo_university_newcastle.jpg / logo_soil_crc.png ... Badge logos (report)
- logo1.png / logo2.png .. Legacy logo copies (retained for compatibility)
- worker/ ............... Cloudflare Worker backend for the AI Assistant

How the AI Assistant is grounded
--------------------------------
The assistant is domain-grounded, not fine-tuned. It is steered by a fixed
system prompt and a benefit-cost knowledge base inside the Worker, and it is
sent the live tool state (ranking, metrics, settings, project info) with every
request. It uses only those values, does not invent numbers, and does not learn
or retain anything from user questions. Answers are interpretation support, not
financial or agronomic advice, and are marked for review when added to a report.

Deploying
---------
1. Tool: push this folder to a GitHub repository and enable GitHub Pages. The
   tool works immediately; the assistant shows "Backend not set" until step 2.
2. Assistant: deploy the Worker in worker/ and set the GEMINI_API_KEY secret,
   then confirm the Worker URL in assistant.js. Full steps are in
   worker/README_DEPLOY_CHATBOT.md. Set the Worker's ALLOWED_ORIGINS to your
   GitHub Pages origin.

Notes
-----
- Scientific calculations are unchanged: NPV, BCR, gross profit margin, ROI and
  the discounting schedule work exactly as before. Direct costs are summed
  internally from the workbook component fields.
- Word reports export as Word-compatible documents with publication-style
  tables, chart images and a generated date/time stamp.

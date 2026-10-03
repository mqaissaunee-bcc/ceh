# CEH v13 — interactivity and light/dark fixes (Oct 2026)

Based on the live files at mqaissaunee-bcc.github.io/ceh/ (not the older Project copies).
Replace the 20 ceh_moduleN.html files; index.html and glossary.html are unchanged.

## All 20 modules (shared chassis + Lab Kit)
- Terminals, demo outputs and Lab Kit terminals now stay readable in light mode (were dark text on dark background).
- Footer/faint text darkened to meet 4.5:1 in light mode; dark-theme danger button text fixed.
- Native dropdowns and checkboxes follow the theme.
- Lab Kit builder: a correct command now counts toward a challenge even if the student didn't click the challenge first.
- Quiz options and clickable lab cards work from the keyboard (Tab + Enter/Space).
- New CSP-safe `data-change` hook for dropdowns; `CEH.printElement()` helper for certificates.

## Module-specific
- M5: CVSS calculator rebuilt — dropdowns were dead (inline handler error); now all 8 v3.1 base metrics, official equations, vector string.
- M5, M10: page now resumes at the last section instead of jumping back to the start.
- M1, M4, M5, M6, M7, M10: hard-coded colours converted to theme colours (resource boxes, certificates, results panels, lab pop-ups).
- M4, M5, M7: "Download certificate" now opens Print / Save as PDF. M4 "Continue to Module 5" now goes there. M7 "Share" opens LinkedIn.
- M6, M7: lab cards no longer show a "this would open a lab" pop-up; they scroll to the working Lab Kit.

## Verification
HTML5 parse (0 errors, all files), node --check on every script, headless-Chromium audit clicking every action and control
in both themes (0 script errors, 0 contrast failures), Lab Kit solver test of every challenge and terminal objective,
CVSS results checked against 8 known vectors, line diff confirming section/quiz/lab counts unchanged.

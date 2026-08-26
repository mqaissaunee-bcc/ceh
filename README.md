# CEH v13 Interactive Course

An interactive study companion for the **Certified Ethical Hacker (CEH) v13**
curriculum: 20 self-contained browser modules covering the full attack
lifecycle, each pairing offensive technique with the defensive controls that
counter it. Built as standalone HTML — the modules work offline, embed in
Canvas or any LMS, and remember where each student left off.

> **Authorized testing only.** Every technique in this course is taught for
> defensive education and authorized assessment. Using these tools against
> systems you do not own or lack written permission to test is illegal in most
> jurisdictions. This is an independent study companion and is **not** affiliated
> with or endorsed by EC-Council.

---

## What's here

| Path | What it is |
|------|------------|
| `index.html` | Course hub — module grid, progress tracking, and cross-module search |
| `ceh_module1.html` … `ceh_module20.html` | The 20 course modules |
| `glossary.html` | Course-wide glossary (74 terms), filterable, linked to modules |
| `cheatsheet-*.html` | Printable one-page cheat sheet per module |
| `_build/` | The build system that generates everything above (see below) |

Open `index.html` in any modern browser to start. No server, build step, or
internet connection is required to **use** the course.

## Features

- **20 modules** spanning the full CEH v13 domain list, from reconnaissance and
  scanning through web apps, wireless, cloud, and cryptography.
- **Per-module progress** with resume — each module remembers your last section.
- **Glossary tooltips** — key terms are underlined throughout; hover, tap, or
  focus to see the definition. All definitions come from a single source.
- **Course glossary page** — every term in one place, with a live filter and a
  link back to the module where it's taught.
- **Cross-module search** on the hub — find any module, section, or term and
  jump straight to it.
- **Cross-reference links** — "Module N" mentions link to that module.
- **Printable cheat sheets** — one per module: sections, commands, and key terms,
  print-optimized (great for exam review or a PDF export).
- **Interactive assessments** — each module ends with a self-scoring quiz.
- **Notes** — take per-section notes and export them as Markdown.
- **Accessible & themeable** — keyboard navigation, ARIA roles, screen-reader
  support, dark/light theme, and adjustable text size.
- **Self-contained & CSP-safe** — no inline handlers, no third-party runtime
  dependencies, no tracking.

## Hosting on GitHub Pages

Because the course is static HTML, GitHub Pages serves it as-is:

1. Push this repository to GitHub.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*,
   choose your branch (e.g. `main`) and the `/ (root)` folder, and save.
4. After a minute, your course is live at
   `https://<your-username>.github.io/<repo-name>/`.

The hub is `index.html`, so that URL loads the course directly.

> Note: module cards and study-aid links open in a new tab by design, so a
> student never loses their place. That works the same on GitHub Pages.

## Embedding in Canvas (or another LMS)

Each module is a single self-contained file. To embed one:

- **Upload + link:** upload the module `.html` to your course files and link to
  it, or
- **Iframe:** host the files (e.g. on GitHub Pages) and embed a module with an
  `<iframe>` pointing at its URL.

Progress and notes are stored in the browser's local storage, per student, per
device.

## Rebuilding from source

You only need this if you want to **change** the course. Editing content and
regenerating is done through the pipeline in `_build/`.

### Requirements

- **Python 3** with `lxml` (`pip install lxml`)
- **Node.js** with `jsdom` (`npm install jsdom`) — only needed to run the validator

### Where content lives

Module prose lives in `_build/content/<slug>/*.html` — one file per section
(e.g. `_build/content/sqli/types.html`). The 20 slugs are: `intro`,
`footprinting`, `scanning`, `enumeration`, `vuln-analysis`, `system-hacking`,
`malware`, `sniffing`, `social-engineering`, `dos`, `session-hijacking`,
`evasion`, `web-servers`, `web-apps`, `sqli`, `wireless`, `mobile`, `iot-ot`,
`cloud`, `crypto`.

The glossary — the single source of truth for tooltips, the glossary page,
search, and cheat sheets — is `_build/glossary.py`.

### Build commands

Run these from inside `_build/`, in order:

```bash
cd _build

# 1. sync the glossary into the shared runtime (run if glossary.py changed)
python3 inject_glossary.py

# 2. build all 20 modules (applies content, tooltips, cross-refs, chassis)
python3 -c "import build_all; build_all.main()"

# 3. build the course hub (with the cross-module search index)
python3 build_index.py

# 4. build the glossary page
python3 build_glossary.py

# 5. build the printable cheat sheets
python3 build_cheatsheets.py

# 6. validate (modules + index + glossary should report "All modules clean")
node validate.js build
```

Steps 2–5 write their output into `_build/build/`. Copy the generated
`build/*.html` up to the repository root to publish them, and copy
`_build/content/` through unchanged.

### How the pipeline fits together

- `chassis.css` / `chassis.js` — the shared design system and runtime every page
  is built on (theme, navigation, progress, notes, tooltips, search deep-linking).
- `build_module.py` — assembles one module: injects the section content, runs the
  enrichment passes, and wraps it in the chassis.
- `enrich.py` — the two automatic passes: glossary-tooltip wrapping and
  cross-reference linking. Both operate on the parsed DOM, so tag structure is
  never corrupted.
- `glossary.py` + `inject_glossary.py` — the canonical term list and the injector
  that syncs it into `chassis.js`.
- `build_all.py` — drives a full rebuild of all 20 modules.
- `build_index.py`, `build_glossary.py`, `build_cheatsheets.py` — generate the
  hub, glossary page, and cheat sheets.
- `validate.js` — a jsdom harness that boots every page and checks for dead
  navigation, orphaned sections, CSP violations, accessibility gaps, and console
  errors.

## Project layout

```
.
├── index.html                 # course hub
├── ceh_module1.html … ceh_module20.html
├── glossary.html
├── cheatsheet-<slug>.html     # 20 printable cheat sheets
├── README.md
├── LICENSE
├── .gitignore
└── _build/                    # build system (source of everything above)
    ├── chassis.css, chassis.js
    ├── glossary.py, inject_glossary.py
    ├── enrich.py
    ├── build_module.py, build_all.py
    ├── build_index.py, build_glossary.py, build_cheatsheets.py
    ├── validate.js
    ├── ceh_module*_*.html      # native module source
    ├── patch_m*.js             # per-module runtime patches
    └── content/<slug>/*.html   # section prose (edit here)
```

## License

See [LICENSE](LICENSE). Course content is provided for educational use.

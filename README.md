# SQA Release Standards — Bad vs Good Examples

> A living reference guide for QA and Development teams — in any organization: **25 categories** of common UI/UX and functional basics, each shown as a side-by-side **Bad (rejected) vs Good (passes review)** comparison — so nothing obvious slips through again.

**Status:** Living document (v1.0) · Framework-agnostic, works for any team

---

## 📋 Table of Contents

- [Why this exists](#-why-this-exists)
- [What's inside](#-whats-inside)
- [Preview](#-preview)
- [Getting started](#-getting-started)
- [Project structure](#-project-structure)
- [Tech stack](#-tech-stack)
- [Adding a new category](#-adding-a-new-category)
- [Contributing](#-contributing)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🎯 Why this exists

QA and Development often miss the same "obvious" things — not because of skill
gaps, but because **there's no shared, written definition of "basic."**

- Developers sometimes ship a feature without covering edge cases that seem too
  small to mention.
- QA sometimes tests the happy path and misses the same basic checks under time
  pressure.

This site is a single source of truth: a checklist of what "done" and "tested"
actually look like, category by category, with real mockups instead of abstract
rules.

> **Prevention is faster than fixing.**

## 🗂 What's inside

25 categories, each with a **Bad** and a **Good** mockup plus the concrete reasons
why:

| # | Category | # | Category | # | Category |
|---|---|---|---|---|---|
| 01 | Register | 10 | Errors | 19 | Currency / Amount |
| 02 | Login | 11 | Input field | 20 | Tooltip |
| 03 | Email | 12 | Mobile Number | 21 | Links |
| 04 | Password | 13 | Searching | 22 | Upload |
| 05 | OTP | 14 | Dropdown | 23 | Download |
| 06 | Captcha | 15 | Checkbox | 24 | Header |
| 07 | Forgot Password | 16 | Radio | 25 | Footer |
| 08 | Buttons | 17 | URL | | |
| 09 | Delete Button | 18 | Text area | | |

Every category follows the same format:

- ❌ **Bad — Will be rejected**: a realistic mockup of the mistake, plus a bullet
  list of exactly why it fails review.
- ✅ **Good — Passes review**: the corrected version, plus a bullet list of what
  makes it acceptable.

## 🖼 Preview

> Open `SQA_Bad_vs_Good_Examples.html` in any modern browser — no build step, no
> server required.

The page is a single scrollable document with a sticky side/top navigation that
jumps to any of the 25 categories and highlights the active section as you scroll.

## 🚀 Getting started

No dependencies, no build tools — it's a static HTML file.

```bash
# Clone the repository
git clone https://github.com/<your-org>/<your-repo>.git
cd <your-repo>

# Open directly in a browser
open SQA_Bad_vs_Good_Examples.html      # macOS
xdg-open SQA_Bad_vs_Good_Examples.html  # Linux
start SQA_Bad_vs_Good_Examples.html     # Windows
```

Or serve it locally (optional, useful if you later add fetch-based features):

```bash
npx serve .
```

## 🏗 Project structure

```
.
├── SQA_Bad_vs_Good_Examples.html   # Main page (HTML + CSS + JS)
├── CLAUDE.md                       # Instructions for AI-assisted contributions
└── README.md                       # You are here
```

> This currently ships as a single self-contained HTML file by design — easy to
> share, upload, or open with zero setup. See [`CLAUDE.md`](./CLAUDE.md) for the
> internal content pattern and conventions used throughout the file.

## 🧰 Tech stack

- **HTML5** — semantic sections, one per category
- **CSS3** — custom properties (design tokens) for a consistent Bad/Good color
  system, no framework/preprocessor
- **Vanilla JavaScript** — `IntersectionObserver` for scroll-spy navigation, a
  small cosmetic password show/hide toggle
- **Fonts** — [Fraunces](https://fonts.google.com/specimen/Fraunces) (display),
  [IBM Plex Sans](https://fonts.google.com/specimen/IBM+Plex+Sans) (body),
  [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono) (labels)

No build step, no package manager, no runtime dependencies.

## ➕ Adding a new category

1. Pick a short, hyphenated `id` (e.g. `dark-mode`).
2. Add a nav entry pointing to it: `<a href="#dark-mode">...</a>`.
3. Duplicate an existing `<section>` block as a template and adapt the copy, the
   bad/good mockups, and the note lists.
4. Keep section numbers sequential and update the "Category N of 25" labels
   across the file if you're inserting in the middle.
5. Verify nav links and section IDs still match one-to-one (see command below).

```bash
# Sanity check: every nav link must resolve to a real section
grep -oP 'href="#\K[^"]+' SQA_Bad_vs_Good_Examples.html | sort > /tmp/nav.txt
grep -oP '<section class="section" id="\K[^"]+' SQA_Bad_vs_Good_Examples.html | sort > /tmp/sections.txt
diff /tmp/nav.txt /tmp/sections.txt && echo "✅ nav and sections match"
```

Full conventions (design tokens, content pattern, accessibility notes) live in
[`CLAUDE.md`](./CLAUDE.md).

## 🤝 Contributing

This is a living document meant to be maintained by whichever QA/Dev team adopts
it — fork it, rename it, and make it yours.

- Spot a "basic" that keeps getting missed in QA? Open an issue or a PR adding it
  as a new category.
- Found a category that's outdated or no longer relevant? Flag it — the goal is
  for this list to reflect what the team actually checks today, not a frozen
  standard.
- Keep bad-example mockups intentionally flawed (typos, broken states, etc.) —
  that's the teaching content, not something to "fix."

## 🗺 Roadmap

- [ ] Split into `index.html` / `styles.css` / `script.js` for easier diffs
- [ ] Add a search/filter across all 25 categories
- [ ] Add meta description + favicon for shareability
- [ ] Optional: generate category pages from a single JSON/data source

## 📄 License

No license specified yet — add one (e.g. MIT) if you want this open for reuse
by other teams/organizations, or mark it "Internal use only" if it's meant to
stay private to your org.

# CLAUDE.md

Guidance for Claude (or any contributor) working in this repository.

## Project overview

**SQA Release Standards — Bad vs Good Examples**

A single-page internal reference site for the QA team. It exists because developers
sometimes miss basic requirements and QA sometimes misses basic tests — this page is
the shared checklist that removes the ambiguity. Each of the **25 categories**
(Register, Login, Email, Password, OTP, Captcha, Forgot Password, Buttons, Delete
Button, Errors, Input field, Mobile Number, Searching, Dropdown, Checkbox, Radio,
URL, Text area, Currency/Amount, Tooltip, Links, Upload, Download, Header, Footer)
shows a **"Bad — Will be rejected"** mockup next to a **"Good — Passes review"**
mockup, with a bullet-point list of concrete reasons under each.

This is a living document. New categories get added as the team discovers more
"basics" that keep getting missed.

## Current file structure

```
SQA_Bad_vs_Good_Examples.html   ← everything: HTML + CSS + JS in one file (~3,500 lines)
```

Known limitation: this is currently a single monolithic HTML file. Content edits,
CSS edits, and JS edits all live in one diff-noisy file. **Prefer splitting** into
`index.html`, `styles.css`, `script.js` when doing any non-trivial work here, unless
the user explicitly wants to keep it a single portable file (some internal
tools/wikis only accept a single HTML upload — confirm before splitting).

## Design system (CSS custom properties)

Defined once in `:root`, do not hardcode colors elsewhere:

| Variable | Purpose |
|---|---|
| `--ink`, `--ink-soft` | primary text / headings |
| `--paper`, `--paper-pure` | backgrounds |
| `--line`, `--line-soft` | borders/dividers |
| `--muted`, `--muted-dark` | secondary text |
| `--bad`, `--bad-deep`, `--bad-tint`, `--bad-line` | "Bad" column theme (red) |
| `--good`, `--good-deep`, `--good-tint`, `--good-line` | "Good" column theme (green) |
| `--warn`, `--warn-tint` | warning/amber accents |
| `--display` | Fraunces (serif, headings) |
| `--body` | IBM Plex Sans (body text) |
| `--mono` | IBM Plex Mono (labels, kickers, numerals) |

**Do not add new colors as raw hex values.** If a new state is needed (e.g. a
"warning" mockup), add a variable to `:root` first.

## Content pattern — how a category section is structured

Every category is a `<section class="section" id="{short-id}">` with this shape:

```html
<section class="section" id="register">
  <div class="section-head">
    <div class="section-num">01<em>.</em></div>
    <div class="section-info">
      <div class="section-kicker">Category 1 of 25 · registration form</div>
      <h2 class="section-title">{One-line thesis statement about this category}</h2>
      <p class="section-lede">{1-2 sentence why-it-matters framing}</p>
    </div>
  </div>

  <div class="compare">
    <div class="col bad">
      <div class="col-head">{svg icon} Bad — Will be rejected</div>
      <div class="col-mockup">{fake UI showing the bad behavior}</div>
      <div class="col-notes">
        <ul class="note-list">{specific, concrete bullet reasons}</ul>
      </div>
    </div>
    <div class="col good">
      <div class="col-head">{svg icon} Good — Passes review</div>
      <div class="col-mockup">{fake UI showing the correct behavior}</div>
      <div class="col-notes">
        <ul class="note-list">{specific, concrete bullet reasons}</ul>
      </div>
    </div>
  </div>
</section>
```

Rules for content inside this pattern:

- **`id` must be short, lowercase, hyphenated**, and must exactly match its nav
  entry's `href="#id"` in `.nav-links` near the top of the file. Never add/rename a
  section id without updating the nav link in the same commit.
- **`section-num` is zero-padded** (`01`, `02`, … `25`) and must stay sequential with
  no gaps if you insert a new category in the middle — renumber everything after it.
- **Bad-column mockups intentionally contain typos/bugs** (e.g. "Reigster",
  "Cmpany", stale copyright years). This is deliberate teaching content, not a real
  bug in the site — never "fix" these by accident when editing other things nearby.
- **Notes must be specific and testable**, not generic advice. Compare:
  - ❌ "Improve error handling"
  - ✅ "Confirm-password mismatch only checked on Submit, not on blur"
- Every bullet in `.col.bad .note-list` should have a **directly corresponding**
  bullet in `.col.good .note-list` describing the fix, in the same order where
  possible, so a reader can visually pair them up.
- Icons are inline `<svg viewBox="0 0 20 20" fill="currentColor">` (checkmark for
  good, X for bad) — reuse the existing two icon paths already in the file rather
  than sourcing new icons, to keep the visual language consistent.

## Adding a new category (checklist)

1. Decide the short `id` (e.g. `dark-mode`) and the display name for the nav.
2. Add `<a href="#dark-mode"><span>26</span>Dark Mode</a>` to `.nav-links`.
3. Copy an existing `<section>` block as a template (`register` is the most complete
   example) and adapt it following the content pattern above.
4. Update `section-kicker` to `Category 26 of 26 · {topic}` **and update the "of 25"
   text everywhere else in the file** (hero copy, any other category counts).
5. Validate: every `nav-links a[href]` still has a matching `section[id]`, and vice
   versa (see verification command below).
6. If splitting into separate files, add the new section's markup to `index.html`
   only — no CSS/JS changes should be needed for a same-pattern addition.

## Accessibility notes / known gaps

- No `<img>` tags — all icons are inline SVG. Any SVG that is purely decorative
  should have `aria-hidden="true"`; only icons conveying unique information (rare
  here) need a `<title>` or `aria-label`.
- Bad-column mockups deliberately show broken/misspelled UI. Wrap `.col.bad
  .col-mockup` regions with something like `aria-label="Example of an incorrect
  implementation"` so assistive tech doesn't announce the demo content as if it
  were a real defect on this page.
- Only one `<h1>` (correct) and one `<h2>` per section (correct) — keep this
  heading hierarchy intact when adding sections; don't skip to `<h3>` for new
  sub-content without a reason.
- No `<meta name="description">` or favicon currently — add both if this page
  gets shared as a link outside the immediate team.

## JavaScript

Two small vanilla-JS behaviors live in a `<script>` at the end of `<body>`:

1. **Scroll-spy nav highlighting** via `IntersectionObserver` — highlights the nav
   link matching the section currently in view. `rootMargin: '-40% 0px -55% 0px'`
   controls when a section counts as "active"; adjust carefully, it's tuned to
   this page's section heights.
2. **Password show/hide toggle** for `.mock-pwd-wrap .eye` buttons — purely
   cosmetic (swaps a fake masked value for a fake plaintext value), not a real
   password field. Don't wire this to real form logic; the whole page is mockups.

Do not introduce a framework (React/Vue/etc.) for small additions like this — the
page is static and dependency-free by design. If the file is split into
`script.js`, keep it dependency-free unless a specific new feature requires
otherwise (discuss with the user first).

## Style conventions

- Indentation: 2 spaces, consistent throughout.
- Section dividers use the existing HTML comment banner style:
  ```html
  <!-- ============================================================
       SECTION NN — SHORT-ID
       ============================================================ -->
  ```
  Keep this before every `<section>` for scannability in a long file.
- Avoid adding new inline `style="..."` attributes where an existing utility class
  or CSS variable already covers the case — the file already has 240+ inline
  styles and that number should shrink over time, not grow.
- Fonts: `Fraunces` (display/serif), `IBM Plex Sans` (body), `IBM Plex Mono`
  (labels/mono numerals) — loaded from Google Fonts in `<head>`. Don't introduce a
  fourth typeface.

## Verification before committing

Run these checks after any edit that touches nav links or section ids:

```bash
# Every nav href must have a matching section id, and vice versa
grep -oP 'href="#\K[^"]+' index.html | sort > /tmp/nav_ids.txt
grep -oP '<section class="section" id="\K[^"]+' index.html | sort > /tmp/section_ids.txt
diff /tmp/nav_ids.txt /tmp/section_ids.txt && echo "OK: nav and sections match"
```

If you split the file, also check that `styles.css` and `script.js` are correctly
linked from `<head>`/end of `<body>` and that no inline `<style>`/`<script>` blocks
were left behind by accident.

## Non-goals

- This is not a real, functional web app — all forms, buttons, and links are
  static mockups for illustration. Do not wire up real backend calls, real
  validation logic, or real auth.
- Do not "fix" the bad-example content (typos, broken layout, stale dates) — that
  content being wrong is the entire point of the left-hand column.

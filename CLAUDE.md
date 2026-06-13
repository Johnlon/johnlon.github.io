# Project Instructions for Claude

## CV / cv.html — MANDATORY RULES

`cv.html` is a professional CV that can be downloaded as a PDF and sent to employers.
Accuracy is critical. The following rules apply WITHOUT EXCEPTION:

1. **Keep it current** — whenever new career information, skills, open source contributions,
   or role changes are added to the site, consider whether `cv.html` needs updating too.

2. **Always notify the user** — if you make or propose any change to `cv.html`, you MUST
   explicitly tell the user what you changed and why, in plain language.

3. **Always demand review** — after any change to `cv.html`, you MUST ask the user to
   read the CV carefully for accuracy before it is published. Use clear, direct language:
   > "Please review cv.html carefully for accuracy before I push this."

4. **NEVER commit or push `cv.html` without explicit human approval** — do not include
   `cv.html` in any commit until the user has confirmed they have reviewed it and are
   happy with it. This applies even if other files are being committed at the same time.
   Stage and commit `cv.html` separately, only after approval.

5. **No assumptions** — do not infer that a change is "obviously correct". Dates, titles,
   company names, and responsibilities must be confirmed by the user, not guessed.

These rules exist because an inaccurate CV sent to an employer causes real harm.

## CV Design Recipe — Screen (cv.html)

These settings define the dark terminal theme on screen. Maintain them when making changes:

- **Body font size:** 14px
- **Name heading:** 2.1rem
- **Profile / summary bullets:** 14px
- **Job body bullets, skills, open source bullets:** 13px
- **Section labels:** 11px mono, uppercase, green (#39ff7a), letter-spacing 0.12em
- **Job title h3:** 1.05rem, weight 700
- **Dates:** 12px mono, amber (#ffb830)
- **Max content width:** 860px
- **Nav:** "John Lonergan" left (green mono), links right (text colour, bold), "Download CV" green button
- **Nav links order:** Home · Bio · Blog · Projects · Download CV
- **Download button:** direct `<a href="/assets/john-lonergan-cv.pdf" download>` link — serves the committed PDF file
- **Section order (screen + print):** Profile → Experience → Technology → Open Source → Education

## CV Design Recipe — Print / PDF

These settings define the white professional PDF output. Maintain them when making changes:

- **Body font size:** 11px, line-height 1.55
- **Name heading:** 26px
- **Title line:** 11px mono, green (#1a6b3a)
- **Contacts:** 10px
- **Section labels:** 9px mono, uppercase, green (#1a6b3a), letter-spacing 0.15em
- **Job title h3:** 12px, weight 700, dark (#1a1a1a)
- **Job company:** 10.5px, green (#1a6b3a)
- **Dates:** 10px mono, amber-dark (#7a4e00)
- **Bullet text:** 10.5px, dark (#1a1a1a)
- **Colours:** white bg, #1a6b3a green accents, #7a4e00 dates, #1a1a1a body text
- **Page size:** A4, `@page { size: A4; margin: 16mm; }`
- **Skills grid:** 2-column in print

### Page break rules (print CSS)

```css
.cv-section        { margin-bottom: 18px; }            /* NO page-break-inside:avoid — allows sections to flow */
.cv-section-label  { page-break-after: avoid; }        /* label always stays with its content */
.job               { page-break-inside: avoid; }       /* each job stays together */
.job-header        { page-break-after: avoid; }        /* title never orphaned from bullets */
.job-body          { page-break-before: avoid; }       /* bullets never orphaned from title */
.summary-bullets   { page-break-inside: avoid; }
.edu-item          { page-break-inside: avoid; }
```

**Key rule:** Do NOT put `page-break-inside: avoid` on `.cv-section` — the Experience section
is too large to fit on one page and Chrome will push the whole section to the next page if
you do, leaving a blank gap. Let sections flow; keep individual jobs together instead.

## PDF — THE PRIMARY DOCUMENT

**`cv.html` is the content source of truth.** The PDF is generated from it using Puppeteer
(headless Chrome) and committed as a static file at `assets/john-lonergan-cv.pdf`.
The Download CV button serves this file directly.

**NEVER use html2pdf.js** — it captures the dark screen theme. The PDF must be generated
from the `@media print` CSS (white, professional).

### PDF Generation — Puppeteer (PREFERRED)

Puppeteer is installed at `c:\tmp\node_modules\puppeteer`. Script at `c:\tmp\gen-cv-pdf.mjs`:

```js
import puppeteer from 'puppeteer';
const browser = await puppeteer.launch({ args: ['--no-sandbox', '--disable-setuid-sandbox'] });
const page = await browser.newPage();
await page.goto('http://localhost:4000/cv.html', { waitUntil: 'networkidle2' });
await page.pdf({
  path: 'c:/tmp/john-lonergan-cv.pdf',
  format: 'A4',
  printBackground: true,
  displayHeaderFooter: false,
  margin: { top: '0', right: '0', bottom: '0', left: '0' }
});
await browser.close();
```

**Steps:**
1. Ensure Jekyll is running (`docker ps` — should see a container on port 4000)
2. If Jekyll hasn't picked up the latest `cv.html`, copy it: `Copy-Item cv.html _site\cv.html -Force`
3. `cd c:\tmp && node gen-cv-pdf.mjs`
4. PDF written to `c:\tmp\john-lonergan-cv.pdf` — open and inspect
5. If happy: `Copy-Item c:\tmp\john-lonergan-cv.pdf assets\john-lonergan-cv.pdf -Force`

### PDF Update Workflow — MANDATORY

Every time `cv.html` content changes and is approved:

1. User reviews and explicitly approves `cv.html` content
2. Run Puppeteer to generate PDF (steps above)
3. Open PDF and verify — page breaks, readability, nothing cut off
4. Copy to `assets/john-lonergan-cv.pdf`
5. **Commit BOTH `cv.html` AND `assets/john-lonergan-cv.pdf` in the same commit**
6. **NEVER commit `cv.html` without an updated `assets/john-lonergan-cv.pdf` alongside it**
7. **NEVER commit either file without explicit user approval**

If the PDF has not been generated and placed in `assets/`, do not push `cv.html`.

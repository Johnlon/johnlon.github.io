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

## CV Design Recipe

These settings define the intended look of `cv.html`. Maintain them when making changes:

- **Body font size:** 14px
- **Name heading:** 2.1rem
- **Profile / summary bullets:** 14px
- **Job body bullets, skills, open source bullets:** 13px
- **Section labels:** 11px mono, uppercase, green, letter-spacing 0.12em
- **Job title h3:** 1.05rem, weight 700
- **Dates:** 12px mono, amber colour
- **Max content width:** 860px
- **Nav:** "John Lonergan" left (green mono), links right (text colour, bold), "Download CV" green button
- **Nav links order:** Home · Bio · Blog · Projects · Download CV
- **Print/PDF:** White background, dark green (#1a6b3a) accents, A4, 16mm margins
- **Download button:** direct `<a href="/assets/john-lonergan-cv.pdf" download>` link — serves the committed PDF file
- **PDF format:** A4, margins 14mm all sides (`margin: [14,14,14,14]` in html2pdf, `size: A4; margin: 16mm` in `@page`)
- **Page breaks:** `page-break-inside: avoid` on `.job`, `.cv-section`, `.summary-bullets`, `.edu-item`; `page-break-after: avoid` on `.cv-section-label`, `.job-header`; `page-break-before: avoid` on `.job-body`
- **html2pdf pagebreak mode:** `['avoid-all', 'css', 'legacy']` — ensures jobs and sections never split across pages

## PDF Update Workflow — MANDATORY

The PDF is stored as a static file at `assets/john-lonergan-cv.pdf` and committed to git.

**Every time `cv.html` is changed and approved, follow these steps WITHOUT EXCEPTION:**

1. User reviews and explicitly approves `cv.html` content
2. Open `cv.html` locally in the browser
3. Use the browser print dialog (`Ctrl+P`) → **Save as PDF** → A4, background graphics ON → save as `john-lonergan-cv.pdf`
4. Move the saved file to `assets/john-lonergan-cv.pdf` (overwriting the previous version)
5. Commit BOTH `cv.html` AND `assets/john-lonergan-cv.pdf` in the same commit
6. NEVER commit `cv.html` without an updated `assets/john-lonergan-cv.pdf` alongside it
7. NEVER commit either file without explicit user approval

If the user has not yet placed the PDF in `assets/`, tell them clearly and do not push `cv.html` until both files are ready.

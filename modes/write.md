# Mode: write — Resume & Cover Letter Writer

Generates a tailored, ATS-clean resume and a matching one-page cover letter for a specific job. Reads the candidate's source of truth (`cv.md`), maps real proof points to the JD, and produces copy-paste-ready output plus printable HTML.

This mode never invents experience. Everything it writes is grounded in `cv.md` and, where present, the offer report.

## Requirements

- **Always**: `cv.md` (candidate source of truth) must be readable.
- **Best**: the JD text or URL, and an existing offer report in `reports/` (for archetype + proof-point mapping).
- **Optional**: `modes/_profile.md` for archetype framing, `article-digest.md` for detailed metrics.

## Inputs

| Input | How provided | Required |
|-------|--------------|----------|
| JD | URL, pasted text, or screenshot | Yes (for tailoring) |
| cv.md | Project root | Yes |
| Offer report | `reports/` match by company | No |
| Target language | Defaults to JD language (EN default) | No |

If no JD is provided, ask for one. A resume written without a target is just a reformat — say so and offer a generic polish instead.

## Workflow

```
1. LOAD     → Read cv.md (+ article-digest.md, _profile.md if present)
2. TARGET   → Read the JD; match against reports/ by company name
3. ARCHETYPE→ Detect role archetype; pull framing from _profile.md
4. MAP      → Map JD requirements → real proof points from cv.md
5. RESUME   → Tailor summary, reorder/rewrite bullets, surface keywords
6. LETTER   → Draft 1-page cover letter: JD quotes → proof points
7. CHECK    → Run the quality gate (clichés, ATS, specifics, length)
8. OUTPUT   → Markdown for review + printable HTML for PDF
```

## Step 1 — Load source of truth

- Read `cv.md`. This is the ONLY source of experience and metrics.
- If `article-digest.md` exists, prefer its metrics over `cv.md` for articles/projects.
- If `modes/_profile.md` exists, read it after `_shared` rules — user customizations win.

**RULE: Never modify `cv.md`.** This mode reads from it, never writes to it.

## Step 2 — Target the JD

1. Extract company, role title, and the JD text (URL → fetch; screenshot → Read; or pasted).
2. Grep `reports/` for the company. If a report exists, load it for the archetype, score, and Section G drafts.
3. Pull out the JD's must-haves: named tools, responsibilities, outcomes, seniority signals.

## Step 3 — Detect archetype

Classify the role (per the archetype table in `_shared.md`). Then read `_profile.md` for the candidate's specific framing and proof points for that archetype. The resume summary and the letter's opening should both speak to that archetype.

## Step 4 — Map requirements to proof points

Build a quick mapping table (internal, not shown in final output unless asked):

| JD requirement | Proof point from cv.md | Where it lands |
|----------------|------------------------|----------------|
| e.g. "stakeholder management" | "primary liaison for media relations" (Cool FM) | Summary + bullet 3 |

- One JD requirement → one real proof point. No fabrication.
- If a requirement has no backing in `cv.md`, leave it out. Do NOT invent it. Flag the gap in notes.

## Step 5 — Write the resume

Tailor, don't rewrite from scratch:

- **Professional Summary**: 2-4 sentences, rewritten for this archetype. Lead with the strongest archetype-relevant proof point. No clichés.
- **Bullets**: Reorder so the most relevant experience leads. Rephrase to mirror JD keywords *only where the underlying experience is real*. Start bullets with varied action verbs. Prefer specifics ("Increased MANEX audit score from 62% to 83%") over abstractions.
- **Skills**: Surface the JD's named tools/skills that the candidate genuinely has, near the top.
- **Keep**: contact info, education, publications, references line — unchanged from `cv.md`.
- **Length**: 1-2 pages. Cut weakest bullets first if it runs long.

## Step 6 — Write the cover letter

One page, max. Same visual identity as the resume.

Structure:
1. **Hook** (1 short para): why this company/role specifically — quote one concrete thing from the JD.
2. **Proof** (1-2 paras): 2-3 JD requirements, each mapped to a real proof point with a metric where possible.
3. **Fit + close** (1 para): archetype framing + clear, confident close in the "I'm choosing you" tone — not pleading.

Rules:
- Quote the JD, map to `cv.md`. Never generic.
- Never share phone number in the letter body.
- Native, direct English. Short sentences. Active voice.

## Step 7 — Quality gate

Before presenting, self-check against `_shared.md` writing rules:

- [ ] No cliché phrases ("passionate about", "results-oriented", "leveraged", "spearheaded", "synergies", "robust", "seamless", "cutting-edge")
- [ ] No invented experience or metrics — every claim traces to `cv.md`
- [ ] Specifics over abstractions; named tools and numbers
- [ ] Varied sentence structure and opening verbs
- [ ] ASCII-clean (no em-dashes, smart quotes, zero-width chars) for ATS
- [ ] Cover letter ≤ 1 page; resume ≤ 2 pages
- [ ] Output language matches the JD (EN default)

## Step 8 — Output format

```
## Resume — [Candidate] for [Company] · [Role]

Tailored from: cv.md | Archetype: [type] | Report: #NNN (if any)

### Professional Summary
[rewritten summary]

### [Sections, tailored bullets...]

---

## Cover Letter — [Company] · [Role]

[Dear Hiring Team / named recipient,]

[Hook]

[Proof paragraphs]

[Fit + close]

[Sign-off — name + email only, no phone]

---

### Notes
- Gaps flagged: [JD requirements with no cv.md backing]
- Personalization to review: [...]
- To produce PDF: save HTML and run `node generate-pdf.mjs`
```

If the candidate wants printable files, also emit a self-contained HTML version using the same visual design as `templates/cv-template.html`, ready for `generate-pdf.mjs`.

## Guardrails

- NEVER invent experience, metrics, employers, or dates.
- NEVER modify `cv.md` or portfolio files.
- NEVER submit anything on the candidate's behalf.
- NEVER include the phone number in the cover letter body.
- ALWAYS ground every claim in `cv.md` (or `article-digest.md`).
- ALWAYS write in the JD's language, default English.
- If the role and CV genuinely don't match, say so plainly rather than stretching the truth.

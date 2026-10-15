# AI Risk Checker (Dual-Standard Edition)

| Field | Value |
|---|---|
| Title | AI Risk Checker — Dual-Standard Edition |
| Document Reference | HCXAI-prj05-001 *(provisional — confirm against register)* |
| Version | 1.0 |
| Status | Released |
| Owner | HCXAIResearch (https://hcxairesearch.org/) |
| Audience | AI governance leads, DPOs and risk owners, compliance and assurance teams, ISO/IEC 42001 implementers |
| Standards Reference | EU AI Act (Regulation 2024/1689); ISO/IEC 42001:2023. Classification cross-reads informed by NIST AI RMF 1.0 and OECD AI Principles |
| License | See LICENSE |
| Project Link | https://hcxairesearch.github.io/ai-risk-checker/ *(provisional)* |

A single-file, browser-based tool that walks an AI system through a structured risk and algorithmic assessment against **two frameworks at once** — the EU AI Act as binding law and ISO/IEC 42001 as the management-system standard — and shows, at every stage, which provision of each is in play. You answer the questions, rate the risks, and export a practical report as HTML, Markdown or print-to-PDF.

Eight assessment stages plus a computed summary, a live classification gate, likelihood×impact risk scoring, a dual-standard coverage matrix. No backend, no build step, no telemetry.

---

## Why this exists

Most AI risk templates pick a side. They either walk the EU AI Act's obligations or they structure an ISO/IEC 42001 management system — rarely both, and almost never side by side. A practitioner is then left to hold the mapping in their head: *this* data-governance duty under Article 10 corresponds to *that* set of Annex A controls under A.7, and the impact assessment the regulation calls a FRIA is the same artefact the standard calls an AI system impact assessment.

That mental cross-walk is where governance work quietly goes wrong. A deployer satisfies one instrument and assumes the other is covered; a gap between what the law compels and what the standard recommends goes unnoticed; and the cases that fall *outside* both frameworks — the ones that matter most — are never surfaced because nothing in the workflow was looking for them.

This tool takes the dual-standard view as the design premise. Two distinctions run through it and are enforced by the structure:

| Distinction | Consequence for the assessment |
| --- | --- |
| What the **law requires** vs. what the **standard recommends** | Every stage carries two parallel reference rails — EU AI Act provisions (legal duties) and ISO/IEC 42001 clauses and Annex A controls (management controls). The summary marks each requirement *Required*, *Recommended* or *Not triggered*, so the difference between the two is visible rather than assumed |
| What the **classification is** vs. what the **residual risk is** | The Article 5 / Annex III / Article 50 gate fixes the legal tier; the likelihood×impact register records the risk that remains after it. A system can be *minimal risk* in law and still carry *critical* residual risk — and the report says so |

The result is an assessment that reads the same system through both lenses in one pass, and flags where neither lens reaches.

---

## What is in it

The assessment runs as a stepped wizard. Each stage states its purpose, shows the two standards rails, and collects the relevant inputs. The summary is computed from everything entered.

| # | Stage | What it captures | Standards rail |
| --- | --- | --- | --- |
| 1 | System profile and your role | Purpose, provider/deployer role, GPAI dependency, deployment context and affected persons | `Art. 2–3, 25, 51` · `Cl. 4, A.6.2.2` |
| 2 | Risk classification | The gate: prohibited practices, high-risk categories, transparency triggers — drives downstream applicability | `Art. 5 / 6+Annex III / 50` · `Cl. 6.1.2, A.5.2` |
| 3 | Data and data governance | Representativeness, bias, provenance and lawful basis, data quality — each rated | `Art. 10` · `A.7.2–A.7.6` |
| 4 | Algorithmic and model risk | Accuracy, robustness and drift, fairness, explainability, cybersecurity — each rated | `Art. 15, 9` · `A.6.2.3–4, Cl. 8.2` |
| 5 | Fundamental rights and societal impact | Rights impact, vulnerable groups, autonomy and contestability, systemic effect — each rated | `Art. 27` · `Cl. 6.1.4 / 8.4, A.5` |
| 6 | Transparency, documentation and human oversight | Technical documentation, logging, user disclosure, assigned oversight | `Art. 11–14, 50` · `A.8, A.9, Cl. 7.5` |
| 7 | Risk treatment and controls | Treatment plan, Statement of Applicability, residual-risk acceptance, policy linkage | `Art. 9, 17` · `Cl. 6.1.3 / 8.3, A.2` |
| 8 | Monitoring, incidents and continual improvement | Post-market monitoring, incident reporting, management review, improvement actions | `Art. 72, 73, 43/47/49` · `Cl. 9, 10, A.6.2.6` |
| — | Assessment summary | Classification banner, system record, risk register, dual-standard coverage matrix, open gaps | *(computed)* |

Two mechanics do the work behind the stages:

- **The classification gate.** Stage 2 resolves the EU AI Act tier live — a single Article 5 practice returns *Prohibited*; any Annex III category returns *High-risk* and switches the full Article 9–15 regime to *Required* in the summary; a transparency trigger adds Article 50 on top; otherwise *Minimal*.
- **The risk register.** Stages 3–5 rate each risk by likelihood and impact; the tool computes a Low / Medium / High / Critical rating, sorts the register by severity, and carries it into the report.

---

## Running it

Open `index.html` in any modern browser. That is the whole installation procedure.

To host it, drop the file anywhere that serves static content — GitHub Pages, a SharePoint document library, an internal wiki, an S3 bucket. It has no server dependency and no same-origin requirement.

The only external request is to Google Fonts. If your environment blocks them, the tool degrades to the declared fallbacks (system sans-serif) with no loss of function. The HCXAIResearch logomark is embedded as a base64 image, so it renders with or without network access.

---

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The tool. Self-contained: markup, CSS, stage definitions, scoring and export logic in one file |
| `README.md` | This document |

---

## Design and branding

Branded to the HCXAIResearch identity. Tokens are declared once in `:root` and nothing downstream hard-codes a colour.

| Token | Value | Role |
|---|---|---|
| `--ink` / `--eu` | `#1B2A4A` | Navy — body text, masthead, and the **EU AI Act** rail (legal duties) |
| `--iso` | `#0E7C7B` | Deep teal — the **ISO/IEC 42001** rail (management controls) |
| `--eu-tint` | `#EEF4FF` | EU rail fill |
| `--iso-tint` | `#E6F4F4` | ISO rail fill, logomark surround |
| `--accent-blue` | `#3B82F6` | Secondary accent |
| `--accent-violet` | `#8B5CF6` | Secondary accent |
| `--paper` | `#F8FAFC` | Page stock |
| `--card` | `#FFFFFF` | Panels |
| `--line` | `#DDE7EE` | Hairlines and borders |
| `--muted` | `#64748B` | Secondary text, hints |

Risk ratings and EU AI Act tiers carry their own semantic colours — red (High / Critical / Prohibited), amber (Medium / High-risk), blue (Limited risk), green (Low / Minimal risk) — deliberately outside the brand palette, so a rating or a classification is never mistaken for decoration.

**Typography.** Archivo for stage headings and display; Inter for the interface and body; IBM Plex Mono for every provision and clause reference. The mono face is doing real work here: rendering `Art. 10` and `A.7.4` as precise, legible citations rather than prose, so the two standards rails read as a reference apparatus.

**Signature device.** The two standards rails. At every stage a navy EU AI Act rail and a teal ISO/IEC 42001 rail sit in parallel, each carrying its provision codes in mono. The rendered report mirrors the on-screen assessment, and the **dual-standard coverage matrix** closes it — one row per requirement, EU provision and ISO control side by side, marked *Required*, *Recommended* or *Not triggered* against the computed classification.

**Emblem.** The HCXAIResearch bracket-and-figure logomark, embedded as a base64 image in the site header and hero.

---

## Technical notes

- Vanilla JavaScript, no framework, no bundler. Stage content is held as a data structure; the renderer rebuilds the active panel on navigation and recomputes the classification and ratings on every input.
- **No browser storage.** State is in memory only. A reload clears the assessment, by design — the tool runs one assessment at a time and keeps no library. Export before you close the tab.
- Risk items are rated on a Low / Medium / High scale for both likelihood and impact; the product resolves to Low / Medium / High / Critical.
- Exports, all client-side:
  - **HTML** and **Markdown** are `Blob` downloads and always work, including inside a sandboxed iframe. Filenames: `hcxairesearch-ai-risk-checker-report.html` / `.md`.
  - **Print / Save PDF** uses the browser's native print dialog on a standalone, print-ruled report. In a normal browser tab this produces a clean PDF; note that `window.print()` can be blocked inside sandboxed iframes, so the HTML report carries its own Print button and is the reliable route to PDF.
- The HTML report is self-contained and re-opens outside the tool; the Markdown report suits docs-repo and static-site use.

---

## Scope limits

Read these before circulating a generated report.

1. **An assessment aid, not a conformity assessment.** The tool structures a reading of the EU AI Act and ISO/IEC 42001 against a system you describe. It is not a notified-body conformity assessment, a certification audit, or an attestation of conformance.
2. **Your reading, recorded.** The classification, ratings and coverage status reflect the answers entered. The tool computes consistently from those inputs; it does not verify the facts behind them.
3. **Abbreviated references.** Provision and clause citations are shortened for navigation. Verify against the current consolidated EU AI Act and ISO/IEC 42001:2023 texts before relying on any single reference.
4. **Not legal advice.** Where a deployment decision turns on classification or on a specific obligation, take qualified legal and compliance advice.
5. **Frameworks have edges.** The coverage matrix shows where each standard reaches — and, by omission, where neither does. Decisive controls for some systems (for example biosafety, dual-use research oversight, sector-specific regulation) sit outside both frameworks and outside this tool; a *Minimal* classification is not a finding of low risk.
6. **No persistence.** Nothing is stored between sessions. There is no audit trail unless you export and file the report yourself.

---

## Roadmap

| Item | Note |
|---|---|
| ISO/IEC 42005 integration | Add the `ISO/IEC 42005:2025` AI system impact-assessment structure alongside the 42001 rail |
| Control owner & evidence fields | Per-risk owner, action and evidence capture, so the treatment stage exports as a working Statement of Applicability |
| JSON snapshot | Export and re-import an assessment as JSON, consistent with the SharePoint versioning pattern used across the HCXAI toolkit |
| OECD / NIST tags | Surface OECD AI Principles and NIST AI RMF functions as cross-reference tags on the summary |
| Case-study templates | Pre-filled assessments for teaching, including the biosecurity / bacteriophage "framework gap" case |
| True OOXML export | A genuine `.docx` writer for document pipelines, alongside the current Markdown route |

---

## Changelog

| Version | Change |
|---|---|
| 1.0 | HCXAIResearch rebrand — navy/deep-teal dual-rail identity, Archivo / Inter / IBM Plex Mono, embedded logomark, site chrome. Export set: HTML / Markdown / print-to-PDF |
| 0.2 | Export reliability — moved to `Blob` downloads and a standalone print-ruled report, so delivery works inside sandboxed frames |
| 0.1 | First build. Eight-stage dual-standard wizard, live classification gate, likelihood×impact register, dual-standard coverage matrix |

---

*HCXAIResearch · Human Centered Explainable AI (Governance) · `HCXAI-prj05-001` v1.0*

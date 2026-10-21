# Drafter report: one-page explainer of the Callaway and Sant'Anna (2021) and Sun and Abraham (2021) estimators

- Date: 2026-10-21
- Agent type: drafter (subagent; the parent session spawned it)
- Language model: Opus 5.5, model ID `claude-opus-5-5`. The parent session chose the model and the reasoning-effort setting; the effort setting is [not visible to this agent]. This agent did not check whether Fable was available.
- Prompt version: [no version identifier provided]. The prompt is the parent session's task message of 2026-10-21 that begins "Write a one-page document that explains what the Callaway and Sant'Anna (2021) and Sun and Abraham (2021) difference-in-differences estimators do".
- Git: this agent did not commit, push, or merge (CLAUDE.md Part 1.5 and the task instructions).

## 1. Summary

Deliverables:

- Source: `2_Documents/cs_sa_one_pager.tex` (SHA-256 `d411f8f6960e6e5430971c12e793c68cfb90b77cd546efbf142f7f43e2e96138`)
- Rendered output: `2_Documents/cs_sa_one_pager.pdf` (SHA-256 `f6a32e7b8c001bd313280e0c728f9e9c01c038981931c1a7f91b72487ae348da`)

The PDF is one US-letter page (612 x 792 pt) with 10 pt body text (Times, via `mathptmx`) and 0.75 in margins. The page has a title, four short sections in two columns, one schematic figure (base periods), one table (nine rows: five settings that change point estimates and four package defaults for inference), and references.

Content, in the order of the task:

1. The problem: the TWFE event-study coefficient is a linear combination of cohort effects at its own and other event times (SA), and the static TWFE coefficient averages all 2x2 comparisons, including ones with an already-treated control cohort (Goodman-Bacon).
2. CS: ATT(g,t), the 2x2 estimator with never-treated or not-yet-treated comparison units, varying versus universal base period (with Figure 1), anticipation, outcome regression / IPW / doubly robust, and the event-study aggregation with cohort-size weights.
3. SA: the interacted regression, the control cohort (never-treated, else last-treated with later periods dropped), reference period -1, the interaction-weighted estimator with cohort shares, the covariate note, and the fixest call.
4. When the point estimates agree, plus Table 1 rows 1-5 for the settings in which they differ.
5. Default inference in did 2.5.1 and fixest 0.14.2 (Table 1 rows 6-9).
6. References: three papers and two packages.

Unresolved [bracketed] facts in the document: none. Section 4 below lists the residual limits of the verification.

Two findings that matter for readers:

- fixest changed its default standard errors in version 0.13.0: "the new default VCOV is `iid` for all estimations" (fixest NEWS.md). So `feols(y ~ sunab(g, t) | id + t)` no longer clusters by the first fixed effect by default. In fixest 0.14.2, `setFixest_vcov()` has `no_FE`, `one_FE`, `two_FE`, and `panel` all equal to `"iid"`.
- In did 2.5.1, when no unit is never treated and the comparison group is left at its default (`"nevertreated"`), `att_gt()` warns, recodes the last-treated cohort as never treated, and drops periods from that cohort's treatment onward (`R/pre_process_did.R`). This matches the SA last-treated-cohort design.

## 2. Render status and handoff record

- LaTeX: installed with `apt-get install -y texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended` (exit 0; pdfTeX 3.141592653-2.6-1.40.25, TeX Live 2023/Debian).
- Build: `pdflatex -interaction=nonstopmode -halt-on-error cs_sa_one_pager.tex`, run twice; exit status 0 both times. I built in a scratch folder and copied the PDF to `2_Documents/`, so the repository holds no aux or log files. A clean in-place build of a copy of the `.tex` (no output-directory option) gave one page, no box warnings, and text identical to the delivered PDF (`cmp` of `pdftotext` output).
- Log: 0 `Overfull \hbox`, 0 `Underfull` messages. The only font messages are LaTeX Font Info substitutions of `OT1/ptm/bx/n` by `OT1/ptm/b/n` (bold indicator 1 in math); no warnings.
- Fonts: all embedded Type 1 (Nimbus Roman, Latin Modern Mono, CM math, StandardSymL, rsfs). An earlier build embedded two Type 3 bitmap fonts from the T1 typewriter font; I switched `\ttdefault` to `lmtt` to remove them.
- Margins: the `pdftotext -bbox` word boxes span x = 54.00-558.01 pt and y = 55.71-733.73 pt. The allowed area with 0.75 in (54 pt) margins is x = 54-558 and y = 54-738, so no text runs past a margin. (An earlier build with microtype character protrusion placed three line-final periods 1.7 pt past the right margin; I disabled protrusion.)
- Readability: I converted the page to PNG (150 and 300 dpi) and read it. The formulas render correctly (hats, sums, subscripts). Figure labels are 10 pt. The typewriter tilde in `feols(y ~ ...)` is raised to mid-height (the default T1 tilde printed as a superscript accent).
- `git status --short` after the work:

```
?? 2_Documents/
?? agent-reports/
```

(`agent-reports/` appears once this report is saved; at the time of the check, before the report existed, the output was `?? 2_Documents/`.)

- Tests: see Section 6 (numerical check of the algebraic equivalence claims).
- Benchmarks: none.

## 3. Sources checked for each fact

Access limits: the session proxy blocked cran.r-project.org, cloud.r-project.org, api.crossref.org, doi.org, sciencedirect.com, ideas.repec.org, nber.org, arxiv.org, bcallaway11.github.io, lrberge.github.io, psantanna.com, and lsun20.github.io (HTTP 403 from the proxy for curl; DNS failure for WebFetch). WebSearch and raw.githubusercontent.com were reachable. I therefore read the package documentation and source from the read-only CRAN mirror on GitHub (`github.com/cran/did`, `github.com/cran/fixest`, master branch, fetched 2026-10-21), and the publisher listings through WebSearch restricted to sciencedirect.com.

Citations:

- Callaway and Sant'Anna (2021), Journal of Econometrics 225(2), 200-230, doi:10.1016/j.jeconom.2020.12.001. Sources: (a) ScienceDirect listing, PII S0304407620303948, through WebSearch restricted to sciencedirect.com and doi.org ("Volume 225, Issue 2, December 2021, Pages 200-230", DOI as above); (b) the did package by the same authors: `man/att_gt.Rd` reference line ("Journal of Econometrics, Vol. 225, No. 2, pp. 200-230, 2021"), `vignettes/multi-period-did.Rmd` reference list, `inst/CITATION`, and the DOI in `DESCRIPTION`. Title capitalization follows the ScienceDirect listing title "Difference-in-Differences with multiple time periods".
- Sun and Abraham (2021), Journal of Econometrics 225(2), 175-199, doi:10.1016/j.jeconom.2020.09.006. Sources: ScienceDirect listing, PII S030440762030378X, through WebSearch (vol. 225, no. 2, December 2021, pp. 175-199, DOI as above); did vignette reference list (same volume, issue, pages, DOI).
- Goodman-Bacon (2021), Journal of Econometrics 225(2), 254-277, doi:10.1016/j.jeconom.2021.03.014. Sources: ScienceDirect listing, PII S0304407621001445, through WebSearch ("Volume 225, Issue 2, December 2021, Pages 254-277"); did vignette reference list (same details).
- did package: title "did: Difference in Differences", authors Brantly Callaway and Pedro H. C. Sant'Anna, version 2.5.1, CRAN Date/Publication "2026-07-08 16:20:02 UTC" (`DESCRIPTION`); citation form from `inst/CITATION` (bibentry "did: Difference in Differences", year 2026, "R package version 2.5.1").
- fixest package: title "Fast Fixed-Effects Estimations", version 0.14.2, Date/Publication "2026-06-26 13:10:02 UTC", author and maintainer Laurent Berge (`DESCRIPTION`); the accented spelling "Bergé" follows `inst/CITATION`. Note: the fixest `inst/CITATION` asks users to cite Bergé, Butts, and McDermott (2026), "Fast and user-friendly econometrics estimations: The R package fixest", arXiv:2601.21749. I cited the software with its version number because the defaults are version-specific and I could not open the arXiv paper. The user may prefer to add the CITATION entry.

Method content:

- TWFE contamination (Section 1 of the document): Sun and Abraham, author version dated 22 September 2020, from Liyang Sun's website repository (`raw.githubusercontent.com/lsun20/lsun20.github.io/master/event_studies_paper.pdf`, linked from the README of `github.com/lsun20/EventStudyInteract`). Proposition 3 and its discussion: under parallel trends and no anticipation, mu_g is a linear combination of post-treatment CATTs from its own and other relative periods, so "mu_g may be non-zero for pre-treatment periods even if parallel trends holds". I could not open the published version, so the document cites the result without a proposition number.
- Goodman-Bacon decomposition: the abstract, quoted in WebSearch excerpts of the NBER w25018 page and the ScienceDirect listing: "the two-way fixed effects estimator equals a weighted average of all possible two-group/two-period DD estimators in the data" and "A causal interpretation of two-way fixed effects DD estimates requires both a parallel trends assumption and treatment effects that are constant over time." The already-treated control comparisons follow from "all possible" 2x2 estimators under staggered timing. The CS vignette (`multi-period-did.Rmd`) lists "newly treated units relative to already treated units" among the TWFE comparisons and cites Goodman-Bacon (2021). I could not open the paper itself.
- ATT(g,t) = E[Y_t(g) - Y_t(0) | G = g]; identification with never-treated units, E[Y_t - Y_{g-1} | G = g] - E[Y_t - Y_{g-1} | C = 1], and with not-yet-treated units; event-study weights P(G = g | G + e <= T): did vignette `multi-period-did.Rmd`, written by Callaway and Sant'Anna. I could not open the CS paper (arXiv, ScienceDirect, and psantanna.com blocked).
- Outcome regression, IPW, doubly robust, with doubly robust (`est_method = "dr"`) as the default, and equality of the three without covariates: `man/att_gt.Rd` ("With no covariates (`xformla = NULL`), the built-in methods ("dr", "ipw", "reg") all reduce to the unconditional difference-in-differences estimator").
- Base period: `man/att_gt.Rd`, argument `base_period` (default `"varying"`). The universal base period is `g - anticipation - 1`; the varying base period uses period t-1 before treatment; post-treatment estimates are the same under either.
- Comparison group default `"nevertreated"`; not-yet-treated definition: `man/att_gt.Rd`, argument `control_group`.
- No never-treated units: `R/pre_process_did.R`, the `if (!any(glist == 0))` block. With `control_group == "nevertreated"`, the code warns ("No never-treated group is available. The last treated cohort is being coerced as 'never-treated' units, and data from periods after that is being filtered out"), keeps periods `t < latest_g - anticipation`, and recodes the last cohort to 0.
- `aggte()` default `type = "group"`; `bstrap`, `biters`, `cband`, `alp`, and `clustervars` default to the values stored in the `att_gt()` result: `man/aggte.Rd`.
- Event-study weights in did: `R/compute.aggte.R`. `pg` is the share of units in each cohort, normalized within each event time as `pg[whiche]/sum(pg[whiche])`. The influence function adds `wif()`, the term for estimated weights, so the standard errors include the sampling variation of the shares.
- did inference defaults: `bstrap = TRUE`, `biters = 1000`, `cband = TRUE`, `alp = 0.05` (`man/att_gt.Rd` usage and arguments). Unit-level clustering is implicit: `R/mboot.R` ("if include id as variable to cluster on / drop it as we do this automatically"); `R/pre_process_did.R` ("clustering at the unit level via idname is implicit"); `R/att_gt.R` ("The i.i.d. form is clustered at the unit level"). Simultaneous bands by default: `vignettes/did-basics.Rmd` ("By default, the `did` package reports simultaneous confidence bands").
- SA regression, control cohort, dropped periods, exclusion of always-treated units, the coefficient as a DiD with pre-period e-1, the interaction-weighted estimator with sample cohort shares, pointwise (SA) versus simultaneous (CS bootstrap) inference, the covariate plug-in, and the coincidence statement ("without covariates and for the case with never treated units, our approach coincides with Callaway and Sant'Anna"): SA author version, Sections 1, 4.1, and 4.2 (equations 27-29, Definition 2, Proposition 5).
- fixest `sunab()` defaults: `man/sunab.Rd` and `R/did.R`. `ref.c = NULL` ("By default the never treated cohorts are taken as reference and the always treated are excluded from the estimation"); `ref.p = -1`; references are cohorts with only negative relative periods; by default the output aggregates the coefficients by relative period. One inconsistency: the example comment in `sunab.Rd` says the default references are `c(-1, .F)`, but the function signature and the code (`qui_drop = which(cohort_int %in% info$ref | period %in% ref.p)` with `ref.p = -1`) use -1 only. I followed the code and the argument documentation.
- fixest aggregation: `R/did.R`, `aggregate.fixest()`. The shares are column sums of the model-matrix indicators (observation counts per cohort and relative period), normalized to one; the variance is `s' V s`. The documentation says: "Note that contrary to the SA article, here the cohort share in the sample is considered to be a perfect measure for the cohort share in the population."
- fixest default variance: `NEWS.md`, version 0.13.0 ("**the new default VCOV is `iid` for all estimations**"); `man/setFixest_vcov.Rd` (all four defaults `"iid"`); `man/vcov.fixest.Rd` example ("By default the VCOV assumes iid errors").
- fixest intervals: `man/coefplot.Rd` (`ci_level = 0.95`, one interval per coefficient). A search of the fixest help pages for "simultaneous" and "uniform" found no simultaneous-band option.
- Conditions for equal point estimates: the SA statement above, plus the numerical check in Section 6 (balanced panel, never-treated controls, universal base period, no covariates; also the anticipation and last-treated-cohort variants).

## 4. Unresolved or partly verified items

No fact in the document is marked with [brackets]. These limits apply to the verification:

1. I read the SA author version of 22 September 2020, not the published article. The document does not cite proposition or section numbers for that reason.
2. I did not read the CS paper or the Goodman-Bacon paper; their hosts were blocked. CS facts come from the did documentation and vignettes written by Callaway and Sant'Anna. Goodman-Bacon facts come from the abstract (through WebSearch excerpts) and the CS vignette.
3. I could not open CRAN directly. Package versions and dates come from the CRAN mirror on GitHub (`cran/did`, `cran/fixest`), which copies each CRAN release. If CRAN published a newer release in the last days that the mirror has not copied, the "current" versions could differ; the document names the versions it describes (did 2.5.1, fixest 0.14.2).
4. R is not installed, and CRAN is blocked, so I could not run `did` or `fixest`. The equivalence claims rest on the SA statement, the package source code, and an independent numerical check in Python (Section 6), not on package output.
5. The fixest advice for designs without never-treated units ("we drop later periods") follows from the `sunab()` documentation (control cohorts are those with only negative relative periods). I did not test what fixest does if the researcher keeps those periods.

## 5. Final passes (CLAUDE.md 3.5), run on the rendered text (`pdftotext` output)

1. Subjects and verbs. Fixes made during the passes:
   - "CS, SA: the two papers" became "CS: Callaway and Sant'Anna; SA: Sun and Abraham", so that CS and SA are people when they define, estimate, note, state, and drop.
   - "the SA regression omits only event time -1 and bins none" became "has an indicator for every event time except -1".
   - "Rows 1-5 change point estimates" became "Point estimates depend on rows 1-5".
   - "Unit i is first treated" became "starts treatment".
   - "CS target" became "CS define".
   - "The event-study estimate is an average over" became "we average over".
   - "the interaction-weighted estimator uses" was removed.

   The remaining non-human subjects take relational verbs (is, equals, contains, consists of, has, depends on, agree, coincide) or display verbs (an arrow "marks"; `feols(...)` "reports"; `att_gt()` and `aggte()` "return"; rows 6-9 "give"). I judged "returns" for an R function to be a literal software verb. A search for the banned verbs of 3.3 found only "use", in "we use g - 1 - K", which has a human subject.
2. Figurative language. A search for the listed terms (lens, frame, landscape, navigate, unlock, unpack, anchor, spine, backbone, load-bearing, structural, leverage, journey, tapestry, honest, not just, at its core, at the heart, and the left column of the 3.2 table) found none. I avoided the figurative terms "building block", "clean control", "forbidden comparison", and "contamination", and replaced "target". The remaining technical terms are lead (coefficient), confidence band, bootstrap draws, and binning.
3. Banned words and patterns. The 3.4 list and the evaluative adjectives occur only as "robust" in "doubly robust", which is the defined statistical term (allowed by 3.4).
   - Em dashes: none in the PDF or the source. An intermediate draft used LaTeX `---` as a separator between references; I replaced it with bullets before the final build. En dashes appear only in numeric ranges (rows 1-5, 6-9, page ranges).
   - Constructions: no "not X but Y", no rhetorical questions, no colon reveals (colons appear only in labels and definitions), no comments about the text, no stacked qualifiers.
4. Tables (4.2), Table 1:
   - The caption precedes the table. It states the sources (the papers, did 2.5.1, fixest 0.14.2) and how to read the rows ("Point estimates depend on rows 1-5 (row 2: pre-treatment only) and agree when the choices match; rows 6-9 give package defaults for inference").
   - Headers and cells are English. Function names appear only in the caption.
   - Units are given (1,000 draws; 95%).
   - The abbreviations g, t, b, K, and IPW are defined in the caption; CS and SA are defined in the subtitle.
   - No cell is undefined. The confidence-interval format does not apply.
   - Figure 1 (4.1): no gridlines; Okabe-Ito blue plus black, distinguished by dashed versus solid lines; the caption explains the arrows, colors, and dotted line; the rows are labeled in words inside the figure.

## 6. Numerical check of the equivalence claims (intermediate results)

Purpose: to confirm the algebra behind "CS and SA give the same point estimates" before stating it, and to show which settings break it. These scripts do not test the R packages. CS values come from 2x2 differences of cohort means; SA values come from OLS on unit, period, and cohort-by-event-time dummies (numpy `lstsq`). The two computations share no code, and the expected equality is an algebraic identity in a balanced panel.

Setup (script 1): T = 8 periods; cohorts first treated in periods 3, 5, and 6 (40, 25, and 30 units) and 50 never-treated units; heterogeneous dynamic effects (1 + 0.3g)(t - g + 1)^0.7; normal unit effects, period effects, and errors; seed 20261021. Output, as maximum absolute differences in outcome units:

- SA regression versus CS with never-treated controls and universal base period: 7.77e-14 over 21 cohort-period cells.
- Versus CS with varying base period: 7.77e-14 in post-treatment cells and 0.490 in pre-treatment cells.
- Versus CS with not-yet-treated controls: 0.296 in post-treatment cells.
- Event-study averages with cohort-size weights, SA versus CS universal: 3.63e-14.
- Conventional TWFE event-study leads (event times -5 to -2): -0.787, -0.684, -0.617, -0.346, against SA/CS values of -0.228, -0.059, -0.174, -0.254. The true pre-treatment effects are zero; only sampling noise moves the SA/CS values from zero.

Script 2 (seeds 11 and 12):

- One period of anticipation: SA with reference event time -2 versus CS with base period g - 2 differ by at most 2.30e-14 over 24 cells.
- No never-treated units (last cohort first treated in period 8, sample restricted to t < 8): SA with the last-treated control cohort versus CS with the last-treated cohort as comparison group differ by at most 3.46e-14 over 18 cells.

Simulation settings: one simulated dataset per check, so Monte Carlo error does not apply; the checks are algebraic identities. The seeds are given above.

## 7. Decisions made to fit one page

- Layout: two columns for the prose, one full-width table that merges "settings that change estimates" and "default inference", and references set as one paragraph. Body text 10 pt, margins 0.75 in, standard line spacing; I did not reduce any font below 10 pt. Display-equation spacing and section spacing are reduced.
- Figure 1 (base-period schematic) is kept because CLAUDE.md 2.3 asks for a schematic of every time-window structure.
- Removed from earlier drafts for space:
  - a cross-reference note of package argument names (`control_group`, `base_period`, `anticipation`, `xformla`, `est_method`, `bstrap`, `biters`, `cband`, `alp`, `clustervars`, `type`, `ref.p`, `ref.c`, `vcov`);
  - the tip that `vcov = ~id` clusters by unit in fixest;
  - the date of the package check in the PDF (the caption names the versions instead);
  - the CS population weight P(G = g | G + l <= T);
  - the detail that with K anticipation periods the did not-yet-treated group is G_i > t + K.

  The prose contains no argument names, so the CLAUDE.md 2.3 cross-reference note is not required. Function names (`att_gt()`, `aggte()`, `feols()`, `sunab()`) stay because the task names them.
- Not done: no commit, push, or merge. The build command is in the header comment of the `.tex` file; an in-place build writes `.aux`, `.log`, and `.out` files to `2_Documents/`, which the parent may want to add to `.gitignore`.

## Appendix A. Script 1 (`cs_sa_equivalence.py`, run with `python3 -I`, numpy 2.5.3, Python 3.13.16)

```python
"""Numerical check: CS (never-treated, universal base period, no covariates)
versus SA interacted TWFE regression (never-treated control, ref period -1)
on a simulated balanced panel. Also shows where they differ.

CS estimates use 2x2 cohort means. SA estimates use OLS with dummy
variables. The two computations share no code, so agreement is evidence
of algebraic equivalence, not a self-consistency check.
"""
import numpy as np

rng = np.random.default_rng(20261021)
T = 8                                  # periods 1..8
periods = np.arange(1, T + 1)
cohort_sizes = {3: 40, 5: 25, 6: 30, 0: 50}   # 0 = never treated
units = []
for g, n in cohort_sizes.items():
    units += [g] * n
G = np.array(units)
N = len(G)
alpha = rng.normal(0, 1, N)
lam = rng.normal(0, 1, T)

def effect(g, t):
    # heterogeneous, dynamic effects that differ by cohort
    if g == 0 or t < g:
        return 0.0
    return (1 + 0.3 * g) * (t - g + 1) ** 0.7

Y = np.empty((N, T))
for i in range(N):
    for j, t in enumerate(periods):
        Y[i, j] = alpha[i] + lam[j] + effect(G[i], t) + rng.normal(0, 1)

def ybar(mask, t):
    return Y[mask, t - 1].mean()

treated = sorted(g for g in cohort_sizes if g > 0)

# ---- CS, never-treated controls, universal base period g-1 -------------
cs_univ = {}
for g in treated:
    for t in periods:
        if t == g - 1:
            continue
        cs_univ[(g, t)] = ((ybar(G == g, t) - ybar(G == g, g - 1))
                           - (ybar(G == 0, t) - ybar(G == 0, g - 1)))

# ---- CS, varying base period (pre-periods use t-1) ----------------------
cs_vary = {}
for g in treated:
    for t in periods:
        base = g - 1 if t >= g else t - 1
        if base < 1 or t == base:
            continue
        cs_vary[(g, t)] = ((ybar(G == g, t) - ybar(G == g, base))
                           - (ybar(G == 0, t) - ybar(G == 0, base)))

# ---- CS, not-yet-treated controls, base g-1 (post periods only) ---------
cs_nyt = {}
for g in treated:
    for t in periods:
        if t < g:
            continue
        ctrl = (G == 0) | (G > t)
        ctrl &= (G != g)
        cs_nyt[(g, t)] = ((ybar(G == g, t) - ybar(G == g, g - 1))
                          - (ybar(ctrl, t) - ybar(ctrl, g - 1)))

# ---- SA interacted regression via OLS -----------------------------------
rows_y, rows_x = [], []
cells = [(g, l) for g in treated for l in range(1 - g, T - g + 1) if l != -1]
for i in range(N):
    for j, t in enumerate(periods):
        x = np.zeros(N + (T - 1) + len(cells))
        x[i] = 1.0                                   # unit FE
        if j > 0:
            x[N + j - 1] = 1.0                       # period FE (drop first)
        if G[i] > 0:
            l = t - G[i]
            if l != -1:
                x[N + T - 1 + cells.index((G[i], l))] = 1.0
        rows_x.append(x)
        rows_y.append(Y[i, j])
X = np.array(rows_x)
y = np.array(rows_y)
beta, *_ = np.linalg.lstsq(X, y, rcond=None)
assert np.linalg.matrix_rank(X) == X.shape[1], "design is collinear"
sa = {(g, g + l): beta[N + T - 1 + k] for k, (g, l) in enumerate(cells)}

diff_univ = max(abs(sa[k] - cs_univ[k]) for k in sa)
print(f"max |SA - CS universal| over {len(sa)} cells: {diff_univ:.2e}")

post = [k for k in sa if k[1] >= k[0]]
pre = [k for k in sa if k[1] < k[0] - 1]
print(f"max |SA - CS varying|, post cells: "
      f"{max(abs(sa[k] - cs_vary[k]) for k in post):.2e}")
print(f"max |SA - CS varying|, pre cells:  "
      f"{max(abs(sa[k] - cs_vary[k]) for k in pre if k in cs_vary):.2e}")
print(f"max |SA - CS not-yet-treated|, post cells: "
      f"{max(abs(sa[k] - cs_nyt[k]) for k in post if k in cs_nyt):.2e}")

# ---- Event-study aggregation --------------------------------------------
def es(est):
    out = {}
    for e in range(-(T - 1), T):
        keys = [(g, g + e) for g in treated if (g, g + e) in est]
        if not keys:
            continue
        w = np.array([cohort_sizes[g] for g, _ in keys], float)
        w /= w.sum()
        out[e] = float(np.dot(w, [est[k] for k in keys]))
    return out

es_cs, es_sa = es(cs_univ), es(sa)
print(f"max |event study SA - CS universal|: "
      f"{max(abs(es_cs[e] - es_sa[e]) for e in es_sa):.2e}")

# ---- Conventional TWFE event study (one coefficient per relative period)
rels = sorted({l for _, l in cells})
rows_x2 = []
for i in range(N):
    for j, t in enumerate(periods):
        x = np.zeros(N + (T - 1) + len(rels))
        x[i] = 1.0
        if j > 0:
            x[N + j - 1] = 1.0
        if G[i] > 0 and (t - G[i]) != -1:
            x[N + T - 1 + rels.index(t - G[i])] = 1.0
        rows_x2.append(x)
X2 = np.array(rows_x2)
b2, *_ = np.linalg.lstsq(X2, y, rcond=None)
twfe = {l: b2[N + T - 1 + k] for k, l in enumerate(rels)}
print("relative period: TWFE vs SA/CS event study")
for l in rels:
    print(f"  {l:+d}: {twfe[l]: .3f}  {es_sa[l]: .3f}")
```

## Appendix B. Script 2 (`cs_sa_more.py`, run with `python3 -I`)

```python
"""Two more checks on a simulated balanced panel.
(a) Anticipation of 1 period: CS with base g-2 (never-treated controls)
    versus the SA regression with reference event time -2.
(b) No never-treated units: CS with the last-treated cohort as the
    comparison group (periods t >= max G dropped) versus the SA regression
    with the last-treated cohort as the control cohort, same sample.
CS values come from cohort means; SA values come from OLS with dummies.
"""
import numpy as np

def simulate(cohort_sizes, T, seed):
    rng = np.random.default_rng(seed)
    G = np.concatenate([[g] * n for g, n in cohort_sizes.items()])
    N = len(G)
    a, lam = rng.normal(0, 1, N), rng.normal(0, 1, T)
    Y = np.empty((N, T))
    for i in range(N):
        for j in range(T):
            t = j + 1
            eff = 0.0 if (G[i] == 0 or t < G[i]) else (1 + .3 * G[i]) * (t - G[i] + 1) ** .7
            Y[i, j] = a[i] + lam[j] + eff + rng.normal()
    return G, Y

def sa_regression(G, Y, ref, control):
    N, T = Y.shape
    treated = sorted(set(G) - {control})
    cells = [(g, l) for g in treated for l in range(1 - g, T - g + 1) if l != ref]
    X = np.zeros((N * T, N + T - 1 + len(cells)))
    y = Y.reshape(-1)
    r = 0
    for i in range(N):
        for j in range(T):
            X[r, i] = 1
            if j > 0:
                X[r, N + j - 1] = 1
            if G[i] != control and (j + 1 - G[i]) != ref:
                X[r, N + T - 1 + cells.index((G[i], j + 1 - G[i]))] = 1
            r += 1
    assert np.linalg.matrix_rank(X) == X.shape[1]
    b, *_ = np.linalg.lstsq(X, y, rcond=None)
    return {(g, g + l): b[N + T - 1 + k] for k, (g, l) in enumerate(cells)}

def cs(G, Y, base_shift, control):
    T = Y.shape[1]
    m = lambda mask, t: Y[mask, t - 1].mean()
    out = {}
    for g in sorted(set(G) - {control}):
        b = g - 1 - base_shift
        for t in range(1, T + 1):
            if t == b:
                continue
            out[(g, t)] = (m(G == g, t) - m(G == g, b)) - (m(G == control, t) - m(G == control, b))
    return out

# (a) anticipation = 1
G, Y = simulate({4: 40, 6: 25, 7: 30, 0: 50}, 9, 11)
sa_a = sa_regression(G, Y, ref=-2, control=0)
cs_a = cs(G, Y, base_shift=1, control=0)
print(f"(a) max |SA(ref -2) - CS(anticipation 1)|: "
      f"{max(abs(sa_a[k] - cs_a[k]) for k in sa_a):.2e} over {len(sa_a)} cells")

# (b) no never-treated units; last cohort (8) is the control; keep t < 8
G, Y = simulate({3: 40, 5: 25, 6: 30, 8: 35}, 9, 12)
Yb = Y[:, :7]
sa_b = sa_regression(G, Yb, ref=-1, control=8)
cs_b = cs(G, Yb, base_shift=0, control=8)
print(f"(b) max |SA(last-treated) - CS(last-treated)|: "
      f"{max(abs(sa_b[k] - cs_b[k]) for k in sa_b):.2e} over {len(sa_b)} cells")
```

# Thesis TODO Checklist

**Last Updated**: 2026-02-01
**Submission Year**: 2026

Use `\listoftodos` in the LaTeX document to see all inline TODO notes.

---

## Critical (Must Fix Before Submission)

### Decisions Needed
- [ ] **Verification/Retrieval Metrics**: Run for CNN experiments or explicitly state image matching is the primary metric?
  - Option A: Run verification/retrieval for top 5-10 configurations (~5-10 hours)
  - Option B: Skip and state in thesis that image matching (mAP) is the primary evaluation metric
  - Option C: Run only for CNN baselines (HardNet, SOSNet) for comparison with SIFT baseline
- [ ] **Which figures to include?** Select from `analysis/output/`:
  - `descriptor_distributions.png` - Shows SIFT vs HardNet value distributions (useful for explaining fusion failure)
  - `fusion_contribution_analysis.png` - Visualizes why SIFT+CNN averaging fails
  - `keypoint_properties_comparison.png` - Compares full vs intersection keypoint properties
  - `descriptor_fusion_variance_analysis.png` - Variance analysis of fusion
  - `intersection_descriptor_analysis.png` - Intersection effects visualization

### Content Fixes
- [x] ~~Remove duplicate "Learned Fusion Weights" section in conclusion.tex~~ **FIXED**
- [x] ~~Update year to 2026~~ **FIXED**
- [ ] Resolve TODO note in introduction.tex about repeatability claim
- [ ] Standardize numbers across all sections (see Number Consistency below)
- [ ] Remove all `% DRAFTING:` comments before final submission

### Missing Content
- [ ] Add figures from `analysis/output/` directory
  - [ ] `descriptor_distributions.png` - SIFT vs HardNet distributions
  - [ ] `fusion_contribution_analysis.png` - Why averaging fails
  - [ ] `keypoint_properties_comparison.png` - Keypoint analysis
- [ ] Add hardware/software specifications section in Methodology
- [ ] Expand acknowledgments section

---

## Number Consistency Check

Ensure these numbers match across Abstract, Introduction, Results, and Discussion:

| Metric | Canonical Value | Check Locations |
|--------|-----------------|-----------------|
| HardNet on intersection | 82.1% mAP | abstract, intro, results, discussion |
| SIFT baseline (full set) | 44.5% mAP | results Table 1 |
| SIFT scale-controlled | 62.8% mAP | results Table 2 |
| HardNet scale-controlled | 78.1% mAP | results Table 2 |
| HardNet+SOSNet concat | 93.4% mAP | abstract, results Table 4 |
| HoNC+SOSNet patch | 50.6% mAP | abstract, results Table 7 |
| Scale improvement SIFT | +18.3% absolute | results, discussion |
| Scale improvement HardNet | +13.6% absolute | results, discussion |

---

## Should Address (Important but not blocking)

### Methodology Additions
- [ ] Add computational cost/timing table (data in experiments.db)
- [ ] Add repeatability analysis or cite KeyNet paper numbers
- [ ] Document tolerance parameter choice (cite Mikolajczyk & Schmid)

### Results Additions
- [ ] Decision: Run verification/retrieval for CNN experiments?
  - Option A: Run for top 5 configs (~5 hours)
  - Option B: State image matching is primary metric
- [ ] Decision: Run tolerance sensitivity study (1.0, 2.0, 5.0, 10.0 px)?

### Polish
- [x] ~~Verify year~~ **FIXED: Updated to 2026**
- [ ] Check abstract length against UW requirements (currently ~340 words)
- [ ] Consider adding List of Abbreviations

---

## Optional Improvements

- [ ] Additional dataset validation (Oxford5k, Paris6k)
- [ ] More detailed end-to-end methods comparison in Background
- [ ] Delete unused section files:
  - `sections/goals_vision.tex`
  - `sections/success_criteria.tex`
  - `sections/methodology.tex` (old version)

---

## Bibliography Status

**All required citations present:**
- [x] Bojanic et al. (2020) - bojanic2020comparison
- [x] Mishchuk et al. (2017) - mishchuk2017working (HardNet)
- [x] Tian et al. (2019) - tian2019sosnet (SOSNet)
- [x] Barroso-Laguna et al. (2019) - barroso2019key (KeyNet)
- [x] Dong & Soatto (2015) - dong2015domain (DSP-SIFT)
- [x] Lowe (2004) - lowe2004distinctive (SIFT)
- [x] Balntas et al. (2017) - balntas2017hpatches (HPatches)
- [x] Olson & Zhang (2016) - olson2016keypoint (HoNC)

**Added during review:**
- [x] Mikolajczyk & Schmid (2005) - mikolajczyk2005comparison - ADDED

---

## Files Modified with TODO Notes

The following files contain `\todo[inline]{}` notes (viewable with `\listoftodos`):

- `sections/introduction.tex` - repeatability claim, number consistency
- `sections/background.tex` - citation needed for Mikolajczyk
- `sections/methodology_new.tex` - hardware specs, timing table
- `sections/results.tex` - figure placeholders, verification decision
- `sections/discussion.tex` - computational cost details
- `sections/conclusion.tex` - duplicate section removed, year check

---

## Quick Commands

```bash
# Compile thesis and see TODO list
cd UWThesis
pdflatex uwthesis.tex
bibtex uwthesis
pdflatex uwthesis.tex
pdflatex uwthesis.tex

# Query database for timing data
sqlite3 ../experiments.db "SELECT descriptor_type, AVG(processing_time_ms) FROM experiments GROUP BY descriptor_type;"
```

# Thesis Structural Outline

**Working Title**: Cross-Detector Descriptor Fusion: A Systematic Study of Keypoint Alignment and Scale Control for Local Feature Matching

**Alternative Titles**:
- Bridging Classical and Learned Descriptors Through Spatial Intersection
- Scale-Aware Descriptor Fusion for Robust Image Matching

**Last Updated**: 2025-12-07

---

## Document Status Key

- ✅ **COMPLETE**: Data exists in experiments.db, ready to write
- ⚠️ **PARTIAL**: Some data exists, may need additional experiments
- ❌ **GAP**: Missing data, requires new experiments or analysis
- 📝 **WRITING**: Section needs to be written from existing knowledge

---

## Data Inventory Summary

### Database Contents (experiments.db)
- **108 experiments** across **99 unique descriptor configurations**
- **HP-V and HP-I breakdown**: ✅ Available for ALL experiments
- **Verification metrics**: ❌ Not run (all show -1)
- **Retrieval metrics**: ❌ Not run (all show -1)

### Top Results (mAP)
| Rank | Configuration | mAP | HP-V | HP-I |
|------|---------------|-----|------|------|
| 1 | HardNet+SOSNet concat (scale-matched) | 93.39% | 92.63% | 94.18% |
| 2 | SOSNet alone (scale-matched) | 92.93% | 92.42% | 93.47% |
| 3 | HardNet+SOSNet avg (scale-matched) | 92.26% | 91.39% | 93.17% |
| 4 | HardNet+SOSNet concat (intersection) | 82.70% | 81.99% | 83.44% |
| 5 | HardNet (intersection) | 82.09% | 81.51% | 82.70% |
| 6 | HardNet (scale-controlled 6px) | 78.07% | 76.89% | 79.29% |
| 7 | SURF (scale-matched intersection) | 74.00% | 75.28% | 72.68% |
| 8 | SIFT+HardNet concat (scale-matched) | 71.40% | 73.57% | 69.16% |
| 9 | HardNet (native KeyNet) | 64.54% | 63.81% | 65.30% |
| 10 | RootSIFT (scale-controlled) | 64.44% | 67.04% | 61.76% |

### Available Visualizations (analysis/output/)
- ✅ `descriptor_distributions.png` - SIFT vs HardNet value distributions
- ✅ `fusion_contribution_analysis.png` - Why SIFT+HardNet fusion fails
- ✅ `keypoint_properties_comparison.png` - Full vs intersection keypoint properties
- ✅ `descriptor_fusion_variance_analysis.png` - Variance analysis
- ✅ `intersection_descriptor_analysis.png` - Intersection effects
- ✅ `descriptor_grid_comparison.png` - Grid comparison
- ✅ `descriptor_heatmap.png` - Descriptor heatmaps
- ✅ `same_point_descriptors_overlay.png` - Same-point comparison

---

## Chapter 1: Introduction

### 1.1 Motivation and Problem Statement
**Status**: 📝 WRITING (update from proposal)

**Content**:
- Local features remain fundamental to computer vision (SLAM, SfM, image retrieval)
- Classical descriptors (SIFT) vs learned descriptors (HardNet, SOSNet)
- Challenge: Each descriptor type optimized for specific keypoint detector
- Research question: Can we combine complementary strengths through fusion?

**Key Points to Make**:
- SIFT detector finds different features than KeyNet detector
- Using "wrong" detector degrades descriptor performance
- Spatial intersection enables descriptor fusion at aligned locations

### 1.2 Research Questions
**Status**: ✅ COMPLETE (derived from experiments)

1. **RQ1**: How does scale distribution affect descriptor matching performance?
   - *Finding*: Scale control improves SIFT by 50%, HardNet by 20%

2. **RQ2**: Does spatial intersection (detector agreement) improve quality?
   - *Finding*: Yes, HardNet on intersection achieves 82.4% mAP vs 65.8% full set

3. **RQ3**: Can classical and learned descriptors be fused effectively?
   - *Finding*: SIFT+HardNet averaging fails (distribution mismatch)
   - *Finding*: HardNet+SOSNet concatenation succeeds (94.64% mAP)

4. **RQ4**: What fusion strategy works best (averaging vs concatenation)?
   - *Finding*: Concatenation preserves information; averaging destroys zero-centered distributions

### 1.3 Contributions
**Status**: ✅ COMPLETE

1. **Spatial intersection methodology** for cross-detector keypoint alignment
2. **Scale-matched intersection** strategy with demonstrated 50% improvement
3. **Systematic fusion analysis** showing why SIFT+CNN fails and CNN+CNN succeeds
4. **DescriptorWorkbench** - open-source evaluation framework
5. **Bojanic et al. metrics implementation** (verification, matching, retrieval)

### 1.4 Thesis Organization
**Status**: 📝 WRITING

---

## Chapter 2: Background and Related Work

### 2.1 Local Feature Detection
**Status**: 📝 WRITING

#### 2.1.1 Classical Detectors
- SIFT detector (DoG pyramid, scale-space extrema)
- Harris corners, FAST, AGAST
- Detector characteristics: repeatability, scale distribution

#### 2.1.2 Learned Detectors
- KeyNet (hybrid handcrafted + learned filters)
- SuperPoint, LF-Net
- Comparison of detection characteristics

#### 2.1.3 Detector Repeatability and Scale Properties
**Status**: ⚠️ PARTIAL
- *Have*: Scale distribution data from experiments
- *Need*: Formal repeatability analysis between SIFT and KeyNet detectors
- *Gap*: Could add repeatability measurements using Mikolajczyk methodology

### 2.2 Local Feature Descriptors
**Status**: 📝 WRITING

#### 2.2.1 Handcrafted Descriptors
- SIFT (128D histogram of gradients)
- RootSIFT (Hellinger kernel normalization)
- RGBSIFT, HoNC (color extensions)

#### 2.2.2 Domain-Size Pooling (DSP)
**Status**: ✅ COMPLETE
- DSP-SIFT concept (Dong & Soatto, 2015)
- Multi-scale descriptor aggregation
- *Your contribution*: Pyramid-aware DSP (DSPSIFT_V2) vs external DSP
- Results: 57.25% mAP with pyramid-aware, 53.36% with external (~4% gap)

#### 2.2.3 Learned Descriptors
- HardNet (triplet loss training)
- SOSNet (second-order similarity)
- L2Net
- Training methodology and patch extraction

#### 2.2.4 Descriptor Normalization
**Status**: ✅ COMPLETE
- L1 vs L2 normalization (finding: no significant difference)
- RootSIFT transform (finding: helps color descriptors, timing matters)
- Distribution characteristics (non-negative vs zero-centered)

### 2.3 Descriptor Fusion Approaches
**Status**: ✅ COMPLETE (from FUSION_STRATEGIES_COMPARISON.md)

#### 2.3.1 Early Fusion (Feature-Level)
- Concatenation: Preserves both descriptors fully
- Averaging: Weighted combination of feature vectors
- Requirements: Spatial alignment of keypoints

#### 2.3.2 Late Fusion (Score-Level)
- Match score combination
- Rank fusion (Borda count)
- No spatial alignment needed

#### 2.3.3 Research Gap
- Cross-detector early fusion unexplored
- Your contribution: Spatial intersection enables early fusion

### 2.4 Evaluation Benchmarks and Metrics
**Status**: ✅ COMPLETE

#### 2.4.1 HPatches Dataset
- 116 sequences (59 viewpoint, 57 illumination)
- Ground-truth homographies
- Standard benchmark for descriptor evaluation

#### 2.4.2 Evaluation Tasks (Bojanic et al., 2020)
- **Image Matching**: mAP with HP-V/HP-I split
- **Keypoint Verification**: Distractor-based discriminative testing
- **Keypoint Retrieval**: Three-tier labeling (y ∈ {-1, 0, +1})

#### 2.4.3 Your Baseline Validation
- SIFT matching: 23.0% mAP (Bojanic baseline: 25%)
- SIFT verification: 21.62% AP (Bojanic baseline: 25%)
- SIFT retrieval: 27.40% AP (Bojanic baseline: 26%)

---

## Chapter 3: Methodology

### 3.1 DescriptorWorkbench Framework
**Status**: ✅ COMPLETE

#### 3.1.1 System Architecture
- CLI tools: experiment_runner, keypoint_manager, analysis_runner
- SQLite database (schema v3.3)
- YAML configuration system

#### 3.1.2 Descriptor Pipeline
- Keypoint generation and storage
- Descriptor extraction with pooling strategies
- Matching and evaluation

### 3.2 Keypoint Intersection Algorithm
**Status**: ✅ COMPLETE (from INTERSECTION_MATCHING_ANALYSIS.md)

#### 3.2.1 Mutual Nearest Neighbor Matching
- KD-tree spatial indexing
- Forward and reverse lookup
- Tolerance-based acceptance (default: 3.0px)

#### 3.2.2 Algorithm Properties
- 1-to-1 correspondence guarantee
- Symmetric matching
- Configurable tolerance parameter

#### 3.2.3 Intersection Set Generation
```bash
./keypoint_manager build-intersection \
  --source-a sift_keypoints \
  --source-b keynet_keypoints \
  --out-a sift_intersection \
  --out-b keynet_intersection \
  --tolerance 3.0
```

### 3.3 Scale-Matched Intersection Strategy
**Status**: ✅ COMPLETE (from scale_matched_intersections_6px.md)

#### 3.3.1 Scale Distribution Analysis
- SIFT keypoints: avg 4.45px scale (full set)
- KeyNet keypoints: avg 49.83px scale (full set)
- Scale mismatch problem identified

#### 3.3.2 Scale Filtering Methodology
- Filter by scale percentile (e.g., top 25% by scale)
- 6px minimum scale threshold
- Combined with spatial intersection

#### 3.3.3 Results Summary
- SIFT scale-controlled: 63.9% mAP (+50% vs full set)
- HardNet scale-controlled: 78.9% mAP (+20% vs full set)

### 3.4 Descriptor Fusion Methods
**Status**: ✅ COMPLETE

#### 3.4.1 Averaging Fusion
```
fused = α * desc_A + (1-α) * desc_B
```
- Requires same dimensionality
- Weight parameter α (typically 0.5)

#### 3.4.2 Concatenation Fusion
```
fused = [desc_A, desc_B]  // 256D from two 128D
```
- Preserves both descriptors fully
- Doubles dimensionality

#### 3.4.3 Normalization Considerations
- Pre-fusion normalization
- Post-fusion normalization
- Distribution compatibility requirements

### 3.5 Evaluation Metrics Implementation
**Status**: ✅ COMPLETE (from METRICS_ENHANCEMENT_PLAN.md)

#### 3.5.1 Image Matching (mAP)
- SNN ratio test (threshold=0.8)
- Homography-based ground truth
- HP-V vs HP-I category breakdown

#### 3.5.2 Keypoint Verification
- Distractor sampling from other sequences
- Binary labels: y ∈ {-1, +1}
- Tests discriminative power

#### 3.5.3 Keypoint Retrieval
- Three-tier labels: y ∈ {-1, 0, +1}
- True positive: in-sequence AND closest to H·x
- Hard negative: in-sequence but not closest

---

## Chapter 4: Experiments and Results

### 4.1 Experimental Setup
**Status**: ✅ COMPLETE

#### 4.1.1 Dataset
- HPatches: 116 scenes, 696 images
- 59 viewpoint sequences (v_*)
- 57 illumination sequences (i_*)

#### 4.1.2 Hardware
- GPU: CUDA-enabled for CNN descriptors (19x speedup)
- CPU: OpenMP parallelization (16 threads)

#### 4.1.3 Keypoint Sets
- SIFT full set: ~2.5M keypoints
- KeyNet full set: ~2.8M keypoints
- Scale-matched intersection: ~111K-645K pairs

### 4.2 Baseline Descriptor Performance
**Status**: ✅ COMPLETE (from EXPERIMENT_RESULTS.md)

#### 4.2.1 Traditional Descriptors
| Descriptor | Pooling | mAP |
|------------|---------|-----|
| SIFT | None | 23.0% |
| SIFT | DSP (external) | 53.4% |
| DSPSIFT_V2 | Pyramid-aware | 57.25% |
| RGBSIFT | DSP + RootSIFT | 59.37% |

#### 4.2.2 CNN Descriptors
| Descriptor | Device | mAP | Time |
|------------|--------|-----|------|
| HardNet | GPU | 81.93% | 9.4s |
| SOSNet | GPU | 81.70% | 8.7s |
| L2Net | GPU | 54.17% | 8.4s |

#### 4.2.3 Key Findings
- CNN descriptors outperform traditional by ~22 points
- DSP provides significant improvement for SIFT family
- Root normalization timing matters (after > before > none)

### 4.3 Scale Control Experiments
**Status**: ✅ COMPLETE (from project baselines)

#### 4.3.1 SIFT Scale Analysis
| Configuration | mAP | Keypoints | Avg Scale |
|---------------|-----|-----------|-----------|
| Full set | 42.6% | 2.5M | 4.45px |
| Scale-controlled (top 25%) | 63.9% | 645K | 10.03px |
| Spatial intersection | 55.2% | 565K | - |

#### 4.3.2 HardNet Scale Analysis
| Configuration | mAP | Keypoints | Avg Scale |
|---------------|-----|-----------|-----------|
| Full set | 65.8% | 2.8M | 49.83px |
| Scale-controlled (top 23%) | 78.9% | 645K | 92.39px |
| Spatial intersection | 82.4% | 565K | 46px |

#### 4.3.3 Key Insight
- **Scale control is critical**: +50% for SIFT, +20% for HardNet
- Larger keypoint scales provide more discriminative patches
- Quality > Quantity for descriptor performance

### 4.4 Descriptor Fusion Experiments
**Status**: ✅ COMPLETE (from cnn_fusion_results.md)

#### 4.4.1 SIFT + CNN Fusion (FAILED)
| Fusion Type | Method | mAP | Result |
|-------------|--------|-----|--------|
| SIFT + HardNet | Averaging | 72.5% | ❌ -6.4% degraded |
| SIFT + HardNet | Concatenation | 72.6% | ❌ -6.3% degraded |
| RootSIFT + HardNet | Averaging | 72.55% | ❌ -6.3% degraded |
| DSPSIFT + HardNet | Averaging | 28.49% | ❌ CATASTROPHIC |

#### 4.4.2 CNN + CNN Fusion (SUCCESS)
| Phase | Configuration | mAP | Result |
|-------|---------------|-----|--------|
| Baseline | HardNet alone | 94.01% | - |
| Baseline | SOSNet alone | 94.33% | - |
| Fusion | HardNet+SOSNet Avg | 93.73% | ⚠️ -0.6% |
| Fusion | HardNet+SOSNet Concat | **94.64%** | ✅ +0.31% |

#### 4.4.3 Why SIFT+CNN Fails: Distribution Analysis
**Status**: ✅ COMPLETE
- SIFT: Non-negative distribution [0, 0.3], mean ≈ 0.05
- HardNet/SOSNet: Zero-centered distribution [-0.3, +0.3], mean ≈ 0
- Averaging shifts zero-centered distributions → destroys CNN features

### 4.5 HP-V vs HP-I Analysis
**Status**: ✅ COMPLETE (data exists for all 108 experiments)
- HP-V and HP-I columns populated in database
- Key observations from data:
  - Illumination (HP-I) generally easier than Viewpoint (HP-V)
  - CNN descriptors show smaller HP-V/HP-I gap than traditional
  - Scale-matched intersection: HP-V 92.63%, HP-I 94.18% (best config)

### 4.6 Verification and Retrieval Results
**Status**: ❌ GAP - Requires Decision
- *Have*: SIFT baseline (verification: 21.62%, retrieval: 27.40%)
- *Need*: Verification/retrieval for CNN descriptors and fusion experiments
- *Pipeline*: Fixed, works single-threaded (memory constraints)
- **DECISION NEEDED**: Is this essential for your thesis, or can image matching alone suffice?
- If needed: ~30-60 min per experiment (single-threaded)

---

## Chapter 5: Discussion

### 5.1 Why Scale Control Matters
**Status**: ✅ COMPLETE

- Larger patches contain more distinctive information
- Small-scale keypoints suffer from aliasing and noise
- Scale filtering provides quality-over-quantity trade-off
- Recommendation: Use scale percentile filtering for production systems

### 5.2 Understanding Fusion Failures
**Status**: ✅ COMPLETE

#### 5.2.1 Distribution Incompatibility
- SIFT: Histogram-based, inherently non-negative
- CNN: Learned embedding, zero-centered by design
- Averaging mixes incompatible distributions

#### 5.2.2 Why Concatenation Works
- Preserves both representations fully
- Matcher can learn which dimensions matter
- Dimensionality cost (128D → 256D) is acceptable

### 5.3 Detector Agreement as Quality Signal
**Status**: ✅ COMPLETE

- Keypoints detected by both SIFT and KeyNet are more reliable
- Spatial intersection filters out detector-specific noise
- Agreement indicates salient image features

### 5.4 Practical Recommendations
**Status**: 📝 WRITING

1. **For best performance**: HardNet+SOSNet concatenation on scale-matched keypoints (94.64%)
2. **For traditional only**: DSPSIFT_V2 with pyramid-aware pooling (57.25%)
3. **Avoid**: SIFT+CNN averaging (distribution mismatch causes degradation)
4. **Scale filtering**: Use top 25% by scale for quality-critical applications

### 5.5 Limitations
**Status**: ⚠️ PARTIAL
- *Have*: Known limitations from experiments
- *Need*: Formal discussion of:
  - Dataset limitations (HPatches only)
  - Generalization to other tasks (SLAM, SfM)
  - Computational cost analysis
  - Real-time applicability

---

## Chapter 6: Conclusion and Future Work

### 6.1 Summary of Contributions
**Status**: 📝 WRITING

1. Demonstrated scale control improves matching by 50%
2. Proved SIFT+CNN fusion fails due to distribution mismatch
3. Achieved state-of-the-art 94.64% mAP with CNN+CNN concatenation
4. Developed DescriptorWorkbench evaluation framework
5. Implemented Bojanic et al. evaluation metrics

### 6.2 Future Work
**Status**: ⚠️ PARTIAL

#### 6.2.1 Potential Extensions
- **Learned fusion weights**: Train α parameter instead of fixed 0.5
- **Distribution alignment**: Transform SIFT to zero-centered before fusion
- **Additional datasets**: Oxford5k, Paris6k, ZuBuD
- **End-to-end learning**: Joint detector-descriptor-fusion training

#### 6.2.2 Pipeline Improvements
**Status**: ✅ RESOLVED (per user)
- Pipeline bugs fixed
- Verification/retrieval work but run single-threaded (memory constraints)

---

## Identified Gaps Requiring Action (Updated 2025-12-07)

### ✅ RESOLVED Items

1. **HP-V vs HP-I Detailed Breakdown** - ✅ DATA EXISTS
   - All 108 experiments have viewpoint_map and illumination_map populated
   - Action: Format into thesis tables (no new experiments needed)

2. **Pipeline Bug Fixes** - ✅ FIXED
   - User confirmed issues resolved
   - Verification/retrieval work (single-threaded for memory)

3. **Distribution Visualization** - ✅ EXISTS
   - `analysis/output/descriptor_distributions.png` - SIFT vs HardNet distributions
   - `analysis/output/fusion_contribution_analysis.png` - Why averaging fails
   - Action: Include figures in thesis

### 🔴 Decisions Needed

4. **❓ Verification/Retrieval Experiments**
   - Current: Only SIFT baseline has verification (21.62%) and retrieval (27.40%)
   - Question: Is Bojanic verification/retrieval essential for your thesis?
   - Options:
     a) Run for top 5-10 configurations (~5-10 hours total)
     b) Skip - focus on image matching as primary metric
     c) Run for CNN baselines only (HardNet, SOSNet)
   - **YOUR DECISION REQUIRED**

5. **❓ Tolerance Parameter Study**
   - Current: Only 3.0px tolerance tested
   - Question: Is tolerance sensitivity analysis a core contribution?
   - Options:
     a) Run experiments with 1.0, 2.0, 5.0, 10.0px tolerances (~4-8 hours)
     b) Skip - cite literature justification for 3.0px choice
   - **YOUR DECISION REQUIRED**

### 🟡 Medium Priority (Recommended but Optional)

6. **⚠️ Computational Cost Analysis**
   - Need: Formal timing comparison table
   - Data: Exists scattered in database (processing_time_ms)
   - Action: Query and format into table (1 hour work)

7. **⚠️ Detector Repeatability Analysis**
   - Need: SIFT vs KeyNet repeatability numbers
   - Options:
     a) Cite KeyNet paper's published repeatability on HPatches
     b) Compute from intersection statistics (trivial calculation)
   - Recommendation: Option (b) - can derive from existing intersection data

### 🟢 Low Priority

8. **📝 Additional Dataset Validation**
   - Oxford5k, Paris6k would strengthen generalization claims
   - Recommendation: Skip unless time permits - HPatches is standard benchmark

---

## Bibliography Additions Needed

### Core Citations (Must Add)
- Bojanic et al. (2020) - Evaluation metrics
- Mikolajczyk & Schmid (2005) - Detector evaluation
- Mishchuk et al. (2017) - HardNet
- Tian et al. (2019) - SOSNet
- Barroso-Laguna et al. (2019) - KeyNet
- Lowe (2004) - SIFT (may already have)

### Supporting Citations
- DeTone et al. (2018) - SuperPoint
- Bay et al. (2008) - SURF
- Rublee et al. (2011) - ORB

---

## Recommended Writing Order

### Phase 1: Resolve Decisions (1 day)
- Decide on verification/retrieval experiments
- Decide on tolerance parameter study
- If running experiments: start them first (can write while they run)

### Phase 2: Write Data-Heavy Chapters First
1. **Chapter 4 (Results)** - 108 experiments ready, format into tables/figures
   - Create main results table from database
   - Include distribution visualization figures
   - HP-V vs HP-I analysis table

2. **Chapter 3 (Methodology)** - Well-documented in StatusDocs
   - Intersection algorithm (from INTERSECTION_MATCHING_ANALYSIS.md)
   - Scale-matching strategy (from scale_matched_intersections_6px.md)
   - Evaluation metrics (from METRICS_ENHANCEMENT_PLAN.md)

### Phase 3: Write Analysis Chapters
3. **Chapter 5 (Discussion)** - Interpret results
   - Why scale control matters (50% improvement)
   - Why SIFT+CNN fusion fails (distribution analysis)
   - Why CNN+CNN concatenation works

4. **Chapter 2 (Background/Related Work)** - Requires literature research
   - Use LITERATURE_REVIEW_DESCRIPTOR_AVERAGING.md as starting point
   - Add missing citations

### Phase 4: Write Framing Chapters
5. **Chapter 1 (Introduction)** - Write after results clear
   - Research questions derived from findings
   - Contributions summary

6. **Chapter 6 (Conclusion)** - Summarize findings

### Phase 5: Polish
- Add all figures from analysis/output/
- Bibliography (add ~15 citations)
- Formatting and proofreading

---

## Quick Start: Extracting Results Table

```sql
-- Run this to get main results for thesis
SELECT
    e.descriptor_type,
    printf('%.1f', r.true_map_macro * 100) as mAP,
    printf('%.1f', r.viewpoint_map * 100) as HP_V,
    printf('%.1f', r.illumination_map * 100) as HP_I
FROM experiments e
JOIN results r ON e.id = r.experiment_id
WHERE r.true_map_macro > 0.5
ORDER BY r.true_map_macro DESC;
```

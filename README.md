# Single subject task fMRI
Please note this is a learning project, and does not report scientific findings.

Description: I conducted an end-to-end analysis of one participant (sub-1004) from the OpenNeuro dataset ds006039, Category-specific Associative Inference in Memory.

Dataset used: OpenNeuro ds006039, subject sub-1004 (one of 41), doi:10.18112/openneuro.ds006039.v1.0.2,

Experimental design: 
Participants completed study-test phases of the Associative Inference in Memory task while undergoing fMRI scanning. During the study phase, they encoded overlapping AB and BC pairs. A and C items were always objects. The linking B item was either a face or a scene. At test, memory for the direct (AB, BC) and indirect associations (inferred AC) was tested. (taken from:  https://direct.mit.edu/jocn/article-abstract/38/1/100/131668/Scene-sensitive-Medial-Temporal-Lobe-Subregions?redirectedFrom=fulltext)

Subject-1004 data: 
8 blocks * 188 volumes 
Odd blocks are Scene and even blocks are Face
Each block has study (n=16) and test(n=24) trials, AC trials only included in the testing phase


Workflow:
Preprocessing using SPM (standalone version 26.10.01)
Realignment: Estimate and reslice, register to mean, 8 sessions x 188 frames; 6 motion parameters per run
↓
Coregistration:  T1w and functional image 1 from task run 1
↓
Segmentation: T1w segmentation; forward (and inverse) deformation fields
↓
Normalisation: Functional images written to MNI space at 1.7 mm
References: 
1. Andy’s Brain Book on SPM
2. Principles of fMRI: https://www.youtube.com/@principlesoffmri6920

Data analysis with Nilearn in Python
Motion QC: Framewise displacement (Power et al., 2012), summarised per run
↓
Atlas overlay: Schaefer 2018, 100 parcels; detrend, 128 s high-pass, z-score, motion regressors; per run
↓
First level GLM: Face and Scene, one event table per run
↓
Exploratory Ward Parcellation fitted on the functional data
References:
1. Handbook of Functional MRI (Poldrack et al)
2. https://nilearn.github.io/stable/index.html

Figures attached
1. Schafer atlas overlay on mean functional image
2. Schafer contours  on mean functional image
3. Colourmap for visualisation
4. Study Phase (Face - Scene, between runs)
5. TImeseries : bold data and motion
6. Ward parcellation

Findings:
Motion. Maximum framewise displacement per run ranged from 0.23 to 0.90 mm (the largest, in run 5, is a brief spike). Translation shows a slow, near-linear drift of about 2 mm along y within each run that resets at run boundaries. My interpretation is scanner drift rather than head movement, but I have not tested that. Motion parameters enter both the GLM and ROI extraction as confounds.
First-level GLM, study phase: Face minus Scene, study trials only. Posterior occipital cortex show a Scene>Face effect (blue). Face selective FFA effect was not seen.
The map is shown at |z| > 3.1 (uncorrected, for display); peak |z| is about 8. 
Cluster locations are described by eye and have not been labelled against an atlas.
A control contrast on AC test trials (even runs minus odd runs) gave much weaker effects (peak |z| about 6, against about 33 for an earlier Face - Scene map pooling all trials).

Limitations: 
1. Single subject design
2. Between run contrast confounded by differing factors like drift, motion, fatigue, scanner state
3. Runs are combined as fixed effects, so z-values reflect within-run noise only and are inflated relative to a proper random-effects analysis.
4. Figures use an uncorrected display threshold, not corrected inference.
5. Field of view obscure basal structures like ITL and cerebrum.
6. Github copilot was used for debugging code


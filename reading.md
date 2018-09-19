---
layout: page
title: Reading
permalink: /reading/
order: 3
---
Below is a growing collection of medical literature I have read along with some summary thoughts.

Genetics
===
1. 07/2018 -- PrimateAI ([paper](../download/201807_primateai.pdf)). 
	*Sundaram, et. al.* published a deep neural network trained on a set of hundreds of thousands of common variants using a small population of 6 non-human primate species, and demonstrated that their new tool is powerful at classifying benign and pathogenic variants in humans. Notably, PrimateAI is unbiased compared to prior pathogenicity classifiers as it does not take human-classified ClinVar variants as input. PrimateAI also incorporates a secondary structure prediction model and solvent accessibility prediction model which takes as input a variant and its surrounding 102 amino acid sequence.

2. 02/2017 -- Cardiovascular polygenic risk score ([paper](../download/201702_prs_cardiovascular.pdf)).
	*Natarajan, et. al.* describe developing a polygenic risk score from 57 SNPs which predicted additional benefit from statin therapy. This study demonstrated several important points. 1) Statins confer a greater risk reduction in those patients at high genetic risk, determined by the polygenic risk score. Interestingly this group does not on average have higher LDL levels compared to lower genetic risk subgroups. 2) Asymptomatic individuals with no history of coronary heart disease have higher burden of subclinical atherosclerosis with higher PRS. The PRS only incorporates common genetic variation but is not validated in patients with familial hypercholesterolemia.

3. 03/2017 -- Rare and common variants in LPL associated with coronary artery disease ([paper](../download/201703_rare_common_cvd.pdf)).
	*Khera, et. al.* sequenced the protein-coding regions of lipoprotein lipase from several case-control cohorts to characterize rare damaging mutations within *LPL*. Interestingly, heterozygous LPL deficiency was correlated with early-onset CAD, possibly due to impaired lipolysis of triglyceride-rich lipoproteins that penetrate directly into the arterial wall. The authors also performed common variant analysis and characterized an odds ratio of 1.51 for CAD with common *LDL* locus variants. Importantly, this study has limitations characteristic of those involving variant annotations; specifically, the authors used prediction algorithms (LRT score, MutationTaster, PolyPhen-2 HumDiv, PolyPhen-2 HumVar, and SIFT), and did not functionally validate rare missense variants -- this allows for the possibility ofimpact severity misclassification.

4. 07/2018 -- Type II diabetes genome-wide association study ([paper](../download/201807_gwas_t2d.pdf)).
	*Xue, et. al.* 

5. 09/2018 -- Fine-mapping loci implicated in type 1 diabetes and rheumatoid arthritis ([paper](../download/201809_finemap_ra_t1d.pdf)). 

Hematology
===
1. 07/2018 -- CRIPSR screen identifies HRI as fetal globin regulator ([paper](../download/201807_hri_crispr.pdf)). 
	*Grevet, et. al.* developed a CRISPR-Cas9 screen to target protein kinases, which theoretically are easier to target by small molecules. They identified the heme-regulated inhibitor HRI (a.k.a. EIF2AK1) as an inducer of HbF, and found that although HRI has been shown previously to play a role in global translation, the elevation of HbF was remarkably specific. In follow-up experiments, they used short-hairpin RNA to knock down HRI and demonstrated 1) reproducible HbF elevations, 2) diminished sickling of CD34$$^+$$ HbSS cells, 3) decreased BCL11A protein expression with HRI knockdown, and 4) unimpaired erythrocyte maturation with HRI knockdown. This evidence points toward HRI as a potential target for HbF induction and characterizes HRI as a potential upstream player of the BCL11A pathway. However, importantly HRI has a role in global protein translation, which may make clinical translation difficult. Furthermore, HRI$$^{-/-}$$ mice have been previously shown to display impaired adaptation to iron deficiency, and had phenotypes of thalassemia.

2. 04/2018 -- NETs promote thrombosis in MPNs ([paper](../download/201804_nets_mpn_thrombosis.pdf)). I created a presentation for my pediatric hematology rotation describing this paper in detail. The presentation can be found [here](../download/201804_netosis_presentation.pdf). Please note that there are several slides from the beginning of this slide deck that have been redacted for patient confidentiality.

Critical Care
===
1. 06/2018 -- *(review)* Upper GI bleed prophylaxis in hospitalized patients ([paper](../download/201806_gi_ppx_review.pdf)).

Theory of Computation
===
1. 02/2015 -- NP-complete problems and physical reality ([paper](../download/201502_NP_hard.pdf)).
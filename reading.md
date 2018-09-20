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
	*Westra, et. al.* fine-mapped 76 loci associated with type 1 diabetes and rheumatoid arthritis, and identified potentially causal missense and noncoding variants. They followed up these findings with functional assays which identified three variants that demonstrated allele-specific protein binding and differential enhancer activity.

Hematology
===
1. 07/2018 -- CRIPSR screen identifies HRI as fetal globin regulator ([paper](../download/201807_hri_crispr.pdf)). 
	*Grevet, et. al.* developed a CRISPR-Cas9 screen to target protein kinases, which theoretically are easier to target by small molecules. They identified the heme-regulated inhibitor HRI (a.k.a. EIF2AK1) as an inducer of HbF, and found that although HRI has been shown previously to play a role in global translation, the elevation of HbF was remarkably specific. In follow-up experiments, they used short-hairpin RNA to knock down HRI and demonstrated 1) reproducible HbF elevations, 2) diminished sickling of CD34$$^+$$ HbSS cells, 3) decreased BCL11A protein expression with HRI knockdown, and 4) unimpaired erythrocyte maturation with HRI knockdown. This evidence points toward HRI as a potential target for HbF induction and characterizes HRI as a potential upstream player of the BCL11A pathway. However, importantly HRI has a role in global protein translation, which may make clinical translation difficult. Furthermore, HRI$$^{-/-}$$ mice have been previously shown to display impaired adaptation to iron deficiency, and had phenotypes of thalassemia.

2. 04/2018 -- NETs promote thrombosis in MPNs ([paper](../download/201804_nets_mpn_thrombosis.pdf)). I created a presentation for my pediatric hematology rotation describing this paper in detail. The presentation can be found [here](../download/201804_netosis_presentation.pdf). Please note that there are several slides from the beginning of this slide deck that have been redacted for patient confidentiality.

3. 07/2018 -- L-Glutamine reduces pain crises in sickle cell disease ([paper](../download/201807_glutamine_scd.pdf)).
	*Niihara, et. al.* demonstrate through a randomized clinical trial--involving 152 patients that received L-glutamine for 48 weeks and 78 patients that received placebo--that pharmaceutical-grade L-glutamine reduces the number of pain crises, time to pain crisis, number of hospitalizations, and incidence of acute chest syndrome in patients with sickle cell disease and sickle-$$\beta_0$$ thalassemia. However, the L-glutamine group did endorse more non-cardiac pain, low-grade nausea, and musculoskeletal pain. Two deaths occurred in the L-glutamine group of cardiac causes, though both these patients had been very sick upon enrollment, and there is no known evidence that L-glutamine contributes to cardiac events. This clinical trial included a diverse group of patients, with multiple genotypes, a large age spread, among other variables, and more detailed subgroup analysis will be helpful to identify those individuals that may benefit most from L-glutamine supplementation. Notably, a large number of patients were treated concomitantly with hydroxyurea, and the paper does not stratify based on hydroxyurea use.

Critical Care
===
1. 06/2018 -- *(review)* Upper GI bleed prophylaxis in hospitalized patients ([paper](../download/201806_gi_ppx_review.pdf)).

Preventive Medicine
===
1. 09/2018 -- Low-dose aspirin in the healthy elderly does not prolong disability-free survival ([paper](../download/201809_aspirin_elderly.pdf)).
	*McNeil, et. al.* demonstrated over a 5-year span that low-dose aspirin therapy in healthy seniors did not prolong disability free survival, and led to a statistically significant increase in major hemorrhagic events. The cohort that was recruited included 19,114 persons, roughly half of which were randomized to receive 100 mg aspirin qday, and the other half of which received placebo. The trial was terminated at a median of 4.7 years of follow-up. There were 21.5 events per 1000 person-years in the aspirin group, compared to 21.2 events per 1000 person-years in the placebo group. Events were defined as death, dementia, or persistent physical disability. Limitations of this particular trial include a relatively short duration of intervention and a relatively old cohort of patients. Importantly, some participants had been regularly taking low-dose aspirin prior to the trial; the results of the paper did not stratify for these people, and it remains to be determined how the conclusion of this paper will contribute to the question of whether healthy elderly patients who had been using aspirin for primary prevention should continue or discontinue its use. Also notable was that the population in question was predominantly white.

Theory of Computation
===
1. 02/2015 -- NP-complete problems and physical reality ([paper](../download/201502_NP_hard.pdf)).
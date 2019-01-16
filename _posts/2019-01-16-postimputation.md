---
layout: post
title: Notes on Post-imputation Analysis
date: 2019-01-16 13:02:00 -0700
categories: notes
math: true
comments: true
author: Aaron
---
*in progress*

Below is a pipeline that takes a reference panel and genotyped dataset and carries out post-imputation accuracy/yield analysis.

~~~ bash
## Measuring the accuracy and yield of imputation results
## In: reference panel, PLINK files (post-thinning 5%)
## Out: accuracy metrics, as defined below

# Yield1 = % recovered SNPs
# Yield2 = [# INFO > 0.9] / [# removed SNPs]
# Accuracy (per SNP) = % individuals with correct matching genotype / total individuals in study

# Requires IMPUTE2 and SHAPEIT in working directory

# 01 Toolbelt

reuse -q PLINK2

# 02 Get the SNPs in the reference panel for imputation. Here, the reference panel is 
# the merged 1kG + WGS panel

for chr in {1..22}; do
    cat /broad/sankaranlab/acheng/wgs_impute/new_reference/chr${chr}.impute.legend | \
    gawk -v chr=${chr} '$1!="id" {print chr"_"$2}' >> snps-reference.txt;
done

# 03 Get a list of positions of SNPs that are in the target set and/or in the reference set
# NOTE: step7 should have 5% of SNPs randomly removed (and recorded); these SNPs will be 
# reimputed and compared to the hard-genotypes to assess accuracy

plink --bfile plink_files/step7.chr1 --recode --out plink_files/step7.chr1

gawk '{print $1"_"$4}' plink_files/step7.chr1.map > snps-reference-and-rawdata
sort snps-reference.txt | uniq >> snps-reference-and-rawdata
sort snps-reference-and-rawdata | uniq -d | gawk -F "_" '{$3=$2+1; print $1, $2,
$3, "R"NR}' > snps-reference-and-rawdata-duplicates

# 04 Prepare for SHAPEIT

plink --file plink_files/step7.chr1 \
--extract snps-reference-and-rawdata-duplicates \
--range --make-bed --out plink_files/step8.for-impute.plink

# 05 Remove duplicate SNPs

plink --bfile plink_files/step8.for-impute.plink --list-duplicate-vars ids-only suppress-first \
--out plink_files/chr1_dupes;

plink --bfile plink_files/step8.for-impute.plink --exclude plink_files/chr1_dupes.dupvar \
--make-bed --out plink_files/step9.nodupes.chr1

# 06 Run SHAPEIT

namefile="plink_files/step9.nodupes.chr1";
./shapeit --input-bed ${namefile}.bed ${namefile}.bim ${namefile}.fam \
--input-map /broad/sankaranlab/acheng/wgs_impute/_hg38/genetic_maps/chr1_genetic_map.txt \
--output-max ${namefile}.phased --thread 4 --output-log ${namefile}.phased;

# 07 Imputation (Note: calls 06.2_helper.sh, a helper script that imputes per 500,000 base pairs)

maxPos=$(gawk '$1!="position" {print $1}' \
'/broad/sankaranlab/acheng/wgs_impute/_hg38/genetic_maps/chr1_genetic_map.txt' | sort -n \
| tail -n 1);
nrChunk=$(expr ${maxPos} "/" 500000);
nrChunk2=$(expr ${nrChunk} "+" 1);
qsub -cwd -N array${chr} -b y -t 1-${nrChunk2} -tc 500 -l h_vmem=18g,h_rt=6:00:00 ./06.2_helper.sh;

# 08 Merge all chunks

for chunk in {1..500}; do
  awk 'NR>=2' plink_files/chr1.impute.chunk${chunk} >> chr1.impute.all_chunks;
done

# 09 Find the intersection of removed SNPs and imputed SNPs. This allows us to calculate Yield1 immediately.

# Get the positions of the SNPs we randomly removed
cut -f4 removedsnps.chr1.txt > removed_positions.txt;

# Get the positions of all of the imputed SNPs
cut -f3 -d' ' chr1.impute.all_chunks > imputed_positions.txt;

# Get the positions that are in common
comm <(sort removed_positions.txt) <(sort imputed_positions.txt) > positions_in_common.txt;

# NOTE: Yield1 will be the wc -l of positions_in_common.txt divided by wc -l of removed_positions.txt.
~~~
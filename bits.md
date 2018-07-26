---
layout: page
title: Bits
permalink: /bits/
---
Short notes for myself on various topics (to be separated into pages once this document gets too long).

Molecular biology
===
1. Polysomal profiling

BASH
===
1. **Running a job on UGER**

~~~ bash
ssh login
use UGER
ish
cd $LOCATION
~~~

Applied Mathematics
===
1. **Principal component analysis**

The goal of PCA is to identify eigenvectors with the largest eigenvalues, as these are the vectors which accounts for the maximal spread of data. 

To do this, we calculate the covariance matrix of the *mean-adjusted* data matrix ($$X - \overline{X}$$). Next we identify the eigenvectors and their associated eigenvalues of the covariance matrix.

We form a feature vector whose columns contain the eigenvectors we desire. Those eigenvectors with the largest associated eigenvalues will contain the greatest spread of data along that vector.

Deriving the new dataset simply involves $$X' = F\times X^T$$, where $$F$$ represents the feature vector containing those eigenvectors we desire.

Reconstructing the original data from our feature vector: $$X'' = (F^T \times X') + \overline{X}$$.

Genetics
===
1. **Using PLINK2 for PCA of large .vcf**

Prune the .vcf first.
~~~ bash
use PLINK2
plink --vcf sampleVCF.vcf.gz --maf 0.01 --indep-pairwise 50 5 0.2 --out sampleVCF_clean
~~~

Calculate identity by descent.
~~~ bash
plink --vcf sampleVCF.vcf.gz --genome gz --out sampleVCF_clean --extract sampleVCF_clean.prune.in
~~~

The output should contain sampleVCF-clean.genome.gz, which can be imported into R for visualization. 

In RStudio, we can plot the file to visualize relatedness by doing the following:
~~~ R
ibd <- read.table("sampleVCF_clean.genome.gz", hea=T, as.is=T)
View(ibd)
hist (ibd$PI_HAT, breaks = 100)
~~~

To zoom in along the y-axis you can run the following line:
~~~ R
hist ( ibd$PI_HAT, breaks = 100, ylim = c(0,1000))
~~~

The result should resemble something like this:
![IBD](../images/IBD_plot.png)

We'll pick one sample from each closely-related pair to exclude, and record the related samples for later input into plink.
~~~ R
exclusions = ibd[ ibd$PI_HAT > 0.2, c('FID2','IID2')]
write.table( exclusions, file="related_samples.txt", col.names = F, row.names = F, quote = F)
~~~

Now, we use plink to compute principal components.
~~~ bash
plink --vcf sampleVCF.vcf.gz --extract sampleVCF_clean.prune.in --remove related_samples.txt --pca var-wts -out sampleVCF_clean
~~~


2. **VCFtools**


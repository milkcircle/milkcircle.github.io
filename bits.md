---
layout: page
title: Bits
permalink: /bits/
---
Short notes for myself on various topics (to be separated into pages once this document gets too long).

Molecular biology
===
Polysomal profiling

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
{% highlight bash linenos %}
use PLINK2
plink --vcf Vertex_Sankaran_1474_Samples_MEGA_VCF.vcf.gz --maf 0.01 --indep-pairwise 50 5 0.2 --out Vertex_clean
{% endhighlight %}

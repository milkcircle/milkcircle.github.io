---
layout: page
title: Bits
permalink: /bits/
order: 4
---
Short notes for myself on various topics (to be separated into pages once this document gets too long).

BASH
===
1. **Running a job on UGER**

    ~~~ bash
    ssh login
    use UGER
    ish
    cd $LOCATION
    ~~~

    Now we are in the interactive shell.

    ~~~ bash
    use UGER
    qsub {/path/to/job_script}
    ~~~

    The default settings at the Broad are as follows:

    * Memory: 1G
    * Runtime: 2h
    * OS: RedHat6
    * Cores: 1 core  
  
    You can use flags to modify $$\texttt{qsub}$$.
    ~~~ bash
    # The following runs for 12 hours, 34 min, 56 sec.
    qsub -l h_rt=12:34:56 {/path/to/job_script}

    # The following accesses 12G of memory.
    qsub -l h_vmem=12G {/path/to/job_script}

    # The following requests 4 cores and 8G memory (simply requested memory multiplied by number of cores). Use the pe and binding flags.
    qsub -pe smp 4 -binding linear:4 -l h_vmem=2G {/path/to/job_script}

    # Execute the job as a binary.
    qsub -b y command

    # Name the job "someName".
    qsub -N someName {/path/to/job_script}

    # Determine the output of the job.
    qsub -o /path/to/output {/path/to/job_script}
    ~~~ 

    Further documentation on Grid Engine found [here](UsersGuideGE.pdf).

Applied Mathematics
===
1. **Principal component analysis**

	The goal of PCA is to identify eigenvectors with the largest eigenvalues, as these are the vectors which accounts for the maximal spread of data. 

	To do this, we calculate the covariance matrix of the *mean-adjusted* data matrix ($$X - \overline{X}$$). Next we identify the eigenvectors and their associated eigenvalues of the covariance matrix.

	We form a feature vector whose columns contain the eigenvectors we desire. Those eigenvectors with the largest associated eigenvalues will contain the greatest spread of data along that vector.

	Deriving the new dataset simply involves $$X' = F\times X^T$$, where $$F$$ represents the feature vector containing those eigenvectors we desire.

	Reconstructing the original data from our feature vector: $$X'' = (F^T \times X') + \overline{X}$$.

2. **Power spectrum**

	A quick implementation of a power spectrum in $$\texttt{R}$$. Briefly, this function takes as input a complex waveform, then outputs the amount of energy at each frequency, and plots the power vs. frequency.

	~~~ R
	TimeInterval = 0.01
	MAXF = 1/(2*TimeInterval)
	NPTS = 10
	twopi = 6.283
	loop_vector = seq(1, MAXF, TimeInterval)

	power_vector = vector(,length(loop_vector))

	test_sin <- function(x){
  	  1*sin(twopi*1000*x) + 3*sin(twopi*2000*x) + 5*sin(twopi*3000*x)
	}

	power_spectrum <- function(FUN){
  	  for(f in 1:length(loop_vector))
  	  {
        e = loop_vector[f]
        realf = 0
        imagef = 0
        temp_arg = twopi * e * TimeInterval
        for (i in 1:NPTS)
        {
      	  realf = realf + FUN(i) * cos(temp_arg*i)
      	  imagef = imagef + FUN(i) * sin(temp_arg*i)
        }
        power_vector[f] <- realf ** 2 + imagef ** 2
  	  }
  	  plot(power_vector)
	}
	~~~

plink
===

1. **vcf to plink format**

	~~~ bash
	plink --vcf sampleVCF.vcf --double-id --make-bed --out {output}
	~~~

	Note that the $$\texttt{--double-id}$$ flag is used so that both the family and within-family IDs are set to the sample ID. $$\texttt{--make-bed}$$ creates a new PLINK binary fileset, and $$\texttt{--out}$$ determines the name of the output file.

2. **LD pruning of SNPs with plink**

	Our goal here is to remove correlated pairs of SNPs so the remaining SNPs are roughly independent. To do this, run the following command:

	~~~ bash
	plink --vcf {sampleVCF.vcf.gz} --maf 0.01 --indep-pairwise 50 5 0.2 --out {sampleVCF_clean}
	~~~

	This leaves SNPs with minor allele frequency of at least 1%, with no pairs remaining with $$r^2 > 0.2$$. 

3. **Using plink for principal component analysis (*in progress*)**

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

	Now, we use plink to compute principal components.
	~~~ bash
	plink --vcf {sampleVCF.vcf.gz} --extract {sampleVCF_clean.prune.in} --pca var-wts -out sampleVCF_clean
	~~~



R
===
1. **Subsetting columns from matrices by value**
	
	~~~ R
	subset_matrix = subset(large_matrix, select=c("value1", "value2", ...))
	~~~

2. **Writing matrices to files**

	~~~ R
	write.table(sample_matrix, file="sample_file.txt", row.names=FALSE, col.names=FALSE)
	~~~

3. **Reading RDS files**

	~~~ R
	readRDS("Path/to/RDS.rds")
	~~~
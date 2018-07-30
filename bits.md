---
layout: page
title: Bits
permalink: /bits/
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

    # Kill a job.
    qdel {jobID}
    ~~~ 

    For some reason, you can't $$\texttt{use}$$ packages easily on UGER. Therefore, you need to add the following lines to the beginning of the script for things to run smoothly without error:

    ~~~ bash
    #!/bin/bash -l

    # The following example is if I want to use Bcftools.
	source /broad/software/scripts/useuse;
	reuse -q Bcftools;
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

Ancestry mapping
===
1. **LD pruning**

	Begin with $$\texttt{SAMPLE.vcf.gz}$$ (our compressed variant call file) and $$\texttt{SAMPLE.vcf.gz.tbi}$$ (our index file for the variant call file). Using plink we will prune the $$\texttt{.vcf.gz}$$ by removing SNPs that are in linkage disequilibrium. *Note that in this example,* $$\texttt{SAMPLE.vcf.gz}$$ includes variants from a single East Asian country so is expected to be homogeneous; therefore, we can probably skip the removal of individuals who have inheritance by descent as we would normally do otherwise.

	~~~ bash
	use PLINK2
	plink --vcf SAMPLE.vcf.gz --maf 0.01 --indep-pairwise 50 5 0.2 --out SAMPLE_clean
	~~~

	The output should be a set of files with the prefix $$\texttt{SAMPLE_clean.\*}$$.

2. **Downloading 1000genomes project**

	We want to compare our sample against the 1000 genome project. To do this we will need the $$\texttt{.vcf.gz}$$ from the 1000 genome project (henceforth called 1000g). 

	~~~ bash
	prefix="ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr" ;
	suffix=".phase3_shapeit2_mvncall_integrated_v5a.20130502.genotypes.vcf.gz" ;
	for chr in {1..22} X; do
    	wget $prefix$chr$suffix $prefix$chr$suffix.tbi ;
	done
	~~~

	This will take some time, but by the end of running this script we will end up with all chromosomes from the 1000g project (the $$\texttt{.vcf.gz}$$ and $$\texttt{vcf.gz.tbi}$$) files.

	Before we merge these all together, the following procedure will use a single chromosome to save on time. Once we can verify that these work, then we can go ahead with the whole genome which will be much more time-intensive.

3. **Merge SAMPLE and 1000g**

	We want to create a merged dataset. To do this we will need to use $$\texttt{Bcftools}$$.

	First, we want two files. $$\texttt{SAMPLEsubset.vcf}$$ will be those records from $$\texttt{SAMPLE.vcf.gz}$$ which are also present in $$\texttt{1000g.vcf.gz}$$. We also want $$\texttt{1000subset.vcf.gz}$$ which will be those records from $$\texttt{1000g.vcf.gz}$$ that are present in $$\texttt{SAMPLEsubset.vcf.gz}$$.

	~~~ bash
	use Bcftools

	# Creating SAMPLEsubset
	bcftools isec -p dir -n=2 -w1 SAMPLE.vcf.gz 1000g.vcf.gz
	mv dir/0000.vcf SAMPLEsubset.vcf
	rm -r dir/
	bgzip SAMPLEsubset.vcf
	tabix SAMPLEsubset.vcf.gz

	# Creating 1000subset
	bcftools isec -p dir -n=2 -w1 1000g.vcf.gz SAMPLEsubset.vcf.gz
	mv dir/0000.vcf 1000subset.vcf.gz
	rm -r dir/
	bgzip 1000subset.vcf
	tabix 1000subset.vcf.gz
	~~~

	Next, we want to merge these together so that there are columns for all of the individuals within the 1000g and our sample as well. 

	~~~ bash
	bcftools merge -o mergedsamples.vcf.gz 1000subset.vcf.gz SAMPLEsubset.vcf.gz
	~~~

	Now we're ready to compute our principal components.
	~~~ bash
	plink --vcf mergedsamples.vcf.gz --extract SAMPLE_clean.prune.in --pca var-wts --out mergedsamples

	This creates $$\texttt{mergedsamples.eigenvec}$$ (along with several other files) which contains our principal components.

	To visualize these principal components, open $$\texttt{RStudio}$$ and perform the following commands.

	~~~ R
	pcs = read.table("mergedsamples.eigenvec")
	~~~

	Note that each row in $$\texttt{pcs}$$ is an individual. Identify the point up to which the individuals from the 1000g cohort ends and the sample cohort begins, and call this $$n$$. Let the number of rows in $$\texttt{pcs}$$ be $$m$$.

	~~~ R
	pcs1000g = pcs[1:n,]
	plot(pcs1000g[,3], pcs1000g[,4], xlab = "PC1", ylab = "PC2")

	# Overlay the sample individuals on this principal component graph, and make the individual points yellow.
	points(pcs[n:m,3], pcs[n:m,4], col="yellow")
	~~~

BCFtools
===
1. **Intersection**

	Given a.vcf.gz and b.vcf.gz, identify those variants that are shared by both and output those lines from a.vcf.gz.

	~~~ R
	bcftools isec -p dir -n=2 -w1 a.vcf.gz b.vcf.gz
	~~~

2. **Subsetting by chromosome**

	~~~ bash
	bcftools view -r 1 -O z -o chr1.vcf.gz
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
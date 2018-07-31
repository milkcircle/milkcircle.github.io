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

Ancestry mapping (attempt 2)
===
1. Quality control of SAMPLE genotypes

	Make a $$\texttt{.txt}$$ file which contains a list of useful samples (i.e. those which were properly genotyped). We will select from our $$\texttt{.vcf.gz}$$ those samples with the following code.

	~~~ bash
	# We will be using plink.
	use PLINK2

	# Convert vcf to plink format.
	plink --vcf SAMPLE.vcf --out step1

	# Tell plink to keep only those samples which are genotyped properly, and 
	# output as a .bed file, which is faster to process with plink.
	plink --bfile step1 --keep SAMPLE.txt --make-bed --out step2_sub_samples
	
	# Determine the percentage of missing SNP data per sample. These are output in 
	# missing.imiss. There is also a file called missing.lmiss which is the 
	# percentage of each variant being called.
	plink --bfile step2_sub_samples --missing

	# Screen out variants for which >0.05% of individuals were not called at that 
	# variant.
	plink --bfile step2_sub_samples --geno 0.05 --make-bed --out step3_miss_snps

	# Screen out samples for which >0.02% of the variants were not called.
	plink --bfile step3_miss_snps --mind 0.02 --make-bed --out step4_miss_indiv

	# Compare the reported sex of the individual with the imputed sex
	plink --bfile step4_miss_indiv --check-sex
	~~~

	Now we can generate some graphs with our data in R.

2. Plotting sex checks

	~~~ R
	# Read in the plink .sexcheck file.
	sexcheck <- read.table("plink.sexcheck", stringsAsFactors = F, header = T)
	~~~

	In the snippet above, $$\texttt{stringsAsFactors}$$ is a modifier that determines whether a string in a dataframe should be treated as a categorical variable or a true string of characters. This field defaults to $$\texttt{TRUE}$$ but in most cases should be set to $$\texttt{FALSE}$$. The $$\texttt{header}$$ field treats the first row as the column label.

	To plot:
	
	~~~ R
	require(ggplot2)
	ggplot(sexcheck[sexcheck$F > 0,], aes(SNPSEX, F)) + geom_jitter(aes(color = as.factor(PEDSEX)))
	~~~ 

	The above line says to plot $$\texttt{sexcheck}$$ where the field $$\texttt{F}>0$$, with the x-axis as the $$\texttt{SNPSEX}$$ value and the y-axis as the $$\texttt{F}$$ value. We add in something called a jitter in order to spread the data out with a little bit of noise so that we can more easily visualize the spread of data points. We color each point depending on the value of $$\texttt{PEDSEX}$$, which we treat as a categorical variable here.

	A representative plot is shown below.
	![]({{ site.url }}/images/step5_sexcheck.png)

3. Interpreting missingness
	
	~~~ R
	# missingi will contain all individuals and their percent missingness.
	# missingl will contain all variants and their percent missingness.
	missingi <- read.table("missing.imiss", header = F, stringsAsFactors = F)
	missingl <- read.table("missing.lmiss", header = F, stringsAsFactors = F)

	# plot missingi
	ggplot(missingi, aes(x = 1- F_MISS)) + geom_histogram(bins = 50) + xlim(.95, 1)
	
	# plot missingl
	ggplot(missingl, aes(x = 1- F_MISS)) + geom_histogram(bins = 50) + xlim(.9, 1)
	~~~

	Above, we plot $$\texttt{1-F_MISS}$$ as a histogram with 50 bins and the axis ranging from 0.95 to 1 (or 0.9 to 1 in the case of $$\texttt{missingl}$$.)

	These plots can give us an easier visual way of determining which thresholds to set, for our $$\texttt{geno}$$ and $$\texttt{mind}$$ options.


Ancestry mapping (attempt 1)
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
	~~~

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

	Given $$\texttt{a.vcf.gz}$$ and $$\texttt{b.vcf.gz}$$, identify those variants that are shared by both and output those lines from $$\texttt{a.vcf.gz}$$.

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
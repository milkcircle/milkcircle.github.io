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

    To submit multiple jobs by using a loop, the following boilerplate is helpful.
    ~~~ bash
    # Loop through all 22 chromosomes
    for i in {1..22};
	do
		# Define command to run
		cmd="use Tabix; tabix imputation_files/chr${i}.filtered.vcf.gz";
		
		# Submit each command. The -b flag is important here.
		qsub -cwd -N chr${i}_filter -l h_vmem=12G -b y -o jobsoutput \
	    -e jobserror $cmd;
	
	done
    ~~~

    A better way of submitting multiple jobs is by using a job array, which can be written using the following template.
    ~~~ bash
    #!/bin/bash -l
    # Define qsub settings here
    #$ -cwd -N {job-name} -l h_vmem=15g,h_rt=10:00:00
    # Define variable for job, to be stored in ${SGE_TASK_ID}
    #$ -t 1-22

    # The below example creates dosage files for use in BOLT-LMM from EAGLE2 output.
    bcftools query -f '%ID\t%CHROM\t%POS\t%REF\t%ALT[\t%DS]\n' vcf/chr${SGE_TASK_ID}.filtered0.3.vcf.gz > vcf/chr${SGE_TASK_ID}.dosageFile
    ~~~

    To run the job array, run the following:
    ~~~ bash
    qsub ./job_array.sh
    ~~~

2. **Variable assignments**

    ~~~ bash
    sum=$(( 1+1*2 ))
    echo $sum
    ~~~

3. **Data wrangling**  

    In a file with many columns where you want to select all but a subset of columns, you can use the $$\texttt{--complement}$$ feature of $$\texttt{cut}$$.

    ~~~ bash
    # Retrieves all but the 13th and 14th space-separated column from input.txt
    cut --complement -f13,14 -d' ' input.txt > output.txt
    ~~~

    To insert a header into a headerless file, consider using $$\texttt{sed}$$:

    ~~~ bash
    # -i to insert text into the file, 1 to select the first line, i to insert text and newline
    sed -i '1i FID IID Age Age2' covariates.txt
    ~~~

4. **Touch**

    Recursively touch all files.

    ~~~ bash
    find . -exec touch {} \;
    ~~~

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

3. **vcf to dosage file for BOLT-LMM**

    ~~~ bash
    bcftools query -f 'ID\t%CHROM\t%POS\t%REF\t%ALT[\t%DS]\n' imputation_files/chr${i}.filtered.vcf.gz > imputation_files/chr${i}.dosageFile
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

4. **Create a vector of iterated strings**

    ~~~ R
    # To replace default column names of a table with strings that loop through 1:20
    colnames(sample_table) <- c("FID", "IID", sprintf("PrincipalComponent%s", 1:20))
    ~~~

Oncology
===
1. **[Blastic plasmacytoid dendritic cell neoplasm](../download/BPDCN.pdf)**

    BPDCN is a rare and aggressive hematologic malignancy that usually presents with skin lesions accompanied by bone marrow involvement or hematologic dissemination. Pathogenesis is unclear given the rarity of this disease. Skin lesions are typically 1) brown/purple nodules, 2) bruise-like patches, or 3) disseminated/mixed lesions. Differential diagnosis usually includes other malignancies with cutaneous manifestations, like 1) CD56+ acute myeloid leukemias, 2) nasal-type extranodal NK/T cell lymphoma, 3) subcutaneous panniculitis-like T cell lymphoma, or 4) cutaneous T cell lymphoma. 

    Evaluation of a patient with BPDCN should include a bone marrow aspiration; a CT scan of the chest, abdomen, and pelvis; and a cardiac echo if anthracyclines are considered. If flow cytometry demonstrates overexpression of BCL-2 in the tumor sample, treatment with **venetoclax** should be considered. Otherwise, combination therapy of **bortozemib** and **simvastatin** can also be considered -- in *in vitro* studies these two agents have been shown to be synergistic.

    A case study of a patient with BPDCN responsive to venetoclax is included [here](../download/201810_BPDCN.pdf).
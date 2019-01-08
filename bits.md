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

    Further documentation on Grid Engine found [here]({{ site.url }}/download/UsersGuideGE.pdf).

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

    To delete a range of jobs, run:
    ~~~ bash
    qdel {6709910..6709931}
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

    To append a column from one file to another file:

    ~~~ bash
    pr -mts' ' file1 file2

    # An example involving cut
    pr -mts' ' tmpDosage/chr$i.dosage5 <(cut -f7 -d' ' postQC/chr$i.info | awk 'NR>1') > tmpDosage/chr$i.info.dosage6;
    ~~~ 

4. **Touch**

    Recursively touch all files.

    ~~~ bash
    find . -exec touch {} \;
    ~~~

5. **Screen**

    Attach to screen

    ~~~ bash
    # See list of active screens
    screen -ls 

    # Attach to screen
    screen -rd [screen ID]
    ~~~

    Quit detached screen (useful if the screen is unresponsive)

    ~~~ bash
    screen -X -S [screen ID] quit
    ~~~

6. **awk**

    To pass a variable into $$\texttt{awk}$$:

    ~~~ bash
    awk -v var="$variable" '{ print var }'
    ~~~

7. **ls**

    I unfortunately often keep pretty messy directories. To only list directories within the current directory, the following commands work:

    ~~~ bash
    ls -d */
    echo */
    ~~~

8. **set difference in bash**

    ~~~ bash
    # First column: only those lines unique to FILE1
    # Second column: only those lines unique to FILE2
    # Third column: lines common to both files

    comm -2 -3 <(sort FILE1) <(sort FILE2) > output.txt

    # Use -1 to suppress column 1, -2 to suppress column 2, -3 to suppress column 3.
    # Remember that files must be presorted prior to using comm.
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

3. **Merging plink binary files**

    ~~~ bash
    cat plink.chr1.fam > plink.fam
    for chr in {1..22} X Y; do cat plink.chr$chr.bim; done > plink.bim
    (echo -en "\x6C\x1B\x01"; for chr in {1..22} X Y; do tail -c +4 plink.chr$chr.bed; done) > plink.bed
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

5. **Select a column from a matrix and keep it as a column matrix rather than a vector**

    ~~~ R
    X[,1,drop=FALSE]
    ~~~

6. **Find values in x but not in y**

    ~~~ R
    setdiff(x,y)
    ~~~

Molecular biology
===
1. **Riboseq**

    Uses nucleases to degrade RNA that is unbound to ribosomes; these segments can then be sequenced and mapped to known mRNA regions to determine areas of active translation. This procedure differs from polysomal profiling (which is also used to generate a translatome), and data generated demonstrate different levels of specificity. 

Oncology
===
1. **[Blastic plasmacytoid dendritic cell neoplasm](../download/BPDCN.pdf)**

    BPDCN is a rare and aggressive hematologic malignancy that usually presents with skin lesions accompanied by bone marrow involvement or hematologic dissemination. Pathogenesis is unclear given the rarity of this disease. Skin lesions are typically 1) brown/purple nodules, 2) bruise-like patches, or 3) disseminated/mixed lesions. Differential diagnosis usually includes other malignancies with cutaneous manifestations, like 1) CD56+ acute myeloid leukemias, 2) nasal-type extranodal NK/T cell lymphoma, 3) subcutaneous panniculitis-like T cell lymphoma, or 4) cutaneous T cell lymphoma. 

    Evaluation of a patient with BPDCN should include a bone marrow aspiration; a CT scan of the chest, abdomen, and pelvis; and a cardiac echo if anthracyclines are considered. If flow cytometry demonstrates overexpression of BCL-2 in the tumor sample, treatment with **venetoclax** should be considered. Otherwise, combination therapy of **bortozemib** and **simvastatin** can also be considered -- in *in vitro* studies these two agents have been shown to be synergistic.

    A case study of a patient with BPDCN responsive to venetoclax is included [here](../download/201810_BPDCN.pdf).

Infectious Disease
===
1. **Spinal brucellosis**

    Brucellosis is a zoonotic infection usually transmitted to humans through contact with infected fluids from sheep, cattle, goats, or pigs. Unpasteurized milk or cheese can also transmit brucellosis. Notably, brucellosis can cause complications in pregnancy such as spontaneous abortion; however, brucellosis is not known to be an opportunistic disease in patients with HIV. 

    Diagnosis should include culture, serology, or PCR. Imaging can provide evidence for focal disease. 

    Treat with **doxycycline** as first-line therapy, plus either streptomycin or rifampin. 

    A case study is included [here](../download/2018_spinal-brucellosis.pdf). This case describes a 62-year old man presenting with progressive back pain along with constitutional symptoms of fevers, chills, night sweats, and weight loss. He traveled to Mexico regularly and was exposed to unpasteurized dairy products. Laboratory results were notable for a mildly elevated white blood cell count and an elevated ESR. MRI spinal imaging revealed numerous contrast-enhancing areas of spondylitis along the spine, along with a psoas abscess. Culture grew *Brucella melitensis*. This patient was treated with **doxycycline** and **rifampin** for a five-month course, and had a drain placed in the psoas abscess. Symptoms resolved with follow-up, and laboratory tests normalized.

# Testing materials for day 1 

* [2022 materials](https://sydney-informatics-hub.github.io/rna-seq-pt1-quarto/)

## **TODOs**

* Restructure workshop materials 
    * Introduction
    * Set up 
    * Case study 
    * Day 1
    * Day 2 
    * Tips and tricks
* Fix samplesheet.csv strandedness 
* Create case study diagram 
* Prepare run commands 
* Prepare activities for breakout rooms

## **Instance set up** 

* Put data and backups on CVMFS 
* Day 1: nf-core workflow code base 
* Day 2: rnaseq-day2.Rmd 
* Symlink CVMFS to ~/training

Something like: 

```bash
.
└── training/
    ├── Data /
    │   ├── reference 
    │   ├── rnaseq-day2.Rmd
    │   ├── rstudio.sif
    │   └── samplesheet.csv
    ├── Day-1/
    │   ├── nf-core-rnaseq_3.12.0/
    │   │   ├── 3_12_0
    │   │   ├── configs 
    │   │   └── singularity-images
    │   └── run-scripts
    └── Day-2/
        └── rnaseq-day2.Rmd
```

## **Intro to the case study**

Learning objectives: 
* Understand experimental design considerations for DE analysis 

Case study 
* [Corley et al. (2014) paper](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-016-2801-4)

We did not include case study section in 2022 day 1 materials:  
* Explain traits associated with WBS
* Explain KO mouse model 
* Explain research questions 

Poll questions 
* Which sorts of genes do you expect will be up/downregulated in KO compared with WT?
* How might gene expression correlate with known features of WBS? 
* Can you identify a potential limiting factor in the experimental design which might affect result interpretation?

## **Intro to the workflow**

Learning objectives: 
* Understand the basic RNAseq DE data processing and analysis workflow 
* Understand why Nextflow and nf-core are good options for reproducible and portable bioinformatics workflows
* Understand in which situations the CLI is a suitable environment for RNAseq analysis 
* Understand the nf-core/rnaseq pipeline structure 
* Identify the default and required nf-core/rnaseq parameters

RNAseq workflow
* Data processing (day 1) and analysis (day 2)
* Explain the RNAseq-DE workflow and which stage we are starting day 1 at 
* Explain [nf-core/rnaseq structure](https://sydney-informatics-hub.github.io/rna-seq-pt1-quarto/notebooks/2.1_nfcore-rnaseq.html#what-tools-and-processes-does-the-pipeline-run)

Nextflow and nf-core 
* Explain why we are using nf-core and working on CLI 
* Take some details re: workflow management systems from [here](https://sydney-informatics-hub.github.io/rna-seq-pt1-quarto/notebooks/2.0_intro_to_nextflow_and_nfcore.html)
* Discuss nf-core params, tools, pipelines, documentation. As per the [customising nf-core workshop](https://sydney-informatics-hub.github.io/customising-nfcore-workshop/notebooks/2.1_design.html)

Activities
* Create sample sheet 
* Run help command 

## **QC run command** 

Learning objectives: 
* Understand how to apply parameters to an nf-core pipeline to cusomise its execution
* Understand the fastq file format 
* Learn how to interpret a FastQC report for RNAseq data 
* Learn about the benefits and drawbacks of read trimming for RNAseq-DE

Breakout rooms:
* Pre-trim fastQC report questions
    * How many sequences in one sample fq?
    * How long are sequence reads? 
    * Which part of the reads has the worst quality? 
    * Is this a good dataset? Why or why not?
    * Solutions to improve the quality? 
* Post-trim QC 
    * Which tool was used for trimming? 
    * Which tool generated quality reports before and after trimming?
    * What effect did trimming have? 

## **Continue run command**

Learning objectives: 
* Understand how to apply parameters to an nf-core pipeline to cusomise its execution
* Understand the nf-core workflow execution standard output 

Activity
* Select parameters for RNAseq run including alignment tool, quantification tool 
* Execute the workflow 
* Look through execution standard output 
* Q&A

## **Explore outputs**

Learning objectives: 
* Understand the process of mapping sequencing reads to a reference genome in a splice-aware manner 
* Understand how mapped reads are used to quantify gene counts 
* Learn how to read a MultiQC report 

Breakout rooms: 
* Read alignment and quantification 
    * Mislabelled sample exercise 
    * Explore the count matrix 

Activities 
* MultiQC report poll questions 
* Nextflow reports 

## **Reports**

Learning objectives: 
* Learn how to interpret Nextflow workflow execution reports 

Consider merging this section with the one above 
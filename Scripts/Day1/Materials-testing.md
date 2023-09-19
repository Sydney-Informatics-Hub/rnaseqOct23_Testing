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

## **QC run command** 

Learning objectives: 
* Understand how to apply parameters to an nf-core pipeline to cusomise its execution
* Understand the fastq file format 
* Learn how to interpret a FastQC report for RNAseq data 
* Learn about the benefits and drawbacks of read trimming for RNAseq-DE

## **Continue run command**

Learning objectives: 
* Understand how to apply parameters to an nf-core pipeline to cusomise its execution

## **Explore outputs**

Learning objectives: 
* Understand the process of mapping sequencing reads to a reference genome in a splice-aware manner 
* Understand how mapped reads are used to quantify gene counts 
* Learn how to read a MultiQC report 

## **Reports**

Learning objectives: 
* Learn how to interpret Nextflow workflow execution reports 
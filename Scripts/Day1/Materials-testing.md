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

* Case study [paper](https://bmcgenomics.biomedcentral.com/articles/10.1186/s12864-016-2801-4)

We did not include case study section in 2022 day 1 materials:  
* Explain traits associated with WBS
* Explain KO mouse model 
* Explain research questions 

Poll questions 
* Which sorts of genes do you expect will be up/downregulated in KO compared with WT?
* How might gene expression correlate with known features of WBS? 
* Can you identify a potential limiting factor in the experimental design which might affect result interpretation?

## **Intro to the workflow**

## **QC run command** 

## **Continue run command**

## **Explore outputs**

## **Reports**


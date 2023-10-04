# RNAseq workshop 2023 testing

A short description of what this repository is for, which project it is linked to. 

* [How to use this repository](#how-to-use-this-repository)
* [Environment set up](#set-up-environment)
    + [Compute environment](#compute-environment)
    + [Test data](#test-data)
* [External resources](#external-resources)

## How to use this repository 

Provide instructions on how to use the repository for your project. For example: 

* Place all code/notebooks/scripts in the `Scripts` directory
* If needing to break up content, delete `Scripts` directory and create a subdirectory for each objective or workshop session
* Use [`.gitignore`](https://www.atlassian.com/git/tutorials/saving-changes/gitignore) to manage files and directories that should **not** be committed
* Each subdirectory should contain a descriptive README.md 
* Include any useful files and scripts in the relevant subdirectories
* Explanation of test dataset(s) if required 

## Environment set up 

We are using Nimbus bioimage for the workshop. Using the following tools and versions:
* nf-core/2.9 
* singularity/3.8.7
* nextflow/23.04.2.5870

### Compute environment 

All testing for this workshop is being performed on Pawsey's Nimbus cloud. VMs are 2c.8r with 40Gb of disk. To create an instance:

* Open Nimbus dashboard 
* Navigate to rnaseq-workshop project in the top menu drop down 
* Add the 8787 network port for Rstudio (this was done for whole project)
    * In side menu go to Network > Security Group
    * Follow instructions [here](https://support.pawsey.org.au/documentation/display/US/Run+RStudio+Interactively#)
* Create an instance 
    * In side menu go to Compute > Instances > Launch Instance
    * Provide a name for the instance 
    * Source: Pawsey Bio - Ubuntu 22.04 - 2023-06
    * Flavour: n3.2c8r
    * Networks: Public external
    * Security Groups: port 8787, rnaseq-workshop-SSH-ICMP
    * Keypair: either specify existing or make a new one and save to your local machine
    * Launch instance

### Test data 

Testing with 2022 rnaseq workshop material on CVMFS. To access from the Nimbus CLI: 

```bash
ls /cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/IntroRNAseq_0922/
ls /cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/UnlockNfcore_0523/
```

* You will need to cache with (ls) before trying to use the files 
* You cannot cd into CVMFS, it is read-only

## External resources 

* [nf-core/rnaseq](https://github.com/nf-core/rnaseq/tree/3.12.0)
* [nf-core/tools](https://nf-co.re/tools)

# Testing download and runtime of nf-core/rnaseq 

## Use of nf-core tools for downloading code base and containers 

1. [Install nf-core tools](https://nf-co.re/tools#installation) with pip (not conda)
```bash
sudo pip install nf-core
```

Test download by printing help message 
```bash
nf-core -h
```

2. [Download pipeline code base with containers](https://nf-co.re/tools#downloading-pipelines-for-offline-use)

```bash
time nf-core download 
```
Used prompts to download:
* rnaseq 
* version 3.12.0
* use singularity containers 
* container library quay.io
* institutional configs true
* output directory: nf-core-rnaseq_3.12.0

**Time taken**
* real    31m34.944s
* user    2m10.827s
* sys     0m21.961s

What is the command for this without prompts? 

```bash
nf-core download nf-core/rnaseq \
    --revision 3.12.0 \
    --container singularity \
    --compress none
```

3. [Download pipeline code base without containers](https://nf-co.re/tools#downloading-pipelines-for-offline-use)

```bash
NXF_SINGULARITY_CACHEDIR=/home/ubuntu/rnaseqOct23_Testing/singularity-images

echo $NXF_SINGULARITY_CACHEDIR

nf-core download nf-core/rnaseq \
    --revision 3.12.0 \
    --container singularity \
    --compress none \
    --singularity-cache-only
```

* Had to copy containers to workflow directory, need to check if this can be skipped or avoided 
* This makes a copy of the singularity images, rather than symlink???? 

**Time taken**
* real    1m45.437s
* user    0m6.106s
* sys     0m3.185s

Using a specified singularity cache (`$NXF_SINGULARITY_CACHEDIR`) available on all cloned VMs, we can reduce download time. 

## nf-core workflow execution testing 

Environmental variables will need to be reset if VMs are disconnected.

```bash
echo $NXF_SINGULARITY_CACHEDIR
```

If no value printed to screen, then: 
```bash
NSXF_SINGULARITY_CACHEDIR=/home/ubuntu/rnaseqOct23_Testing/singularity-images
```

Test running the help command:
```bash
nextflow run nf-core-rnaseq_3.12.0/3_12_0/main.nf --help
```

* Will we need to explain the code base we downloaded? (Might be useful to provide some context similar to the [custom nf-core workshop](https://sydney-informatics-hub.github.io/customising-nfcore-workshop/notebooks/2.1_design.html#download-the-pipeline-code)). 
* Downloading the code base is responsible practice- should have a record of your methods and these pipelines change frequently. Also good for troubleshooting errors, learning. 

### Check CVMFS dataset is suitable 

Cache CVMFS
```bash
ls /cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/UnlockNfcore_0523
```

How many samples in the samplesheet? (all 6)
```bash
cat /cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/UnlockNfcore_0523/samplesheet.csv 
```

In addition to gtf, fasta, indexes for STAR and salmon, will need to check if they are compatible with v3.12.0. 
```bash
ls /cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/UnlockNfcore_0523/mm10_reference/
```

### Test run command with 6 samples 

Store CVMFS path in materials variable: 
```bash
materials=/cvmfs/data.biocommons.aarnet.edu.au/training_materials/SIH_training/UnlockNfcore_0523
```
The run workflow: 
```bash
nextflow run nf-core-rnaseq_3.12.0/3_12_0/main.nf \
    --input ${materials}/samplesheet.csv \
    --outdir WBS-mouse \
    --fasta $materials/mm10_reference/mm10_chr18.fa \
    --gtf $materials/mm10_reference/mm10_chr18.gtf \
    --star_index $materials/mm10_reference/STAR \
    --salmon_index $materials/mm10_reference/salmon-index \
    -profile singularity \
    --skip_markduplicates \
    --save_trimmed true \
    --save_unaligned true \
    --max_memory '6.GB' \
    --max_cpus 2
```

#### Complete run (no download)

First run, workflow was slow to run and failed on Salmon process (exit status 255). Got error previously observed by others. Report to Nimbus support team.  

```default
FATAL:   container creation failed: mount /proc/self/fd/3->/usr/local/var/singularity/mnt/session/rootfs error: while mounting image /proc/self/fd/3: failed to find loop device: could not attach image file to loop device: failed to set loop flags on loop device: resource temporarily unavailable.
```

Successful run printed strandedness check- this is too advanced for an exercise. Strandedness is incorrectly specified in the samplesheet.csv. Suggest changing this so specified strandedness matches check. 

**Time taken**
* real    18m58.694s
* user    10m19.871s
* sys     1m44.330s  

* Should we have backup data available on CVMFS for anyone who's pipeline doesn't run successfully? 
* Directories should be structured with run scripts provided, but encourage them to code along. 
* Can we symlink cvmfs to working directory? 
* Change strandedness specification in samplesheet.csv

#### Run FastQC and trimming only

nf-core/rnaseq provides option to run FastQC and read trimming only. Use the `--skip_alignment` flag. 

```bash
nextflow run nf-core-rnaseq_3.12.0/3_12_0/main.nf \
    --input ${materials}/samplesheet.csv \
    --outdir WBS-mouse-QC \
    --fasta $materials/mm10_reference/mm10_chr18.fa \
    --gtf $materials/mm10_reference/mm10_chr18.gtf \
    --star_index $materials/mm10_reference/STAR \
    --salmon_index $materials/mm10_reference/salmon-index \
    -profile singularity \
    --skip_alignment \
    --max_memory '6.GB' \
    --max_cpus 2
```

**Time taken**
* real    2m45.760s
* user    2m0.555s
* sys     0m21.391s

#### Run skip FastQC 

nf-core/rnaseq provides option to skip FastQC. Use the `--skip_fastqc` flag. 

```bash
nextflow run nf-core-rnaseq_3.12.0/3_12_0/main.nf \
    --input ${materials}/samplesheet.csv \
    --outdir WBS-mouse \
    --fasta $materials/mm10_reference/mm10_chr18.fa \
    --gtf $materials/mm10_reference/mm10_chr18.gtf \
    --star_index $materials/mm10_reference/STAR \
    --salmon_index $materials/mm10_reference/salmon-index \
    -profile singularity \
    --skip_fastqc \
    --skip_markduplicates \
    --max_memory '6.GB' \
    --max_cpus 2 
```

**Time taken**
* real    17m7.048s
* user    9m47.091s
* sys     1m37.860s

#### Run full workflow following QC with resume

nf-core/rnaseq provides option to skip FastQC. Use the `--skip_fastqc` flag and `-resume` flag to not rerun read trimming. 

```bash
nextflow run nf-core-rnaseq_3.12.0/3_12_0/main.nf \
    --input ${materials}/samplesheet.csv \
    --outdir WBS-mouse \
    --fasta $materials/mm10_reference/mm10_chr18.fa \
    --gtf $materials/mm10_reference/mm10_chr18.gtf \
    --star_index $materials/mm10_reference/STAR \
    --salmon_index $materials/mm10_reference/salmon-index \
    -profile singularity \
    --skip_fastqc \
    --skip_markduplicates \
    --max_memory '6.GB' \
    --max_cpus 2 -resume
```

**Time taken**
* real    14m48.686s
* user    9m1.275s
* sys     1m28.457s
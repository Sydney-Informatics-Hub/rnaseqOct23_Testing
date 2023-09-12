# Testing runtime of nf-core/rnaseq 

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

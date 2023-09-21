## Tests for creating and excecuting a RStudio singularity container on Nimbus instance

- **Base link**: [Run RStudio Interactively](https://support.pawsey.org.au/documentation/display/US/Run+RStudio+Interactively)
- Specific steps pertaining to this workshop are as documented below 

### Instance
Pawsey Nimbus bioinstance (n3.2c8r): ubuntu@http://146.118.70.149

### Build a Docker container image
- A user on Nimbus instance does not have root privileges. Hence they cannot deploy a docker container directly. Instead they need to create a docker image and then convert the docker image into a singularity image to deploy it.  
- The command to build a docker image is as follows:  
`sudo docker build -f BioCommons_Dockerfile_4.1.0_V4.0 -t rstudio:4.1.0.wCP .` The above command requires a RStudio `Dockerfile`. Here the name of our Dockerfile is `BioCommons_Dockerfile_4.1.0_V4.0` and the name of the Docker container image is `rstudio:4.1.0.wCP`.  
- Additional details about contents of a RStudio Dockerfile for bioinformatics can be found [here](https://support.pawsey.org.au/documentation/pages/viewpage.action?pageId=59476382#RunRStudioInteractively-2.2.1.BuildRStudiocontainer(R=4.1.0))  
-  Click on the Dockerfile name [BioCommons_Dockerfile_4.1.0_V4.0](BioCommons_Dockerfile_4.1.0_V4.0) to view contents of the file used to create the Docker image for this workshop.  
- Docker images are not “visible”. We can  see the docker images using the command:  `sudo docker images`.
```
'REPOSITORY       TAG         IMAGE ID       CREATED          SIZE
rstudio          4.1.0.wCP   63a14b288cb5   28 minutes ago   4.82GB
rocker/rstudio   latest      96c6bd2c7b6c   2 weeks ago      1.87GB
```
### Update a Docker container image (over-write)
- If you need to modify/update an existing docker image, you can use the same name for a Docker image without deleting the existing one. Docker will take advantage of caching to build the new image efficiently.  
- When you build an image with the same name as an existing image, Docker will only update the layers that have changed in your Dockerfile, and reuse the layers that haven't changed.  
- This helps save disk space because it doesn't create entirely new images.


### Pull and convert the Docker image into a Singularity container
- The next step is to convert the docker container image into a singulairty container image.  
- The required command is:  
`sudo singularity pull docker-daemon:rstudio:4.1.0.wCP`  
- The output of the above command is a singularity container image file called `rstudio:4.1.0.wCP.sif`

### To start the RServer, run:
- On the new training-sized (2c8r) new VM : 146.118.70.149:8787

### Run the singularity container to start the Rstudio Server
- **Step I**  
`mkdir -p /home/ubuntu/rnaseq23/day2_RStudio/working_dir/rstudio-server`
 
- **Step II**   
``` 
PASSWORD='abc' singularity exec \
    -B /home/ubuntu/rnaseq23/day2_RStudio/working_dir/rstudio-server:/var/lib/rstudio-server \
    -B /home/ubuntu/rnaseq23/day2_RStudio/working_dir/rstudio-server:/var/run/rstudio-server \
    -B /home/ubuntu/rnaseq23/day2_RStudio/working_dir:/home \
    rstudio_4.1.0.wCP.sif \
    rserver --auth-none=0 --auth-pam-helper-path=pam-helper --server-user ubuntu
```

**Note**
singularity exec invokes the container to run the rserver command.  
- `-B` is a flag used to for bind-mounting.  
- `/home/ubuntu/base_directory/working_directory` is a directory on your instance.   
- `/home` is the container's directory `/home/ubuntu/base_directory/working_directory` is bind-mounted to.  
- Do not replace `/home`. You can replace `/home/ubuntu/base_directory/working_directory`  with a directory of your choice, as long as it has user ownership.  
- A folder called `rstudio` is automatically created inside the folder `/home/ubuntu/rnaseq23/day2_RStudio/working_dir`
    - All input files to be viewed from RStudio need to be placed in this folder `rstudio`.  
    - All outputs from your R session are saved to the `rstudio` folder.


### Open RStudio from a browser
- Open up a browser window 
- Go to your instance IP address with the 8787 port, e.g. 146.118.XX.XX:8787
- Enter the username you created, e.g. rstudio (or if using a container, ubuntu)
- Enter the password you gave it e.g `abc` as seen in the above command
- Run your R commands as you normally would


### End your RStudio session and server
If you ran RStudio from a container, stop the running command with ctrl+c:

Or kill the process on the port:
`lsof -ti:8787 | xargs kill -9`  
`lsof -ti:8787`



### Other useful commands
1) List all Docker containers (including stopped ones): `docker ps -a`  
2) Remove containers you no longer need: `docker container rm CONTAINER_ID`  
3) Clean the Singularity cache: `sudo singularity cache clean`  
The above command
    - Is used to clean the Singularity cache.  
    - Singularity caches container images to speed up subsequent container operations by storing images locally.  
    - It will remove all cached container images from the cache directory.  
**Note**: Running this command will delete all cached images, and you may need to re-download or rebuild the containers if they are required again in the future.

### Summary
 - RStudio singularity container was updated.     
 - RServer can be run on the Nimbus instance using the RStudio singularity container.     
 - The Rmarkdown was updated and excecuted.  
 - The `knit` option was used from RStudio to create a output `html` file.  
 - One known issue as noted below.


### Intermittent problems when running the Rserver
- This error was observed only a few times during testing - Running thr RSever command.  
- ERROR: `“FATAL:   container creation failed: mount /proc/self/fd/3->/usr/local/var/singularity/mnt/session/rootfs error: while mounting image /proc/self/fd/3: failed to find loop device: could not attach image file to loop device: no loop devices available”`    

**Current workaround**  
- Check if the loop module is `loaded: lsmod | grep loop`  
- If the loop module is not listed, you can load it using the following command: `sudo modprobe loop`  
- Verify the loop devices availability: `sudo losetup -f`  
- I remember encountering a similar error when I had tried running a nfcore workflow on another Nimbus instance.  
- I had previously asked this same question to Pawsey helpdesk and they had provided the following solution
 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Pawsey helpdesk commented:

```
Hi Nandan

This seems to do with some issues that Apptainer/Singularity has identified, and there's a workaround for it. Please see below:

---- 

https://github.com/apptainer/apptainer/issues/1258

On this kernel, you will need to configure max_loop for the loop driver to a higher value. Suggest setting it to the same as max loop devices in /usr/local/etc/singularity/singularity.conf, which is 256 by default.

You must do this with a kernel CMDLINE entry, because loop is compiled into the kernel on Ubuntu. It is not a module.

Add max_loop=256 to the GRUB_CMDLINE_LINUX value in /etc/default/grub
Run update-grub2 as root.
Reboot the system.
cat /proc/cmdline and verify max_loop=256 is present.
—

Let us know how you go or if you need any help.

Kind regards

Pawsey helpdesk
```

Since the problem does not occur frequently (I have observed it only once on any of the instances I have worked so far). One of our collegues Giorgia has also encountered it on her insatnce. I have not tried the workaround as suggested by Pawsey helpdesk, yet.  
Should we check with Pawsey helpdesk if this could be a possible issue on the trainee instances for the workshop and if we need to have any quick workarounds if this occures? 





 





 





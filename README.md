# Create singularity images

Why would I need a singularity image?

* Users of an HPC cluster might not have the required permission rights to install certain softwares.
* A singularity image might also represent an alternative if softwares cannot be installed via conda/mamba environments.

## Installation on your local device

A [quick installation guide](https://docs.sylabs.io/guides/latest/user-guide/quick_start.html#quick-installation-steps) on how to install singularity.


## Generate singularity images using a build file (recommended)


```
sudo singularity build image.sif buildfile.def

```

## Generate singularity images manually (NOT recommended) 

```
sudo singularity build --sandbox mito.sif docker://ubuntu:latest
sudo singularity shell --writable mito.sif

apt-get update
apt-get -y upgrade
apt-get -y install vim less gcc make
apt-get -y install git
apt install build-essential

g++ --version

git clone https://github.com/cmayer/MitoGeneExtractor.git
cd MitoGeneExtractor/
make
```

## Check which software and subprogramms are installed within a singularity image

```
# access the image with
singularity shell image.sif

# check installations
# NOTE: this only works when you are within the image!!!
 ls /opt
```

## Use the image on your local device

```
singularity exec image.sif softwarename
```

# Use image on your HPC system

```
# copy image to HPC system
rsync -avP image.sif leo5:/scratch/cxxxxxxx/bio/

# load singularity on your system
module load singularity/3.8.7-python-3.10.8-gcc-8.5.0-e6f6onc
singularity exec /scratch/cxxxxxxx/bio/image.sif softwarename
```



### Examples for building and using images 

#### Build image for the usage of PAUP*

```
# build image
sudo singularity build paup/paup.sif paup/paup.def

# execute paup
singularity exec paup/paup.sif /opt/paup4a169_ubuntu64


# debugging the singularity container if missing library error occurs to find out which libs are missing
singularity exec paup/paup.sif ldd /opt/paup4a169_ubuntu64

```


#### Build image for the usage of MitoGeneExtractor and Exonerate

```
# build image
sudo singularity build mito/mito.sif  mito/mito.build

# execute paup
singularity exec mito/mito.sif MitoGeneExtractor
```









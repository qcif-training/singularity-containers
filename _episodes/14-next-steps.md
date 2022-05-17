---
title: "Next Steps"
teaching: 30
exercises: 
questions:
objectives:
- Use containers in HPC Jobs
- Use containers in Pipelines
- Use containers on your Desktop
- Building containers
keypoints:
- Using containers in a HPC job is just like an Interactive job
- Using containers in pipelines assists with portability and version control (Using tags etc)
- Containers can be used on your desktop or laptop
- You can build containers if you cannot find one using the build tools in Docker and Singularity
---

## Using Containers in HPC Jobs
So far we have using containers in an interactive job on the HPC. What if we want to use them in a batch job?

```
#!/bin/bash
#PBS -N Blast01
#PBS -l walltime=4:00:00
#PBS -l select=1:ncpus=2:mem=6gb
#PBS -j oe
#PBS -m abe

cd $PBS_O_WORKDIR
#module load singularity/...

singularity exec ../blast/blast_2.9.0--pl526he19e7b1_7.sif makeblastdb -in zebrafish.1.protein.faa -dbtype prot

singularity exec -B $TUTO/demos/blast_db blast_2.9.0--pl526he19e7b1_7.sif blastp -query P04156.fasta -db $TUTO/demos/blast_db/zebrafish.1.protein.faa -out results.txt
```
{: .bash}

The above example shows running singularity as part of a job is just like using an intereactive job. If you need to load a module for singularity, include the command after the #PBS lines.

## Using Containers in Pipelines
Many pipeline engines such as Nextflow and Snakemake can make use of containers using Docker, Singularity or other engines.

For example, to run the NF-Core RNASeq Nextflow pipeline with singularity:
```
nextflow run rn-core/rnaseq -profile test,singularity
```
{: .bash}

This will run the pipeline with test data and a container in singularity.

If a container is specified in a Snakemake pipeline, it can be activated by adding the --use-singularity option:
```
snakemake --use-singularity
```
{: .bash}

Using containers is generally much quicker that building conda environments.

## Use Containers on your Desktop
Singularity is a native Linux application so if you using Windows or Mac, you will need WSL (Windows) or a virtual machine. An alternative is to use Docker Desktop.
Using docker is similar to Singularity.
Optionally pull the container:
```
docker pull quay.io/biocontainers/blast:2.9.0--pl526he19e7b1_7
```
{: .bash}

Run the container:
```
docker exec -v $TUTO/demos/blast_db:/blast_db quay.io/biocontainers/blast:2.9.0--pl526he19e7b1_7 blastp -query P04156.fasta -db /blast_db/zebrafish.1.protein.faa -out results.txt
```
{: .bash}

## Building Containers
So far we have run existing containers published on various repositories. What if you need to run a tool (or a combination of tools) and you cannot find the container? Well, you can build on of course!

A Docker or Singularity container is built from a recipe. Docker expects the recipe file to be called Dockerfile
For example, the Pawsey Lolcow Dockerfile:
```
FROM ubuntu:18.04

LABEL maintainer="Pawsey Supercomputing Centre"
LABEL version="v0.0.1"

RUN apt-get -y update && \
  apt-get -y install fortune cowsay lolcat

ENV PATH=/usr/games:$PATH

CMD fortune | cowsay | lolcat
```
{: .source}
To build a docker image from this Dockerfile:
```
docker build -t lolcow:latest .
```
{: .bash}
Once built in Docker, it can be pushed to a repository, or converted to Singularity etc.

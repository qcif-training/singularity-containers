---
title: "Introduction to containers"
teaching: 20
exercises: 5
questions:
objectives:
- Define the term "container"
- Discuss when you would benefit from using containers in your workflow
keypoints:
- Containers enable you to package up an application and its dependencies.
- By using containers, you can better enforce reproducibility, portability and share-ability of your computational workflows.
---

### Before We Start
Lets launch an Interactive job on your HPC.

First, connect a terminal to your HPC
Eg, QUT:

```
$ #QUT
$ ssh {username}@lyra.qut.edu.au
```
{: .bash}

Then launch the job:
 
```
$ #QUT
$ qsub -I -S /bin/bash -l walltime=4:00:00 -l select=1:ncpus=2:mem=6gb
$ #UQ - Tinaroo users, please ensure you know your group.
$ qsub -I -A {GROUP} -l walltime=4:00:00 -l select=1:ncpus=2:mem=6gb
```
{: .bash}

JCU People will use 'qris-jcu' as the GROUP name.

#### CQU People use Strudel
CQU People, please use Strudel to connect to the HPC then open a terminal.

See: [Graphical Connection to the HPC System](https://www.cqu.edu.au/eresearch/high-performance-computing/hpc-user-guides-and-faqs/graphical-connection-to-the-hpc-system)

This may take some time to run so we will come back to this later.

### Apps vs Containers

To use software on a computer traditionally you were required to install the software. This means placing files on the filesystem, libraries in folders, configuration files in the correct place etc. Applications that have a setup program help by placing the files in the right place. This can lead to problems with conflicting files. What if two packages need to install the Widget library but App A needs Widget version 1 and App B needs Widget version 2? On a HPC, managing the installed software for hundreds or thousands of users becomes a real challenge.

When people use packages like Python or R, they typically need a great deal of libraries to also be installed so their scripts can run.

A container is an entity providing an isolated software environment (or filesystem) for an application and its dependencies.  

<img src="{{ page.root }}/fig/container_diagram.png" alt="Apps vs Containers" width="708" height="331"/>

The key difference here is a container will have all the libraries and dependent apps needed to run the package. It does this by bundling the App, libraries and operation system into a single image.

The benefits are:

* Portable - the image does not need to be installed, it can "run" straight away

* Self-contained - the image contains the whole stack needed to run the application

* Isolated - as the image is self contained, it will not conflict with software in other containers or the host operating system

* Modular - as each container has its defined apps, you can run different containers after each other in a pipeline


### Containers and your workflow

There are a number of reasons for using containers in your daily work:

* Data reproducibility/provenance
* Cross-system portability
* Simplified collaboration
* Simplified software dependencies and management
* Consistent testing environment

A few examples of how containers are being used include:

* Bioinformatics workflows
* Machine Learning 
* Python apps in radio astronomy
* RStudio & Jupyter Notebook sessions
* OpenFoam simulations
* HPC workflows (via Singularity)

### Terminology

An **image** is a file (or set of files) that contains the application and all its dependencies, libraries, run-time systems, etc. required to run.  You can copy images around, upload them, download them etc.

A **container** is an instantiation of an image.  That is, the image in a running state.  You can run multiple containers from the same image, much like you might run the same application with different options or arguments.

In abstract, an image corresponds to a file, a container corresponds to a process.

A **registry** is a place where images are stored and can be accessed by users.  It can be public (*e.g.* *Docker Hub*) or private.

To build an image we need a recipe.  A recipe file is called a **Definition File**, or **def file**, in the *Singularity* jargon and a **Dockerfile** in the *Docker* world.


### Container engines

A number of tools are available to create, deploy and run containerised applications.  We will only be using Singularity in this lesson:

* **Docker**: the first engine to gain popularity, still widely used in the IT industry.  Not very suitable for HPC as it requires *root* privileges to run. Typically used on desktops

* **Singularity**: a simple, powerful container engine for the HPC world.  The main focus of this workshop.

* **Shifter/Sarus**: a Docker-compatible container engine, suitable for HPC.  Can run containers, cannot build them.

* **Charliecloud**: a Docker-compatible tool for lightweight, user-defined software stacks for high-performance computing.

* **Enroot**: Nvidia's take on containers, a simple, yet powerful tool to turn traditional container/OS images into unprivileged sandboxes.

* **Podman**: a *root*-less alternative to Docker.  Catching up quickly with HPC requirements.

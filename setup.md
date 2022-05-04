---
layout: page
title: Setup
root: .
---


### Key requirement

The main requirement for this workshop is a HPC account with your institution. Please ensure you have a HPC account. Check your institition's HPC connection instructions.

* QUT: Visit HEAT and search for HPC account.

* UQ:

* GU:

* CQU:

* USQ:

* USC:

* JCU:

### Zoom

To participate in a {% if site.carpentry == "swc" %} Software Carpentry {% elsif site.carpentry == "dc" %} Data Carpentry {% elsif site.carpentry == "lc" %} Library Carpentry {% endif %} workshop, you will need access to software as described below. In addition, you will need an up-to-date web browser.

We maintain a list of common issues that occur during installation as a reference for instructors that may be useful on the Configuration Problems and Solutions wiki page.

{% comment %} For online workshops, the section below provides:

installation instructions for the Zoom client
recommendations for setting up Learners' workspace so they can follow along the instructions and the videoconferencing
If you do not use Zoom for your online workshop, edit the file _includes/install_instructions/videoconferencing.html to include the relevant installation instrucctions. {% endcomment %} {% if online != "false" %} {% include install_instructions/videoconferencing.html %} {% endif %}

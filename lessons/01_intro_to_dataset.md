---
title: "Intro to dataset"
author: "Mary Piper, Meeta Mistry, Radhika Khetani, Jihe Liu"
date: "Friday, July 16, 2021"
---

Approximate time: 20 minutes

## Learning objectives
* 

# Introduction to the dataset

For this workshop we will be working with RNA-seq data from a recent publication in Neuron by Baizabal et al. (2018). We will learn about how to use the ggplot2 package and other related packages to recreate a figure from this publication. In the second half of this workshop we will learn how to modify the  figure to fit the submission criteria of a different journal. 

## The publication

The authors in this paper discover an epigenetic mechanism that controls the number and positioning of cortical neurons. They discover that the histone methyltransferase PRDM16 works with enhancer elements to either silence or activate expression of sets of genes that impact the organization of the cerebral cortex. 

<p align="center">
<img src="../img/graphical_abstract.png" width="500">
</p>

The authors use various techniques to identify and validate the targets and activities of PRDM16, including ChIP-seq, bulk RNA-seq, FACS, insitu hybridization and immunofluorescent microscopy on brain samples from embryonic mice, generation of conditional knockout mice, etc. Majority of the figures in this publication are a combination of the evidence gathered from several of these techniques.

## The figure

In this workshop, we will focus on recreating [Figure 4](https://els-jbs-prod-cdn.jbs.elsevierhealth.com/cms/attachment/728f6fe8-d0ac-4893-ac8d-535469a2a1d1/gr4.jpg). This figure demonstrates how PRDM16 impacts  on gene expression in three different cell populations as well as in the brains of mice where PRDM16 is knocked out.

<p align="center">
<img src="../img/figure_to_create.jpg" width="600">
</p>

<p align="center">
<img src="../img/figure4_legend.png" width="800">
</p>

The different types of plots here are 
* box plots to show expression of 4 genes in the three different cell populations isolated by FACS, and whose gene expression was assessed by bulk RNA-seq
* a heatmap with the up- and down- regulated genes identified when comparing the expression of genes in Radial Glia in WT or when PRDM16 is knocked out
* volcano plots (essentially a scatter plot)
* Venn diagrams
* Bar plots with functional analysis categories 
 
In addition to these plots, there is a schematic, the FACS output and an immunofluorescent image.

## Reading in the data

## Making figures for a publication: Art or Science?

## References:

1. [Baizabal et al., 2018, Neuron 98, 945–962 - *The Epigenetic State of PRDM16-Regulated Enhancers in Radial Glia Controls Cortical Neuron Position*](https://doi.org/10.1016/j.neuron.2018.04.033)

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

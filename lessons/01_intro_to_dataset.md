---
title: "Intro to dataset"
author: "Mary Piper, Meeta Mistry, Radhika Khetani, Jihe Liu"
date: "Friday, July 16, 2021"
---

Approximate time: 20 minutes

## Learning objectives
* 

# Introduction to the dataset

For this workshop we will be working with RNA-seq data from a recent publication in Neuron by Baizabal et al. (2018). We will learn about how to use the ggplot2 package and other related packages to recreate a figure from this publication. In addition, we will also learn how to modify the figure to fit the submission criteria of a different journal. 

## The publication

The authors in this paper discover an epigenetic mechanism that controls the number and positioning of cortical neurons. They discover that the histone methyltransferase PRDM16 works with enhancer elements to either silence or activate expression of sets of genes that impact the organization of the cerebral cortex. 

<p align="center">
<img src="../img/graphical_abstract.png" width="500">
</p>

The authors use various techniques to identify and validate the targets and activities of PRDM16, including ChIP-seq, bulk RNA-seq, FACS, insitu hybridization and immunofluorescent microscopy on brain samples from embryonic mice, generation of conditional knockout mice, etc. Majority of the figures in this publication are a combination of the evidence gathered from several of these techniques.

## The figure

In this workshop, we will focus on recreating [Figure 4](https://els-jbs-prod-cdn.jbs.elsevierhealth.com/cms/attachment/728f6fe8-d0ac-4893-ac8d-535469a2a1d1/gr4.jpg). This figure demonstrates how knocking out PRDM16 impacts gene expression in three different cell populations in the developing brains of mouse enbryos.

<p align="center">
<img src="../img/figure_to_create.jpg" width="600">
</p>

<p align="center">
<img src="../img/figure4_legend.png" width="800">
</p>

The different types of plots here are:
* **Box plots** to show expression of 4 genes in the three different cell populations isolated by FACS, and their gene expression was assessed by bulk RNA-seq. Each gene represents a cell populations sorted by FACS.
* **A Heatmap** with the significantly up- and down- regulated genes when comparing the WT *Radial Glia* (PAX6 + cells) versus the *Radial Glia* from the PRDM conditional knock out
* **A Volcano plots** (essentially a scatter plot) to demonstrate the impact of knocking out PRDM16 on the 3 different cell types
* **Venn diagrams** to demonstrate the overlap in the list of differentially expressed genes in 2 different cell populations (Radial Glia and Intermediate progenitors)
* **Bar plots** showing the number of genes associated with specific Gene Ontology categories. In each case the number of significantly up- or down- regulated genes are listed in the middle.
 
In addition to the plots listed above, there is (a) a very helpful **schematic of the experiment**, (b) the **FACS output** and (c) **an immunofluorescence image** to show the how well the cell populations were separated from each other.

## Reading in the data

In the first half of this workshop, we will be focusing on creating those plots that use the ggplot2 package. In the second half of this workshop, we will (1) use packages not associated with ggplot2 to create the heatmap and the Venn diagrams, (2) we will also add the schematics and create a figure with the same layout as in the Neuron paper, and finally, (3) we will show you how to change the layout for a different journal.

First though we need to bring the data into R!

1. We will start by downloading a basic project folder with the data by right-clicking on [this link](https://www.dropbox.com/s/ig52a8501ur2eka/publication_perfect.zip?dl=1). We recommend that you place this zipped folder on your Desktop for the duration of the workshop. 
1. Unzip the folder.
1. Run the following code to read in the data.

```r
# read in the metadata file
meta <- read.csv("data/pp_all_meta.csv", row.names=1)

# read in the normalized gene expression values
normalized_counts <- read.csv("data/pp_all_normalized_counts.csv", row.names=1)

# read in the results of the differential gene expression analysis
results <- read.csv("data/pp_all_results.csv", row.names=1)
```

### Downloaded data

The data we have downloaded and read into R above represents the following 3 files from the larger analysis described in the paper:

* **pp_all_meta.csv** - The metadata for the experiment. This experiment has 24 samples:
  * 4 replicates for each sample group
  * 6 sample groups - `"Pax6:WT", "Pax6:KO", "neg:WT", "neg:KO", "Tbr2:WT", "Tbr2:KO"`
  * The `Pax6` samples correspond to Radial Glia
  * The `Tbr2` samples correspond to Intermediate Progenitors
  * The `neg` (negative for Pax6 and Tbr2) samples correspond to Neurons

* **pp_all_normalized_counts.csv** - The normalized counts for all the samples
* **pp_all_results.csv**


## Making figures for a publication: Art or Science?



## References:

1. [Baizabal et al., 2018, Neuron 98, 945–962 - *The Epigenetic State of PRDM16-Regulated Enhancers in Radial Glia Controls Cortical Neuron Position*](https://doi.org/10.1016/j.neuron.2018.04.033)

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

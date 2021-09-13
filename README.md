# Publication Perfect

| Audience | Computational skills required | Duration |
:----------|:-------------|:----------|
| General - researchers | Intermediate R | 1-session in-person or online workshop (~ 6 hours of trainer-led time)|

## Description

One of the biggest challenges in disseminating your research is visualizing the results in a way that is meaningful, easy to interpret and aesthetically pleasing. Oftentimes, the extensive time dedicated to generating experimental results can rival the creation and optimization of their figures. With a point and click environment, you can spend hours or even days tweaking the settings to get the perfect figure - only to realize that you now have to repeat this process for the remaining data. This process can be especially challenging when needing to perform customizations or when pivoting your figures to adhere to guidelines from conferences, journals or other publishing platforms.

In this tutorial, we introduce an efficient and reproducible workflow in R for creating publication-ready figures. We will introduce ggplot2 syntax to create custom plots, and we will explore how to determine the type of plots most appropriate for your data. We will explore how to ensure consistency between figures using custom theme and color selections, with an emphasis on colorblind-friendly palettes from the RColorBrewer and viridis packages. We will also examine methods for enhancing our plots with functions from the ggpubr and cowplot packages, especially regarding layout and labeling of figures. Finally, we will conclude with an activity to use what we have learned to reproduce a published figure.

This is a hands-on tutorial in which the data and code will be distributed to participants who wish to follow along. All tutorial lessons and materials will be hosted on GitHub pages. Participants will be required to have R and RStudio downloaded and installed on their personal computers, in addition to any required R packages. This tutorial assumes an intermediate level of R knowledge.

## Expected Goals

* Learn how to determine the type of plots that are best for your data
* Appreciate the power and flexibility of ggplot2 to create custom plots
* Know how to use custom functions and palettes to create figures with consistent themes, styles and colors
* Understand how to use R packages, such as cowplot and ggpubr, to easily add layouts and labels often required in published figures 
* Know how to save plots in a variety of formats

## Learning Objectives

* Determine the plot types best for visualizing a given dataset
* Define the syntax for creating a plot using ggplot2
* Generate plots for various data types using ggplot2
* Explain how to create multiple plots using the same themes, styles, and colors
* Discuss how to quickly alter figures to meet a different set of requirements (different journal or conference)

## Target Audience

Researchers with an intermediate background in R who interested in using R to create publication-ready figures. 

## Schedule

| Time | Topic | Instructor
:-----------------------|:-------------|:-------------|
| 09:00 - 09:10	| [Introduce the instructors and scope of the workshop](https://github.com/hbctraining/publication_perfect/raw/main/BC2_Intro_to_workshop.pdf) | Mary | 
| 09:10 - 09:30	| [Introduction to data visualization and dataset](lessons/01_Introduction.md) | Mary | 
| 09:30 - 10:15 | [Basic ggplot2 syntax and creating a basic plot](lessons/02_ggplot2_syntax.md) | Mary | 
| 10:15 - 10:45	| Coffee break | |
| 10:45 - 11:05	| [Consistent plots with custom themes](lessons/03_custom_themes.md) | Radhika | 
| 11:05 - 11:45 | [Application of plotting basics](lessons/04_boxplot_application_of_basic_plotting.md) | Meeta | 
| 11:45 - 12:15	| [Customization of scales and color palettes](lessons/05_custom_plot_scales_colors.md) | Mary | 
| 12:15 - 13:30	| Lunch | 
| 13:30 - 14:00	| [Customization of scales and color palettes](lessons/05_custom_plot_scales_colors.md) | Mary |
| 14:00 - 14:35	| [Aligning and labeling plots with cowplot](lessons/06_aligning_plots_using_cowplot.md) | Radhika |
| 14:35 - 14:40	| Break
| 14:40 - 15:15	| [Adding annotations and including statistical comparisons with ggpubr](lessons/07_adding_text_annotations.md) | Jihe |
| 15:15 - 15:45	| [Incorporating external packages to extend plotting functionality](lessons/08_figure_specific_packages.md) | Will/Meeta |
| 15:45 - 15:50	| [Creating final figure](lessons/09_final_figure.md) | Mary |
| 15:50 - 15:55	| [Pivoting publications](lessons/10_pivoting_publications.md) | Mary |
| 15:55 - 16:00	| [Wrap-up and exit survey]() | Mary |

[Take-home exercise to create the bar plot figures](lessons/bar_plots_exercise.md)

## Instructors

* [Mary Piper, PhD](https://bioinformatics.sph.harvard.edu/people/mary-piper)
* [Meeta Mistry, PhD](https://bioinformatics.sph.harvard.edu/people/meeta-mistry)
* [Jihe Liu, PhD](https://bioinformatics.sph.harvard.edu/people/jihe-liu)
* [Will Gammerdinger, PhD](https://bioinformatics.sph.harvard.edu/people/will-gammerdinger)
* [Radhika Khetani, PhD](https://bioinformatics.sph.harvard.edu/people/radhika-khetani)

## Resources

* [ggplot2 free online book](https://ggplot2-book.org/index.html)
* [From Data to Viz online resource](https://www.data-to-viz.com)
* [Tidyverse ggplot2 online reference](https://ggplot2.tidyverse.org/reference/index.html)

### Dataset

Download the R project and data for this workshop [here](https://github.com/hbctraining/Training-modules/raw/master/data/publication_perfect.zip). Decompress and move the folder to the location on your computer where you would like to perform the analysis.

### Installation Requirements

Download the most recent versions of R and RStudio for your laptop:

 - [R](http://lib.stat.cmu.edu/R/CRAN/) (Version 4.0 or higher)
 - [RStudio](https://www.rstudio.com/products/rstudio/download/#download)
 
Install the required R packages by running the following code in RStudio:

```r
# Install CRAN packages
install.packages("tidyverse")
install.packages("cowplot")
install.packages("ggpubr")
install.packages("RColorBrewer")
install.packages("viridis")
install.packages("scales")
install.packages("VennDiagram")
install.packages("pheatmap")
install.packages("png")
install.packages("ggrepel")
install.packages("ggplotify")
install.packages("magick")
```

Load the libraries to make sure the packages installed properly:

```r
# Load R libraries
library(cowplot)
library(ggpubr)
library(RColorBrewer)
library(viridis)
library(scales)
library(tidyverse)
library(VennDiagram)
library(pheatmap)
library(png)
library(ggrepel)
library(ggplotify)
library(magick)
```

*These materials have been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

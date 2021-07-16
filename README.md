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
* Understand how to use the R packages cowplot and ggpubr to easily add layouts and labels often required in published figures 
* Know how to save plots in a variety of formats

## Learning Objectives

* Determine the plot types best for visualizing a given dataset
* Define the syntax for creating a plot using ggplot2
* Generate plots for various data types using ggplot2
* Explain how to create multiple plots using the same themes, styles, and colors
* Discuss how to quickly alter figures to meet a different set of requirements (different journal or conference)

## Target Audience

Researchers interested in using R to create publication-ready figures. 

## Schedule

| Time | Topic | Instructor |
:-----------------------|:-------------|:----------|
| 9:00 - 9:10	| Introduce the instructors and scope of the workshop (lecture)| 
| 9:10 - 9:30	| Introduce the dataset (discussion) [Discuss how to determine appropriate plotting methods for your data & Describe the types of relevant plots you would like to include/create]
| 9:30 - 10:15 	| [Basic ggplot2 syntax and creating a basic plot](lessons/ggplot2_syntax.md)
| 10:15 - 10:45	| Coffee break
| 10:45 - 11:05	| [Consistent plots with custom themes](lessons/custom_themes.md)
| 11:05 - 11:45 | [Application of plotting basics]
| 11:45 - 12:15	| [Consistent plots with defined color palettes]
| 12:15 - 13:30	| Lunch
| 11:45 - 12:15	| [Consistent plots with defined color palettes]
| 11:45 - 12:15	| Introduce features of cowplot for aligning/facetting and labeling plots (live coding)
| 13:30 - 14:15	| Introduce features of ggpubr for adding statistical comparisons and ordering of plots (live coding)
| 15:10 - 15:50	| Practice by changing code to adhere to a journal’s figure requirements (live coding)
| 15:50 - 16:00	| Wrap-up and exit survey (lecture)


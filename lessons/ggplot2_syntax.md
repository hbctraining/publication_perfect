---
title: "Plotting and data visualization in R"
author: "Mary Piper, Meeta Mistry, Radhika Khetani"
date: "Friday, July 9th, 2021"
---

Approximate time: 60 minutes

## Learning Objectives 

* Explain the syntax to utilize the "ggplot2" package to visualize data.

# Graphical syntax of `ggplot2`

When working with data, it is helpful to display it graphically to gain more insight. This is especially important for large datasets, where trends or relationships can be easily obscured. In this lesson, we will be introducing the syntax for the popular Bioconductor package for visualization, [`ggplot2`](https://ggplot2.tidyverse.org/). The `ggplot2` package approaches visualization using principles from the [Grammar of Graphics](https://www.springer.com/gp/book/9780387245447), a pivotal book describing quantitative graphics. The developers of `ggplot2` describe their approach to the Grammer of Graphics in a free, [online book](https://ggplot2-book.org/index.html), which can be a helpful resource for understanding the theory and organization of the `ggplot2` package. 

We will focus the majority of our materials on creating plots with `ggplot2` due to it's versatility and ease of customization. Once you learn the `ggplot2` syntax, you will have laid the foundation for creating a plethora of graphics.

## Set up 

Let's load the library for `tidyverse`, which is a suite of packages that include `ggplot2` for visualization, as well as some useful packages for wrangling (`dplyr`), parsing (`stringr`) and tidying (`tidyr`) data, among others.

```r
# Load the Tidyverse suite of packages
library(tidyverse)
```

> _**NOTE:** Don't be alarmed by any conflict statements when loading this library, these statements are just informing you of packages that are currently loaded that have functions of the same name._

The `ggplot2` syntax takes some getting used to, but once you become comfortable with it, you will find it's extremely powerful and flexible. To start learning about `ggplot2` syntax we are going to re-create our first figure from the [The Epigenetic State of PRDM16-regulated Enhancers in Radial Glia Controls Cortical Neuron Position paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6667181/). We will build the figure below using the layering approach employed by  `ggplot2`. We will highlight the purpose and utility of each layer we add, while highlighting it's flexibility and customization.

<p align="center">
<img src="../img/volcano_plot_glia.png" width="500">
</p>


Please note that `ggplot2` expects as input either a "data frame" or Tidyverse's version of a data frame called a "tibble" (you can find out more about tibbles [here](https://hbctraining.github.io/Training-modules/Tidyverse_ggplot2/lessons/intro_tidyverse.html)). To create this figure we will need the data present in our `results` data frame. This plot examines the magnitude (log2 fold changes) and significance (p-adjusted values) of the differences in gene expression between the *Prdm16* KO and WT samples for every gene in radial glia cells; each point is a gene. Let's take a look at the `results` data frame.

```r
# Inspect the results data frame
View(results)
```

We see that each gene is a different row and each column corresponds to different statistics regarding the differences in gene expression between the KO and WT samples within the radial glia (`pax6` columns), intermediate progenitors (`tbr2` columns) and neurons (`neg` columns). For this plot, we are interested in the radial glia, which correspond to the `pax6` columns. Specifically, we are interested in plotting the `Pax6` log2 fold changes (`pax_log2FoldChanges`) on the x-axis and the -log10 `Pax6` p-adjusted values (`pax6_padj`) on the y-axis.

## Creating a basic plot

The `ggplot()` function is used to **initialize the basic graph structure**, then we add to it. The basic idea is that you specify different parts of the plot using additional functions one after the other and combine them into a "code chunk" using the `+` operator; the functions in the resulting code chunk are called layers.

Let's start: 

```r
# Initialize plot
ggplot(results) # what happens? 
```

You get an blank plot, because you need to **specify additional layers** using the `+` operator. The `ggplot` function does not know what type of plot you want to draw or the columns in your data frame to compare visually. At the very least, we need to provide this information.

The **geom (geometric) object** is the layer that specifies what kind of plot we want to draw. A plot **must have at least one `geom`**; there is no upper limit. Examples include:

* points (`geom_point`, `geom_jitter` for scatter plots, dot plots, etc)
* lines (`geom_line`, for time series, trend lines, etc)
* boxplot (`geom_boxplot`, for, well, boxplots!)
* [many others](https://ggplot2.tidyverse.org/reference/#section-geoms)

The [RStudio cheatsheet for ggplot2](https://github.com/rstudio/cheatsheets/blob/master/data-visualization-2.1.pdf) is a bit overwhelming at first, but it can help with choosing the best 'geom' for our data. With two continuous variables with points, we will choose to use `geom_point()`. Let's add a "geom" layer to our plot using the `+` operator.

```r
# Initializing plot
ggplot(results) +
  geom_point() # note what happens here
```

Why do we get an error? Is the error message easy to decipher?

We get an error because each type of `geom` usually has a **required set of aesthetics** to be set. "Aesthetics" are set with the `aes()` function and can be set either nested within `geom_point()` (applies only to that layer) or within `ggplot()` (applies to the whole plot).

The `aes()` function has many different arguments, and all of those arguments take columns from the original data frame as input. It can be used to specify many plot elements including the following:

* position (i.e., on the x and y axes)
* color ("outside" color)
* fill ("inside" color) 
* shape (of points)
* linetype
* size
* alpha (level of transparency)

To start, we will specify x- and y-axis since `geom_point` requires the most basic information about a scatterplot, i.e. what you want to plot on the x and y axes. All of the other plot elements mentioned above are optional.

```r
# Adding geom layer with required aesthetics mapping to columns in data frame
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj)))
```

<p align="center">
<img src="../img/ggscatter-1.png" width="500">
</p>

## Customizing the appearance of the data points on the plot

Now that we have the required aesthetics, let's add some extras like color to the plot. We can **`color` the points on the plot based on the `pax6_threshold` column** within `aes()`, this column corresponds to the significant genes. You will notice that there are a default set of colors that will be used so we do not have to specify. We will explore later how to change the colors, as well as, incorporate pre-designed color palettes. Also, note that the legend has been plotted for us.

```r
# Adding geom layer with additional aesthetics
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold))
```

<p align="center">
<img src="../img/ggscatter-2.png" width="500">
</p>


If we wanted to modify the **size of the data points** we can use the `size` argument. 

* If we add `size` inside `aes()` we could assign a numeric column to it and the size of the data points would change according to that column. 
* However, if we add `size` inside the `geom_point()` but outside `aes()` we can't assign a column to it, instead we have to give it a numeric value. This use of `size` will uniformly change the size of all the data points.

> **Note:** This is true for several arguments, including `color`, `shape`, `alpha`, etc. E.g. we can change all shapes to square by adding this argument to be outside the `aes()` function; if we put the argument inside the `aes()` function we could change the shape according to a (categorical) variable in our data frame or tibble.

We have decided that we want to change the size of all the data point to a uniform size instead of typing it to a numeric column in the input tibble. Add in the `size` argument by specifying a number for the size of the data point:

```
# Changing size to a constant (do not change with columns in data frame)
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold),
             size = 2.0)
```

> **Note:** The size of the points is personal preference, and you may need to play around with the parameter to decide which size is best. That seems a bit too big, so we can return to our default size. 

## Exercise

Let's explore how to change the shape of the data points. Different shapes are available, as detailed in the [RStudio ggplot2 cheatsheet](https://github.com/rstudio/cheatsheets/blob/master/data-visualization-2.1.pdf). Let's explore this parameter by changing all of the points to squares.

```r
# Changing shape to square
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold),
             shape = "square")
```

<p align="center">
<img src="../img/dotplot4.png" width="700">
</p>

We aren't interested in keeping the squares, but it can be helpful to change shapes for visualizing different groups or conditions in the data, similar to the `color` argument. 

## Customizing the appearance of the non-data points on the plot

Now that we know how to alter and customize the look of the data points on our plot within the `geom` layer, let's explore how to change the look of the non-data plotting elements, such as the plotting grid and labels. Many of these non-data elements can be altered within a `theme()` layer. 

The ggplot2 `theme` system handles non-data plot elements such as:

* Axis label aesthetics
* Plot background
* Facet label backround
* Legend appearance

There are built-in themes we can use (i.e. `theme_bw()`) that mostly change the background/foreground colors, by adding it as additional layer. A nice resource for exploring pre-set themes is [available](https://ggplot2.tidyverse.org/reference/ggtheme.html) from Tidyverse. 

Let's add a layer `theme_bw()`. 

```r
# Add a pre-built theme
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold))  +
  theme_bw()
```

We are relatively close to achieving the figure from the paper. Let's look at our plot side-by-side with the paper figure:

<p align="center">
<img src="../img/volcano_plot_glia_combined1.png" width="500">
</p>

How are these plots different? Let's create a to-do list:

1. Change size of axes labels
2. Remove gridlines
3. Remove legend
4. Rename axes labels
5. Change scale of x-axis
6. Change color of points to purple and grey

We can address the first three tasks within the `theme()` layer. We do not remember the functions to perform all of these tasks, but we do look it up whenever we need to do something custom. To explore the different non-data components customizable within the `theme()` function, let's look at the [documentation](https://ggplot2.tidyverse.org/reference/theme.html).

Notice the options for customizing the axes, titles, tick marks, and legends, among others. Everything is customizable, you just have to know what to customize. Using the `example()` function may be a helpful, as well as some very thorough resources for plotting with ggplot2.

we can add arguments using `theme()` to change the size of axis labels ourselves. Since we will be adding this layer "on top", or after `theme_bw()`, any features we change will override what is set by the `theme_bw()` layer. 

Alternatively, we can adjust specific elements of the current default theme by adding the `theme()` layer and passing in arguments for the things we wish to change. Or we can use both.

Let's **increase the size of both the axes titles to be 1.5 times the default size.** When modifying the size of text the `rel()` function is commonly used to specify a change relative to the default.

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype,
  			shape=celltype), size=2.25) +
  theme_bw() +
  theme(axis.title = element_text(size=rel(1.5)))			
```
 
<p align="center">
<img src="../img/ggscatter-5.png" width="500">
</p> 

> *NOTE:* You can use the `example("geom_point")` function here to explore a multitude of different aesthetics and layers that can be added to your plot. As you scroll through the different plots, take note of how the code is modified. You can use this with any of the different geometric object layers available in ggplot2 to learn how you can easily modify your plots! 

> *NOTE:* RStudio provide this very [useful cheatsheet](../cheatsheets/data-visualization-2.1.pdf) for plotting using `ggplot2`. Different example plots are provided and the associated code (i.e which `geom` or `theme` to use in the appropriate situation.) We also encourage you to persuse through this useful [online reference](https://ggplot2.tidyverse.org/reference/) for working with ggplot2.


***

**Exercise**

1. The current axis label text defaults to what we gave as input to `geom_point` (i.e the column headers). We can change this by **adding additional layers** called `xlab()` and `ylab()` for the x- and y-axis, respectively. Add these layers to the current plot such that the x-axis is labeled "Age (days)" and the y-axis is labeled "Mean expression".
2. Use the `ggtitle` layer to add a plot title of your choice. 
3. Add the following new layer to the code chunk `theme(plot.title=element_text(hjust=0.5))`.
  * What does it change?
  * How many theme() layers can be added to a ggplot code chunk, in your estimation?


# basics not included:
Let's try to have both **celltype and genotype represented on the plot**. To do this we can assign the `shape` argument in `aes()` the celltype column, so that each celltype is plotted with a different shaped data point. 

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype,
  			shape=celltype)) 
```

<p align="center">
<img src="../img/ggscatter-3.png" width="500">
</p>


# Later and use full set of interesting genes

plot the expression of a single gene among the different samples in our dataset. The gene, Pax6, is used as an identifier for radial glia cells. We will explore it's expression in radial glia cells, downstream progenitor cells (identified using expression of the Tbr2 gene) and neurons (identified by having little expression of Pax6 or Tbr2).

Our gene expression data is stored in a data frame called `normalized_counts`. Counts are a measure of gene activity, indicating whether the gene is turned on or off (expressed) and the magnitude of its expression. We have counts for every gene for each of our samples, the higher the counts, the more turned on or expressed the gene. Let's take a look at the `normalized_counts` data frame.

```r
# Inspect the normalized counts data frame
View(normalized_counts)
```

We see that each gene is a different row and each sample is a different column. The numbers represent the magnitude of expression for each gene in every sample. Since we are only interested in the **Pax6** gene, we need to filter our dataset to return only this gene.

```r
pax6_expression <- norm_counts %>%
  filter(geneSymbol == "Pax6")
  
View(pax6_expression)
```

We see that we successfully returned counts for only the **Pax6** gene. Now, we want to visualize the expression or counts of this gene by plotting the sample on the x-axis and the normalized counts on the y-axis. It is important to note that `ggplot2` requires all data that is to be assigned to x- or y-coordinates (or any other plotting variable) are stored in a single column of the data frame. Since our samples and normalized counts values are stored in different columns, we need to 'gather' them together into a single column before we can plot them. To do this we can use a handy `tidyr` function called `pivot_longer()`.

The syntax for `pivot_longer()` is:

```r
# Syntax for `pivot_longer()` function
pivot_longer(input_data_frame,
             cols = columns_to_gather_together,
             names_to = "name_for_column_of_gathered_columns",
             values_to = "name_for_column_of_gathered_values")
```

For our `normalized_counts` data frame, we want to gather all of the normalized counts into a single column (all columns except for `geneSymbol`. Therefore, our gathering of columns from a wide to long format is:

```r
### Gather values to plot into a single column
expression_plot <- pivot_longer(pax6_expression,
                                cols = 2:25,
                                names_to = "samples",
                                values_to = "normalized_counts")
```

Finally, we hope to color our plot with information about the conditions of our samples. Currently we have only the expression data. We need to merge the metadata information for each of our samples. We can use a `_join` function from the Tidyverse or `merge()` base function.

```r
### Join metadata for visualizing groups or features
expression_plot <- left_join(x = expression_plot, 
                             y = meta, 
                             by = "samples")
```


################################

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

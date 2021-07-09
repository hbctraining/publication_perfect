---
title: "Plotting and data visualization in R"
author: "Mary Piper, Meeta Mistry, Radhika Khetani"
date: "Friday, July 9th, 2021"
---

Approximate time: 60 minutes

## Learning Objectives 

* Explain the syntax to utilize the "ggplot2" package to visualize data.

## Graphical syntax of `ggplot2`

Whenever we are working with data, it is usually helpful to display the it graphically to gain more insight. This is especially important for large datasets, where trends or relationships can be easily obscured. In this lesson we will be introducing the syntax for the popular Bioconductor package [`ggplot2`](http://docs.ggplot2.org/). Let's load the library for `tidyverse`, which is a suite of packages that include `ggplot2` for visualization, as well as some useful packages for wrangling (`dplyr`), parsing (`stringr`) and tidying (`tidyr`) data.

```r
# Load the Tidyverse suite of packages
library(tidyverse)
```

> _**NOTE:** Don't be alarmed by any conflict statements when loading this library, these statements are just informing you of packages that are currently loaded that have functions of the same name._

The `ggplot2` syntax takes some getting used to, but once you become comfortable with it, you will find it's extremely powerful and flexible. To start learning about `ggplot2` syntax we are going to re-create our first figure from the [The Epigenetic State of PRDM16-regulated Enhancers in Radial Glia Controls Cortical Neuron Position paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6667181/). We will build the figure below using the layering approach employed by  `ggplot2`. We will highlight the purpose and utility of each layer we add, highlighting it's flexibility and customization.

<p align="center">
<img src="../img/volcano_plot_glia.png" width="500">
</p>


Please note that `ggplot2` expects as input either a "data frame" or Tidyverse's version of a data frame called a "tibble" (you can find out more about tibbles [here](https://hbctraining.github.io/Training-modules/Tidyverse_ggplot2/lessons/intro_tidyverse.html)). To create this figure we will need the data present in our `results` data frame. This plot is examining the magnitude (log2 fold changes) and significance (p-adjusted values) of the differences in gene expression between the *Prdm16* KO and WT samples for every gene in the radial glia cells; each point is a gene in the dataset. Let's take a look at the `results` data frame.

```r
# Inspect the results data frame
View(results)
```

We see that each gene is a different row and each column corresponds to different statistics regarding the differences in gene expression between the KO and WT samples within the radial glia (`pax6` columns), intermediate progenitors (`tbr2` columns) and neurons (`neg` columns). For this plot, we are interested in the radial glia, which correspond to the `pax6` columns.

## Initializing the graph structure

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

The [RStudio cheatsheet for ggplot2]() is a bit overwhelming at first, but it can help with choosing the best geom for our data. With two continuous variables with points, we will choose to use `geom_point()`. Let's add a "geom" layer to our plot using the `+` operator.

```r
ggplot(new_metadata) +
  geom_point() # note what happens here
```

Why do we get an error? Is the error message easy to decipher?

We get an error because each type of `geom` usually has a **required set of aesthetics** to be set. "Aesthetics" are set with the aes() function and can be set either nested within `geom_point()` (applies only to that layer) or within `ggplot()` (applies to the whole plot).

The `aes()` function has many different arguments, and all of those arguments take columns from the original data frame as input. It can be used to specify many plot elements including the following:

* position (i.e., on the x and y axes)
* color ("outside" color)
* fill ("inside" color) 
* shape (of points)
* linetype
* size

To start, we will specify x- and y-axis since `geom_point` requires the most basic information about a scatterplot, i.e. what you want to plot on the x and y axes. All of the other plot elements mentioned above are optional.

```r
ggplot(new_metadata) +
     geom_point(aes(x = age_in_days, y= samplemeans))
```

<p align="center">
<img src="../img/ggscatter-1.png" width="500">
</p>

Now that we have the required aesthetics, let's add some extras like color to the plot. We can **`color` the points on the plot based on the genotype column** within `aes()`. You will notice that there are a default set of colors that will be used so we do not have to specify. Note that the legend has been conveniently plotted for us.

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype)) 
```

<p align="center">
<img src="../img/ggscatter-2.png" width="500">
</p>

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
The `ggplot()` function is used to **initialize the basic graph structure**, then we add to it. The basic idea is that you specify different parts of the plot using additional functions one after the other and combine them into a "code chunk" using the `+` operator; the functions in the resulting code chunk are called layers.

Let's start: 

```r
ggplot(new_metadata) # what happens? 
```

> If you don't have the new_metadata object, you can right-click to download and save an `rds` file [from here](https://www.dropbox.com/s/xwsaisraubghawt/new_metadata.rds?dl=1) into the project `data` folder, and load it in using the `new_metadata <- readRDS("data/new_metadata.rds")`.

You get an blank plot, because you need to **specify additional layers** using the `+` operator.

The **geom (geometric) object** is the layer that specifies what kind of plot we want to draw. A plot **must have at least one `geom`**; there is no upper limit. Examples include:

* points (`geom_point`, `geom_jitter` for scatter plots, dot plots, etc)
* lines (`geom_line`, for time series, trend lines, etc)
* boxplot (`geom_boxplot`, for, well, boxplots!)

Let's add a "geom" layer to our plot using the `+` operator, and since we want a scatter plot so we will use `geom_point()`.

```r
ggplot(new_metadata) +
  geom_point() # note what happens here
```

Why do we get an error? Is the error message easy to decipher?

We get an error because each type of `geom` usually has a **required set of aesthetics** to be set. "Aesthetics" are set with the aes() function and can be set either nested within `geom_point()` (applies only to that layer) or within `ggplot()` (applies to the whole plot).

The `aes()` function has many different arguments, and all of those arguments take columns from the original data frame as input. It can be used to specify many plot elements including the following:

* position (i.e., on the x and y axes)
* color ("outside" color)
* fill ("inside" color) 
* shape (of points)
* linetype
* size

To start, we will specify x- and y-axis since `geom_point` requires the most basic information about a scatterplot, i.e. what you want to plot on the x and y axes. All of the other plot elements mentioned above are optional.

```r
ggplot(new_metadata) +
     geom_point(aes(x = age_in_days, y= samplemeans))
```

<p align="center">
<img src="../img/ggscatter-1.png" width="500">
</p>

Now that we have the required aesthetics, let's add some extras like color to the plot. We can **`color` the points on the plot based on the genotype column** within `aes()`. You will notice that there are a default set of colors that will be used so we do not have to specify. Note that the legend has been conveniently plotted for us.

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype)) 
```

<p align="center">
<img src="../img/ggscatter-2.png" width="500">
</p>

Let's try to have both **celltype and genotype represented on the plot**. To do this we can assign the `shape` argument in `aes()` the celltype column, so that each celltype is plotted with a different shaped data point. 

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype,
  			shape=celltype)) 
```

<p align="center">
<img src="../img/ggscatter-3.png" width="500">
</p>

The data points are quite small. We can adjust the **size of the data points** within the `geom_point()` layer, but it should **not be within `aes()`** since we are not mapping it to a column in the input data frame, instead we are just specifying a number. 

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype,
  			shape=celltype), size=2.25) 
```

<p align="center">
<img src="../img/ggscatter-4.png" width="500">
</p>

The labels on the x- and y-axis are also quite small and hard to read. To change their size, we need to add an additional **theme layer**. The ggplot2 `theme` system handles non-data plot elements such as:

* Axis label aesthetics
* Plot background
* Facet label backround
* Legend appearance

There are built-in themes we can use (i.e. `theme_bw()`) that mostly change the background/foreground colours, by adding it as additional layer. Or we can adjust specific elements of the current default theme by adding the `theme()` layer and passing in arguments for the things we wish to change. Or we can use both.

Let's add a layer `theme_bw()`. 

```r
ggplot(new_metadata) +
  geom_point(aes(x = age_in_days, y= samplemeans, color = genotype,
  			shape=celltype), size=3.0) +
  theme_bw() 
```

Do the axis labels or the tick labels get any larger by changing themes?

No, they don't. But, we can add arguments using `theme()` to change the size of axis labels ourselves. Since we will be adding this layer "on top", or after `theme_bw()`, any features we change will override what is set by the `theme_bw()` layer. 

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

---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

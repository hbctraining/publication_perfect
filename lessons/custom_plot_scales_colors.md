# Consistent formatting using custom scales and colors

When using `ggplot2`, we have observed default values incorporated for our aesthetic mappings. For example, in our boxplot, the x- and y-axis labels and fill colors correspond to default values. By default, the column values in our data frame become the x- and y-axis labels and the colors assigned default to a standard set. 

While the defaults tend to look okay, we often desire customization of these values. To customize the aesthetic mappings (specifications made within the `aes()` function), **scales** are available for every mapping and data type.

## Scales

In `ggplot2`, scales control how the data is visualized on the plot, including how the data points look (size, shape, transparency, color, fill, etc), as well as, the appearance of the axes and legends. The ggplot2 book devotes an [entire section](https://ggplot2-book.org/scales.html) of chapters to scales, and understanding them will help make the ggplot2 syntax more intuitive.

### Position and axes

The first set of scales correspond to position and axes. For x- and y-axis scales, we can use the scale specific to our data to alter limits, breaks and tick mark labels. The most common scales are for discrete and continuous axis data:

* `scale_x_continuous()`: for continuous x-axis values
* `scale_y_continuous()`: for continuous y-axis values
* `scale_x_discrete()`: for categorical or qualitative x-axis values
* `scale_y_discrete()`: for categorical or qualitative y-axis values

There are also a few scales for common transformations and modifications often used when visualizing data, including (for the x-axis):

* `scale_x_log10()`
* `scale_x_reverse()`
* `scale_x_sqrt()`
* `scale_x_binned()`

Using these position or axis scales, we often desire to alter the axis limits (`limits`), axis breaks where the ticks are labeled (`breaks`), and tick labels (`labels`). Using the `scale_` functions, we can alter the **content** of the axes and ticks; however, it is worth noting that the **look and style** of the axes and labels are still altered within the `theme()` function.

Let's change the names of the x-axis labels to match the figure in the paper shown below.

<p align="center">
<img src="../img/published_Pax6_boxplot.png" width="800">
</p>

We'll remind ourselves what our current boxplot looks like:

```r
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, 
                   y=normalized_counts, 
                   fill=group)) +
  theme_bw() +
  ylab('Normalized counts') +
  xlab('') +
  ggtitle("Pax6") +
  personal_theme() +
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1))
```

Let's add a layer to our ggplot code to rename the labels on the x-axis. What scale function do you think we should use?

```r
# Boxplot with renamed x-axis values
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, 
                   y=normalized_counts, 
                   fill=group)) +
  theme_bw() +
  ylab('Normalized counts') +
  xlab('') +
  ggtitle("Pax6") +
  personal_theme() +
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1)) +
  scale_x_discrete(labels=c("Pax6:WT" = "Radial glia",
                            "neg:WT" = "Neurons", 
                            "Tbr2:WT" = "Progenitors"))
```

***

**Exercises**

Let's explore the other graphical elements using the position scales.

1. Change the limits of the **y-axis** to range from 0 to 100 and have breaks (where the ticks are labeled) to be every 20 instead of every 10.
2. Use a log10 scale for the **y-axis** using a `scale_` function.
3. Reverse the order of the **y-axis** using a `scale_` function.

***


###

While the default colors may be fine for many applications, they are often not sufficient to highlight the relationships of interest within our plot or are not optimal for the intended audience/publication. There are cheatsheets available for specifying the base R colors by [name](https://cpb-us-e1.wpmucdn.com/sites.ucsc.edu/dist/d/276/files/2015/10/colorbynames.png) or [hexadecimal]() code. We could individually specify the colors by providing them with a scale layer.




We can add a fill scale layer, and most often one of the following two scales will work:

- **`scale_fill_manual()`:** for categorical data or quantiles
- **`scale_fill_gradient()` family:** for continuous data. 

For our categorical data, we will add the `scale_fill_manual()` layer, specifying the desired color values. 


`scale_fill_manual()` layer.

```r
# Visualize the Pax6 boxplot with custom colors
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill=group)) +
  theme_bw() +
  ylab('Normalized counts') +
  xlab('') +
  ggtitle("Pax6") +
  personal_theme() +
  scale_x_discrete(labels=c("Pax6:WT" = "Radial glia", "neg:WT" = "Neurons", "Tbr2:WT" = "Progenitors")) + 
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1)) +
  scale_fill_manual(values = c("firebrick4", "lightbluesteel", "yellow2")
```

 
We can also use pre-created color palettes from external R packages. This [R Graph Gallery site](http://www.r-graph-gallery.com/ggplot2-color.html) has a nice interactive app for exploring how to find and incorporate desired colors into your plots.

Since the goal of this workshop is 'publication quality' plots, we should be aware of the significant portion of our population who are color-blind. To encourage the use of color-blind friendly selections, we will focus our attention on these palettes. We will identify those palettes from the packages `RColorBrewer` and `viridis`. 

## RColorBrewer palettes

We will start by exploring the `RColorBrewer` library, which contains color palettes designed specifically for the different types of data being compared, with palettes specifically highlighting sequential, qualitative, and diverging data. 

```r
# Load the RColorBrewer library
library(RColorBrewer)
```

The available palettes can be viewed using the `display.brewer.all()` function:

```r
# Check the available color palettes
display.brewer.all()
```

<p align="center">
<img src="../img/Rcolorbrewer_palettes.png" width="800">
</p>

The palettes are separated into three sections based on the type of data: 

- **Sequential palettes (top):** For sequential data, with lighter colors for low values and darker colors for high values.
- **Qualitative palettes (middle):** For categorical data, where the color does not denote differences in magnitude or value.
- **Diverging palettes (bottom):** For data with emphasis on mid-range values and extremes.

Let's explore changing the colors of our boxplot (shown below), created in the previous lesson using default colors. 

```r
# Visualize the Pax6 boxplot
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill=group)) +
  theme_bw() +
  ylab('Normalized counts') +
  xlab('') +
  ggtitle("Pax6") +
  personal_theme() +
  scale_x_discrete(labels=c("Pax6:WT" = "Radial glia", "neg:WT" = "Neurons", "Tbr2:WT" = "Progenitors")) + 
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1))
```

<p align="center">
<img src="../img/Pax6_boxplot.png" width="800">
</p>

The boxplot is colored by `group`, which is a categorical variable. Therefore, we will choose a **Qualitative palette** to assign contrasting colors to the groups. We can choose how many colors from the palette to include, which for our data will be three colors (one for each group). Let's choose the **Set1** palette and see how we like it. We can test the colors included in a palette by using the `display.brewer.pal()` function:

```r
# Testing the Set1 palette with three colors
display.brewer.pal(3, "Set1")
```

These colors do a nice job of differentiating between groups; however, they are not color-blind friendly. There is an argument within the `display.brewer.all()` function called `colorblindFriendly`. We can set this to `TRUE` to return the color-blind friendly palettes, and choose from there.

```r
# Finding color-blind friendly palettes
display.brewer.all(colorblindFriendly = TRUE)
```

We have a much more limited set of **Qualitative palettes** to choose from for our categorical data, and we no longer see the 'Set1' as an option to choose. Let's choose the 'Dark2' color-blind friendly palette.

```r
# Testing a color-blind friendly palette with three colors
display.brewer.pal(3, "Dark2")
```

To use this palette in our plots, we need to define it with `brewer.pal()` and we'll save it to a variable called `mypalette`.

```r
# Define a palette
mypalette <- brewer.pal(3, "Dark2")

# how are the colors represented in the mypalette vector?
mypalette
```

Those colors look okay, so let's test them in our plot.

```r
# Visualize the Pax6 boxplot with RColorBrewer palette
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill=group)) +
  theme_bw() +
  ylab('Normalized counts') +
  xlab('') +
  ggtitle("Pax6") +
  personal_theme() +
  scale_x_discrete(labels=c("Pax6:WT" = "Radial glia", "neg:WT" = "Neurons", "Tbr2:WT" = "Progenitors")) + 
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1)) +
  scale_fill_manual(values = mypalette)
```


Note that the same 'scale_manual' and 'scale_gradient' syntax can be used to change the features of other mappings within the aesthetics, such as `color`, `size`, `shape`, `linetype`, or `alpha`. More information can be found using `?scale_manual`. In addition, the [ggplot2 book](https://ggplot2-book.org/scales-guides.html) discusses scales in great detail and how they can be used to alter the mapping aesthetics.

> _**NOTE:** When we created our plot, we used the `fill` argument within the `aes()` function; therefore, to change the colors of these groups, we need to use the `scale_fill_manual()`. If we had used the `color` argument within the `aes()` function, we would use the `scale_color_manual()`._
> 
> Let's see how it would change our plot if we had used the `color` argument instead:
> 
> ```r
> # Visualize the Pax6 boxplot
> ggplot(pax6_exp) +
>   geom_boxplot(aes(x=group, y=normalized_counts, color=group)) +
>   theme_bw() +
>   ylab('Normalized counts') +
>   xlab('') +
>   ggtitle("Pax6") +
>   personal_theme() +
>   scale_x_discrete(labels=c("Pax6:WT" = "Radial glia", "neg:WT" = "Neurons", "Tbr2:WT" = "Progenitors")) + 
>   theme(axis.text.x = element_text(angle = 45, 
>                                    vjust = 1, 
>                                    hjust = 1)) +
>   scale_color_manual(values = mypalette)
> ```

These colors look nice and are color-blind friendly, so would be great for publication and presentations; however, often readers will print out our articles in black and white. The chosen palette is unlikely to show much difference in color for black-and-white publication. 

## Viridis palettes

The viridis palettes represent good choices for color-blind friendly palettes and printing in black-and-white.




By default, `scale_color_gradient()` creates a two color gradient from low to high. Since we plan to use more colors, we will use the more flexible `scale_color_gradientn()` function. To make the legend a bit cleaner, we will also perform a -log10 transform on the p-values (higher values means more significant).

```r
ggplot(bp_plot) +
  geom_point(aes(x = gene_ratio, y = GO_term, color = -log10(p.value)), 
             size = 2) +
  theme_bw() +
  theme(axis.text.x = element_text(size=rel(1.15)),
        axis.title = element_text(size=rel(1.15))) +
  xlab("Gene ratios") +
  ylab("Top 30 significant GO terms") +
  ggtitle("Dotplot of top 30 significant GO terms") +
  theme(plot.title = element_text(hjust=0.5, 
  	face = "bold")) +
  scale_color_gradientn(colors = mypalette)
			 
```

This looks good, but we want to add better name for the legend and we want to make sure the legend title is centered and bold. To do this, we can add a `name` argument to `scale_color_gradientn()` and a new theme layer for the legend title.

```r
ggplot(bp_plot) +
  geom_point(aes(x = gene_ratio, y = GO_term, color = -log10(p.value)), 
             size = 2) +
  theme_bw() +
  theme(axis.text.x = element_text(size=rel(1.15)),
        axis.title = element_text(size=rel(1.15))) +
  xlab("Gene ratios") +
  ylab("Top 30 significant GO terms") +
  ggtitle("Dotplot of top 30 significant GO terms") +
  theme(plot.title = element_text(hjust=0.5, 
  	face = "bold")) +
  scale_color_gradientn(name = "Significance \n (-log10(padj))", colors = mypalette) +
  theme(legend.title = element_text(size=rel(1.15),
	hjust=0.5, 
	face="bold"))
			 
```


<p align="center">
<img src="../img/dotplot6.png" width="700">
</p>

***
**Exercises**

1. Arrange `bp_oe` by `term_percent` in descending order.
2. Create a dotplot with the top 30 GO terms with highest `term_percent`, with `term_percent` as x-axis and `GO_term` as the y-axis.
3. [Optional] Color the plot using the palette of your choice.

***

So far we have explored many layers that can be added to any plot with the ggplot2 package. However, we haven't explored the different `geom`s available. The type of data you are plotting will determine the type of `geom` needed, but a nice summary of the main `geom`s is available on the [RStudio ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2016/11/ggplot2-cheatsheet-2.1.pdf).

Let's explore different `geom`s by creating a couple of different plots. We'll start with a bar plot of the number of genes per category. We can start with the most basic plot by specifying the dataframe, geom, and aesthetics. 

```r
ggplot(bp_plot) +
  geom_col(aes(x = GO_term, y = overlap.size))
```

<p align="center">
<img src="../img/barplot1.png" width="600">
</p>

This is a good base to start from, now let's start to customize. To add color to the bars, we can use the `fill` argument, and if we would like to add an outline color to the bars, we can use the `color` argument.

```r
ggplot(bp_plot) +
  geom_col(aes(x = GO_term, y = overlap.size),
           fill = "royalblue",
           color = "black")
```

Then we can provide our theme preferences, give the plot a title, and label our axes:

```r
ggplot(bp_plot) +
  geom_col(aes(x = GO_term, y = overlap.size),
           fill = "royalblue",
           color = "black") +
  theme(axis.text.x = element_text(size=rel(1.15)),
        axis.title = element_text(size=rel(1.15))) +
  theme(plot.title = element_text(hjust=0.5, 
                                  face = "bold")) +
  labs(title = "DE genes per GO process", x = NULL, y =  "# DE genes")
```

<p align="center">
<img src="../img/barplot2.png" width="600">
</p>

Note that instead of using the functions `xlab()`, `ylab()`, and `ggtitle()`, we can provide all as arguments to the `labs()` function.

Now we might be fairly happy with our plot, but the x-axis labelling needs some help. Within the `theme()` layer, we can change the orientiation of the x-axis labels with the `angle` argument and align the labels to the x-axis with the `hjust` argument.

```r
ggplot(bp_plot) +
  geom_col(aes(x = GO_term, y = overlap.size),
           fill = "royalblue",
           color = "black") +
  theme(axis.text.x = element_text(size=rel(1.15)),
        axis.title = element_text(size=rel(1.15))) +
  theme(plot.title = element_text(hjust=0.5, 
                                  face = "bold")) +
  labs(title = "DE genes per GO process", x = NULL, y =  "# DE genes") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

<p align="center">
<img src="../img/barplot3.png" width="600">
</p>

This is almost what we were looking for, but the labels are getting cut-off because the plotting area is too small. The `plot.margin` argument of the theme's `element_text()` function can be used to alter the plotting dimensions to make room for our labels.

```r
ggplot(bp_plot) +
  geom_col(aes(x = GO_term, y = overlap.size),
           fill = "royalblue",
           color = "black") +
  theme(axis.text.x = element_text(size=rel(1.15)),
        axis.title = element_text(size=rel(1.15))) +
  theme(plot.title = element_text(hjust=0.5, 
                                  face = "bold")) +
  labs(title = "DE genes per GO process", x = NULL, y =  "# DE genes") +
  theme(axis.text.x = element_text(angle = 45, 
                                   hjust = 1)) + 
  theme(plot.margin = unit(c(1,1,1,3), "cm"))
```

<p align="center">
<img src="../img/barplot4.png" width="600">
</p>

>**NOTE:** If we wanted to remove the space between the x-axis and the labels, we could add an additional layer for `scale_y_continuous(expand = c(0, 0))`, which would not expand the y-axis past the plotting limits.
  
### Exporting figures to file

There are two ways in which figures and plots can be output to a file (rather than simply displaying on screen). The first (and easiest) is to export directly from the RStudio 'Plots' panel, by clicking on `Export` when the image is plotted. This will give you the option of `png` or `pdf` and selecting the directory to which you wish to save it to. It will also give you options to dictate the size and resolution of the output image.

The second option is to use R functions and have the write to file hard-coded in to your script. This would allow you to run the script from start to finish and automate the process (not requiring human point-and-click actions to save).

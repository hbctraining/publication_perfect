# Consistent formatting using custom colors

When using `ggplot2`, we have observed default colors assigned whenever we use the `color` or `fill` arguments. While the default colors may be fine for some applications, they are often not sufficient to highlight the relationships of interest within our plot. There are cheatsheets available for specifying the base R colors by [name](https://cpb-us-e1.wpmucdn.com/sites.ucsc.edu/dist/d/276/files/2015/10/colorbynames.png) or [hexadecimal]() code. We could specify other colors available or use pre-created color palettes from external R packages. This [R Graph Gallery site](http://www.r-graph-gallery.com/ggplot2-color.html) has a nice interactive app for exploring how to find and incorporate desired colors into your code.

Since the goal of this workshop is 'publication quality' plots, we should be aware of the significant portion of our population who are color-blind. To encourage use of color-blind friendly color choices, we will focus our attention on these palettes. We will identify those palettes from the packages `RColorBrewer` and `viridis`. 

## RColorBrewer palettes

First, we need to load the `RColorBrewer` library, which contains color palettes designed specifically for the different types of data being compared. 

```r
# Load the RColorBrewer library
library(RColorBrewer)
```

Let's explore the different palettes available.

```r
# Check the available color palettes
display.brewer.all()
```

<p align="center">
<img src="../img/Rcolorbrewer_palettes.png" width="800">
</p>

The output is separated into three sections based on the suggested palettes for sequential, qualitative, and diverging data. 

- **Sequential palettes (top):** For sequential data, with lighter colors for low values and darker colors for high values.
- **Qualitative palettes (middle):** For categorical data, where the color does not denote differences in magnitude or value.
- **Diverging palettes (bottom):** For data with emphasis on mid-range values and extremes.

If our plotted values were sequential, we would choose from these palettes. Let's explore what the "Yellow, orange, red" palette looks like. We can choose how many colors from the palette to include, which may take some trial and error. We can test the colors included in a palette by using the `display.brewer.pal()` function, and changing if desired:

```r
# Testing the palette with six colors
display.brewer.pal(6, "YlOrRd")
```

The yellow might be a bit too light, and we might not need so many different colors. Let's test with three different colors:

```r
# Testing the palette with three colors
display.brewer.pal(3, "YlOrRd")

# Define a palette
mypalette <- brewer.pal(3, "YlOrRd")

# how are the colors represented in the mypalette vector?
mypalette
```

Those colors look okay, so let's test them in our plot. We can add a color scale layer, and most often one of the following two scales will work:

- **`scale_color_manual()`:** for categorical data or quantiles
- **`scale_color_gradient()` family:** for continuous data. 

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

# Not sure whether to use or not


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

### 2. Changing default colors

You can color the boxplot differently by using some specific layers:

1. Add a new layer `scale_color_manual(values=c("purple","orange"))`. 
	* Do you observe a change?
2. Replace `scale_color_manual(values=c("purple","orange"))` with `scale_fill_manual(values=c("purple","orange"))`.
	* Do you observe a change?
	* In the scatterplot we drew in class, add a new layer `scale_color_manual(values=c("purple","orange"))`, do you observe a difference?
	* What do you think is the difference between `scale_color_manual()` and `scale_fill_manual()`?
3. Back in your boxplot code, change the colors in the `scale_fill_manual()` layer to be your 2 favorite colors.
	* Are there any colors that you tried that did not work? 

You are not restricted to using colors by writing them out as character vectors. You have the choice of a lot of colors in R, and you can do so by using their *hexadecimal code*. For example, "#FF0000" would be red and "#00FF00" would be green similarly, "#FFFFFF" would be white and "#000000" would be black. [click here for more information about color palettes in R](http://www.cookbook-r.com/Graphs/Colors_(ggplot2)/#hexadecimal-color-code-chart).

**OPTIONAL Exercise:**

* Find the hexadecimal code for your 2 favourite colors (from exercise 3 above) and replace the color names with the hexadecimal codes within the ggplot2 code chunk.


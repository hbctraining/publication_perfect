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


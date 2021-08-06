# Statistical comparison annotations

Oftentimes an analysis requires statistical comparisons between groups and denote the significance on our plots. The `ggpubr` package provides easy-to-use functions to create ggplot2-based plots, while providing statistical comparisons between groups and customizable annotations. We will be using it in this lesson to add statistical annotations to our plot.

Below is our boxplot matching the publication with updated color-blind friendly shading. To annotate the **significance of the differences in expression** of the **Pax6 gene** in radial glia cells compared to progenitor cells and neurons, we will use the `ggpubr` package to add pre-computed statistical annotations onto the plot.

The main function we will be utilizing from `ggpubr` is `stat_pvalue_manual()`. This function can be added to a ggplot2 figure as a layer; however, it requires the aesthetics (`aes()`) to be specified in the `ggplot()` layer, which applies the aesthetics to all mappings in the plot. Therefore, we will move our `aes()` function from our `geom_boxplot()` layer to the `ggplot()` layer.

Then we need to provide the statistical annotations as a data frame or tibble with the following column names: 

I am following this [tutorial](https://www.datanovia.com/en/blog/ggpubr-how-to-add-p-values-generated-elsewhere-to-a-ggplot/), but it is not working for me. I need someone else to trouble-shoot this and I will work on the other lessons.

```r
# DON'T KNOW WHY THIS WHOLE CHUNK DOESN'T WORK

# Create statistic table of annotations - the p.adj values are random numbers I chose
stat.test <- tibble::tribble(
  ~group1, ~group2,   ~p.adj,    
  "Pax6:WT",     "Tbr2:WT", 0.003,
  "Pax6:WT",     "neg:WT", 0.0007,
  "Tbr2:WT",     "neg:WT", 0.01)
                            
# Add stats to plot
ggplot(pax6_exp, 
       aes(x=group,
           y=normalized_counts,
           fill=group)) +
  geom_boxplot() +
  ggtitle("Pax6") +
  personal_theme() +
  theme(axis.text.x = element_text(angle = 45, 
                                   vjust = 1, 
                                   hjust = 1)) +
  scale_x_discrete(name = "",
                   labels=c("Pax6:WT" = "Radial glia",
                            "neg:WT" = "Neurons", 
                            "Tbr2:WT" = "Progenitors")) +
  scale_y_continuous(name = "Normalized counts") +
  scale_fill_viridis(discrete = TRUE,
                     option = "viridis",
                     begin = 0.2 ) +
  stat_pvalue_manual(
    stat.test,
    label = "p.adj",
    y.position = c(60, 30, 70))

```

It would be nice to show some additional ways of showing the statistics: 

```r
# Such as:
p_labeling <- list(cutpoints = c(0, 0.0001, 0.001, 0.01, 0.05, 1), 
                   symbols = c("****", "***", "**", "*", "ns"))
```                   

This can be a short lesson - should be a shout out to the additional functionality of ggpubr, especially the ease of creating ggplot plots with a bit more intuitive framework: https://jtr13.github.io/cc20/brief-introduction-and-tutorial-of-ggpubr-package.html and https://cran.r-project.org/web/packages/ggpubr/readme/README.html.

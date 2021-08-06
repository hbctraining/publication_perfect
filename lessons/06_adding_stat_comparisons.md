# Statistical comparison annotations

Often times we need to perform a statistical comparisons and denote significance on our plots. The `ggpubr` package provides easy-to-use functions to perform statistical basic comparisons between groups and provide customizable annotations on top of a ggplot object. 

Below is our boxplot matching the publication with updated color-blind friendly shading. To explore the significance of the differences in expression of the Pax6 gene in radial glia cells relative to progenitor cells and neurons, we will use the `ggpubr` package to perform the statistical testing and to add the annotations onto the plot.

```r
ggplot(pax6_exp, aes(x=group, 
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
  stat_compare_means(comparisons = my_comparisons, label = "p.signif",symnum.args = p_labeling) + # Add pairwise comparisons p-value
  stat_compare_means(label.y = 75)
  ```

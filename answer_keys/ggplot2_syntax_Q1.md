## Exercise

Let's explore how to change the aesthetics of the data points. Different shapes are available, as detailed in the RStudio ggplot2 cheatsheet.

**1. Change all of the points in the plot to squares.**

```
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold),
             size = 2.0, shape = "square")
```

**2. Change the transparency of the points (alpha) to change with the base mean of Pax6.**

```
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold,
                 alpha = pax6_baseMean),
             size = 2.0)
```

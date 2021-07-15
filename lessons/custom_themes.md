
Now that we have the final volcano plot, let's save it to a variable, which we can use downstream to add our annotations.

```r
volcano_RG <- ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold))  +
  theme_bw() +
  theme(axis.title = element_text(size = rel(1.25)),
        axis.text = element_text(size = rel(1.15))) +
  xlab("Log2 fold change") + 
  ylab("-Log10 p-value") +
  ggtitle("Radial glia") +
  theme(plot.title = element_text(size = rel(1.5))) +
  theme(legend.position = "none") +
  theme(panel.grid = element_blank()) +
  theme(plot.title=element_text(hjust=0.5)) +
  scale_color_manual(values = c("grey", "purple")) +
  xlim(c(-3,3.5))
```

We need to create two additional volcano plots that should look very similar to this plot for the 'Intermediate progenitors' and the 'Cortical neurons'. To ensure consistency between our plots, it can be helpful to create custom themes.
***

**Exercise**

Create the volcano plots for the 'Intermediate progenitors' and the 'Cortical neurons' by using the `tbr_` and `neg_` columns, respectively. Save the plots to the variables `volcano_IP` and `volcano_neu`.

***

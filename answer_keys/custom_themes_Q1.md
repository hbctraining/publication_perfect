## Exercise

**1. Using your personal_theme(), create volcano plots for the 'Intermediate progenitors' and the 'Cortical neurons' by using the tbr_ and neg_ columns from the results data frame, respectively. Save the plots to the variables volcano_IP and volcano_neu.**

Intermediate progenitors

```
volcano_IP <- ggplot(results) +
  geom_point(aes(x = tbr2_log2FoldChange, 
                 y = -log10(tbr2_padj), 
                 color = tbr2_threshold)) +
  personal_theme() +
  ggtitle("Intermediate Progenitors") +
  scale_color_manual(values = c("grey", "purple")) +
  scale_x_continuous(name = "Log2 fold change", 
                     limits = c(-3, 3.5)) +
  scale_y_continuous(name = "-Log10 p-value")

```

Cortical neurons

```
volcano_neu <- ggplot(results) +
  geom_point(aes(x = neg_log2FoldChange, 
                 y = -log10(neg_padj), 
                 color = neg_threshold)) +
  personal_theme() +
  ggtitle("Cortical neurons") +
  scale_color_manual(values = c("grey", "purple")) +
  scale_x_continuous(name = "Log2 fold change", 
                     limits = c(-1.5, 1.5)) +
  scale_y_continuous(name = "-Log10 p-value")
```

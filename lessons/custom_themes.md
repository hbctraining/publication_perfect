---
title: "Creating consistent plots with custom themes"
author: "Mary Piper, Meeta Mistry, Radhika Khetani, Jihe Liu"
date: "Friday, July 16th, 2021"
---

Approximate time: 20 minutes

## Learning Objectives 

* Explain how to create custom themes to facilitate consistency between plots.

# Consistent formatting using custom functions

When publishing, it is helpful to ensure plots have consistent formatting. To do this we can create a custom function with our preferences for the theme. Remember the structure of a function is:

```r
name_of_function <- function(arguments) {
    statements or code that does something
}
```

We need to create two additional volcano plots that should look very similar to our original 'Radial glia' plot for the 'Intermediate progenitors' and the 'Cortical neurons'. The code for the original plot is displayed below:

```r
# Volcano plot for radial glia
ggplot(results) +
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


To ensure consistency between our volcano plots, we want our volcano plot themes to be consistent between all plots:

```r
theme_bw() +
  theme(axis.title = element_text(size = rel(1.25)),
        axis.text = element_text(size = rel(1.15))) +
  xlab("Log2 fold change") + 
  ylab("-Log10 p-value") +
  theme(plot.title = element_text(size = rel(1.5))) +
  theme(legend.position = "none") +
  theme(panel.grid = element_blank()) +
  theme(plot.title=element_text(hjust=0.5))
```

> You can also combine multiple arguments within the same `theme()` function:
>
> ```
> theme_bw() +
>   theme(axis.title = element_text(size = rel(1.25)),
>         axis.text = element_text(size = rel(1.15)),
>         plot.title = element_text(size = rel(1.5),
>                                   hjust = 0.5),
>         panel.grid = element_blank(),
>         legend.position = "none")
> ```

If there is nothing that we want to change when we run this, then we do not need to specify any arguments. Creating the function is simple; we can just put the code inside the `{}`:

```r
personal_theme <- function(){ 
  theme_bw() +
  theme(axis.title = element_text(size = rel(1.25)),
        axis.text = element_text(size = rel(1.15)),
        plot.title = element_text(size = rel(1.5),
                                  hjust = 0.5),
        panel.grid = element_blank(),
        legend.position = "none")
}
```

Now to run our personal theme with any plot, we can use this function in place of the lines of `theme()` code:

```r
ggplot(results) +
  geom_point(aes(x = pax6_log2FoldChange, 
                 y = -log10(pax6_padj), 
                 color = pax6_threshold)) +
  personal_theme() +
  ggtitle("Radial glia")  +
  xlab("Log2 fold change") + 
  ylab("-Log10 p-value") +
  scale_color_manual(values = c("grey", "purple")) +
  xlim(c(-3,3.5))
```

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

Using your `personal_theme()`, create volcano plots for the 'Intermediate progenitors' and the 'Cortical neurons' by using the `tbr_` and `neg_` columns in the `results` data frame, respectively. Save the plots to the variables `volcano_IP` and `volcano_neu`.

***


---
*This lesson has been developed by members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

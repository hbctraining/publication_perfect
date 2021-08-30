We have discovered that `ggplot2` has incredible functionality and versatility; however, it is not always the best choice for all graphics. There are a plethora of different packages that specialize in particular types of figures. The [Data to Viz resource](https://www.data-to-viz.com) provides popular packages for specialized plots along with the code to create them. 

Let's start by exploring how to create a Venn diagram in figure 4H. A venn diagram compares two or more lists, and by nature is categoric. If we use the [Data to Viz resource](https://www.data-to-viz.com), we can navigate to the `Categoric` data, and under 'Two independent lists' we find the Venn diagram. Click on this icon, and explore the dedicated page.

<Screen shot of 'dedicated page' link.>

This page has a lot of nice information about Venn diagrams, as well as, suggestions for when to use them (e.g. generally not recommended for comparison of more than 3 sets - use [upset plots](https://jku-vds-lab.at/tools/upset/) instead). Since we are comparing two sets of data for each visualization (e.g. Pax6 and Tbr2-expressing samples), a Venn diagram is a fine method.

Let's expand the 'Code', and note the package used to create the Venn diagrams is `VennDiagram`. We will use this package, as well. To generate the Venn diagram, the sets need to be given as a list. We can subset our data for the `Pax6`- and `Tbr2`-expressing samples to only include those genes that are significant with `threshold` equal to `TRUE` and that are up-regulated using the `log2FoldChange` values > 0.

```r
library(VennDiagram)

# Up-regulated
up1 <- results[which(results$pax6_threshold & results$pax6_log2FoldChange > 0),]
up2 <- results[which(results$tbr2_threshold & results$tbr2_log2FoldChange > 0),]
```

We can do a similar subset for the down-regulated genes, but with `log2FoldChange` < 0.

```r
# Down-regulated
down1 <- results[which(results$pax6_threshold & results$pax6_log2FoldChange < 0),]
down2 <- results[which(results$tbr2_threshold & results$tbr2_log2FoldChange < 0),]
```

Now that we have our sets of genes that are up- and down-regulated in the respective conditions, we can check how many overlap between the conditions. This is a good check to perform to make sure the packages report the correct interaction numbers.

```r
# Check overlap for each of the different sets
length(which(row.names(up1) %in% row.names(up2)))
length(which(row.names(down1) %in% row.names(down2)))
```

To create the figures we first need to change the plots into lists. 

```r
# Create lists for comparison
up <- list(
  PAX6 = rownames(up1),
  TBR2 = rownames(up2))

down <- list(
  PAX6 = rownames(down1),
  TBR2 = rownames(down2))
```

Now to create a simple Venn diagram, we can use the `venn.diagram()` function from the `VennDiagram` package to create the graphics.

```r
venn.diagram(x = up, 
             filename = "results/venn_up.png",
             output = TRUE)
```

This has successfully created a Venn diagram, but this is not exactly a publication-quality figure. A nice feature of the VennDiagram package is the ability for extensive customization of the graphic. Let's explore all of the different arguments available for our Venn diagram.

```r
# Check customizable options for diagram
?venn.diagram
```

Running the examples output in the help page can be quite illuminating when exploring the range of possibilities.

***
**Exercise?**

Customize some of these options to output a figure similar to that in the paper.

```r
# Up-regulated genes
venn.diagram(x = up, 
             filename = "results/venn_up.png",
             category.names = c("PAX6" , "TBR2"),
             output = TRUE,
             main = "Overlap of up-regulated genes",
             main.fontfamily = "sans",
             main.cex = 0.75,
             height = 600 , 
             width = 600 , 
             resolution = 300,
             compression = "lzw",
             lwd = 2,
             col= "gray",
             fill = c("salmon", "lightblue"),
             cex = 0.5,
             fontfamily = "sans",
             cat.cex = 0.7,
             cat.fontface = "bold",
             cat.default.pos = "outer",
             cat.pos = 0,
             cat.dist = c(0.05,0.097),
             cat.fontfamily = "sans",
             cat.col = c("salmon", "lightblue"))
```

<p align="center">
<img src="../img/venn_up.png" height="300">
</p>

```r
# Down-regulated genes
venn.diagram(x = down, 
             filename = "results/venn_down.png",
             category.names = c("PAX6" , "TBR2"),
             output = TRUE,
             main = "Overlap of down-regulated genes",
             main.fontfamily = "sans",
             main.cex = 0.75,
             height = 600 , 
             width = 600 , 
             resolution = 300,
             compression = "lzw",
             lwd = 2,
             col= "gray",
             fill = c("salmon", "lightblue"),
             cex = 0.5,
             fontfamily = "sans",
             cat.cex = 0.7,
             cat.fontface = "bold",
             cat.default.pos = "outer",
             cat.pos = 0,
             cat.dist = c(0.06,0.097),
             cat.fontfamily = "sans",
             cat.col = c("salmon", "lightblue"))

```

<p align="center">
<img src="../img/venn_down.png" height="300">
</p>

***


We will explore some additional graphing packages to create the heatmap (Fig4F) and the Venn diagram (Fig4H). While ggplot2 can easily create a heatmap with `geom_tile()`, it cannot easily provide the heirarchical clustering. We can use two packages that specialize in these plots to create these graphics, `pheatmap` and `VennDiagram`. Let's start wth the Venn diagram.Now we can explore `pheatmap` for our heatmap figure.

```r
# Get Pax6 sig genes
pax6_sig_genes <- results %>%
  rownames_to_column("ensembl_id") %>%
  filter(pax6_padj < 0.05) %>%
  pull(ensembl_id)

# Filter heatmap for only the radial glia (Pax6 samples)
heatmap_normCounts <- normalized_counts[ , colnames(normalized_counts)[str_detect(colnames(normalized_counts), "Pax6")]]

# Filter for the sig genes
heatmap_normCounts <- heatmap_normCounts[which(rownames(heatmap_normCounts) %in% pax6_sig_genes), ]


# Get annotations for samples
heatmap_meta <- meta

rownames(heatmap_meta) <- heatmap_meta$samples

heatmap_meta <- heatmap_meta[which(rownames(heatmap_meta) %in% colnames(heatmap_normCounts)), ]

heatmap_meta <- heatmap_meta %>%
  select(genotype)

heatmap_meta$genotype <- factor(heatmap_meta$genotype, levels = c("WT", "KO"))

heatmap_colors <- colorRampPalette(c("blue", "white", "red"))(6)

# Plot heatmap
library(pheatmap)
library(ggplotify)
ann_colors <- list(
  genotype = c(KO = "#F8766D",
               WT = "#00BFC4"))

pheatmap(heatmap_normCounts, 
         color = heatmap_colors,
         cluster_rows = T, 
         annotation_col = heatmap_meta,
         annotation_colors = ann_colors,
         border_color="white", 
         cluster_cols = T, 
         show_colnames = F, 
         show_rownames = F,  
         scale="row",
         treeheight_col = 25,
         cellwidth = 20,
         cellheight = 0.45,
         fontsize = 9,
         annotation_names_col = F,
         annotation_legend = F)

heatmap <- as.ggplot(pheatmap(heatmap_normCounts, 
                              color = heatmap_colors,
                              cluster_rows = T, 
                              annotation_col = heatmap_meta,
                              annotation_colors = ann_colors,
                              border_color="white", 
                              cluster_cols = T, 
                              show_colnames = F, 
                              show_rownames = F,  
                              scale="row",
                              treeheight_col = 25,
                              cellwidth = 20,
                              cellheight = 0.40,
                              fontsize = 9,
                              annotation_names_col = F,
                              annotation_legend = F))

heatmap <- heatmap + 
  ggtitle("Radial glia") +
  theme(plot.title = element_text(hjust=0.5))


heatmap_figure <- ggdraw(heatmap) +  
 draw_label("Prdm16 cKO", 
             x = 0.24, 
             y = 0.91,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "italic") +
  draw_label("Prdm16 WT", 
             x = 0.58, 
             y = 0.91,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "italic") +
  draw_label("Up-regulated genes",
             angle = 90,
             size = 9,
             x = 0.03,
             y = 0.5,
             hjust = 0,
             vjust = 0) +
  draw_label("Down-regulated genes",
             angle = 90,
             size = 9,
             x = 0.03,
             y = 0.09,
             hjust = 0,
             vjust = 0) +
  draw_label("Genotype",
             size = 9,
             x = 0.09,
             y = 0.8,
             hjust = 0,
             vjust = 0)
             
# Export as png
png(file = "results/heatmap_figure.png", units = "in", width = 4, height = 5.15, res = 500)
heatmap_figure
dev.off()

```

Add the rest of the figure (bar plots) and complete the full figure after aligning with cowplot. If adding bar plots as images, then we could do this here.


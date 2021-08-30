We have discovered that `ggplot2` has incredible functionality and versatility; however, it is not always the best choice for all graphics. It can be helpful to use a different package that specializes in the figures of interest. We will explore some additional graphing packages to create the heatmap (Fig4F) and the Venn diagram (Fig4H). While ggplot2 can easily create a heatmap with `geom_tile()`, it cannot easily provide the heirarchical clustering. We can use two packages that specialize in these plots to create these graphics, `pheatmap` and `VennDiagram`. Let's start wth the Venn diagram.

```r
library(VennDiagram)

#Up-regulated
up1 <- results[which(results$pax6_threshold & results$pax6_log2FoldChange > 0),]
up2 <- results[which(results$tbr2_threshold & results$tbr2_log2FoldChange > 0),]

# Down-regulated
down1 <- results[which(results$pax6_threshold & results$pax6_log2FoldChange < 0),]
down2 <- results[which(results$tbr2_threshold & results$tbr2_log2FoldChange < 0),]

length(which(row.names(up1) %in% row.names(up2)))
length(which(row.names(up1) %in% row.names(down2)))
length(which(row.names(down1) %in% row.names(down2)))
length(which(row.names(down1) %in% row.names(up2)))

up <- list(
  A = rownames(up1),
  B = rownames(up2))

down <- list(
  A = rownames(down1),
  B = rownames(down2))

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
<img src="../img/venn_up.png" height="300">
</p>


<p align="center">
<img src="../img/venn_down.png" height="300">
</p>


Now we can explore `pheatmap` for our heatmap figure.

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
```
<p align="center">
<img src="../img/meta_heatmap.png" height="300">
</p>

```r
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
```

<p align="center">
<img src="../img/heatmap.png" height="300">
</p>


```r
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

<p align="center">
<img src="../img/heatmap_figure.png" height="300">
</p>



Add the rest of the figure (bar plots) and complete the full figure after aligning with cowplot. If adding bar plots as images, then we could do this here.


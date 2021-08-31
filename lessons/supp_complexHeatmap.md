# Complex Heatmap

There is no row scaling argument in complexHeatmap, so we need to perform the scaling manually prior to creating the heatmap.

```r
# Manually scale and center the data
mat <- heatmap_normCounts
mat_scaled = t(apply(mat, 1, scale))
colnames(mat_scaled) <- colnames(heatmap_normCounts)

Make sure the metadata contains the same columns as the counts data (in the same order).

```r
# Reorder metadata to match counts
mat_meta <- heatmap_meta[match(colnames(mat_scaled), rownames(heatmap_meta)), , drop = FALSE]
```

Now create the heatmap using the `Heatmap()` function. There are so many arguments to customize and an incredible amount of functionality for [adding additional plots alongside the heatmap](https://jokergoo.github.io/ComplexHeatmap-reference/book/heatmap-decoration.html). The [ComplexHeatmap book](https://jokergoo.github.io/ComplexHeatmap-reference/book/) is suggested reading whenever creating a heatmap with this package.

```r
# Check options
?Heatmap

# Generate heatmap
hp <- Heatmap(mat_scaled,
              col = heatmap_colors,
              column_km = 2,
              row_km = 2,
              column_title = gt_render(c("*Prdm16* cKO", "*Prdm16* WT")),
              row_title = c("Up-regulated genes", "Down-regulated genes"),
              show_row_names = FALSE,
              show_column_names = FALSE,
              column_title_gp = gpar(fontsize = 14, fontface = "italic"),
              column_dend_reorder = c(4, 3, 2, 1, 2, 3, 1, 4),
              show_parent_dend_line = FALSE,
              row_gap = unit(0, "mm"),
              column_gap = unit(0, "mm"),
              heatmap_legend_param = list(
                title = "", at = c(-2, -1, 0, 1, 2)),
              top_annotation = HeatmapAnnotation(Genotype = anno_simple(mat_meta$genotype,
                                                             col = c(KO = "#F8766D",
                                                                     WT = "#00BFC4")),
                                                 annotation_name_side = "left"))

heatmap <- draw(hp,
     column_title_gp = gpar(fontsize = 16))
```

```r
# Change into ggplot object
gg_heatmap <- as.ggplot(hp)


# Add title with cowplot
ggdraw(gg_heatmap) +
  draw_label("Radial glia",
             x = 0.45,
             y = 0.98,
             size = 16,
             hjust = 0,
             vjust = 0,
             fontface = "plain")
```             
             

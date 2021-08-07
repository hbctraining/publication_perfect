There are a few different popular types of annotations to add to plots. These include:

1. Labeling all values
2. Adding custom text
3. Adding statistical comparison results

## Labeling all values (maybe find a better plot to show this if time permits)

If you would like to label all values on the plot, this can easily be done by adding another layer to the ggplot with `geom_text()` or `geom_label()`. The ggplot2 book provides a [nice resource](https://ggplot2.tidyverse.org/reference/geom_text.html) for exploring the functionality of these geoms.

```r
# Adding labels to all values with geom_text
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, 
                   y=normalized_counts, 
                   fill=group)) +
  geom_text(aes(x=group, 
                y=normalized_counts,
                label=samples)) +
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
                     begin = 0.2 )
```

The `ggrepel` package is a useful ggplot2 extension package that is helpful to prevent the overlap of labels. It can be added as a layer, similar to `geom_text`.

```r
library(ggrepel)

ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, 
                   y=normalized_counts, 
                   fill=group)) +
  geom_text_repel(aes(x=group, 
                y=normalized_counts,
                label=samples)) +
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
                     begin = 0.2 )
```                     
       
## Adding custom text

Let's explore adding custom text to our plot by finishing up our volcano plots we created previously. We are trying to create the figure below, but we still need to add the text to the image. Let's explore the current volcano plot for Pax6 results.

```r
# Pax6 volcano plot
volcano_RG
```

We have previously used `cowplot` to align plots and draw images, and we can use additional functionality from the `cowplot` package to add custom text to our figures. We will use `draw_label()` function. The `ggdraw()` function draws the ggplot2 image to the canvas, then the `draw_label()` functions adds the text on top. 

Let's practice by adding 'Draft' on top of our volcano plot. We can customize the appearance of the text within the `draw_label()` function. 

```r
# Explore the arguments for draw_label
?draw_label
```

***

## Exercise

Customize the 'Draft' text appearance using the different `draw_label()` arguments.

```r
# Practicing adding custom text to images
ggdraw(volcano_RG) + 
  draw_label("Draft", 
             color = "#FF0000", 
             size = 100, 
             angle = 45)
```

***

The trick to adding the text to figures is a lot of trial and error, since you need to specify the x- and y-coordinates for where you would like the labels to appear on the image. The coordinates span from 0 to 1 with (0,0) at the lower left-hand corner.

Center the 'Draft' in the middle of the image. **The x- and y-coordinates appropriate may be different, depending on the size of your plotting window.** 

```r
# Use the x, y, hjust and vjust arguments to center the text
ggdraw(volcano_RG) + 
  draw_label("Draft", 
             color = "#FF0000", 
             size = 100, 
             angle = 45,
             x = 0.35,
             y = 0.15,
             hjust = 0,
             vjust = 0)
```

Now that we know how to add text to an image, let's add the annotations to our volcano plot to match the published figure below.

published radial glia volcano plot

```r
# Add annotations to the RG volcano plot
## x and y need to be adjusted for the desired resolution
# We could also try writing to file then adding annotations after reading in with ggdraw()

ggdraw(volcano_RG) + 
  draw_label("664 genes", 
             x = 0.15, 
             y = 0.82,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "bold") +
  draw_label("Down \n247 genes", 
             x = 0.15,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0) +
  draw_label("Up \n417 genes", 
             x = 0.77,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0)  
```

To finish up the figure, we need to save each of the annotated volcano plots to variables and align with `cowplot` using `plot_grid()`.

```r
# Create annotations for all of the volcano plots and save to variables
volcano_panel1 <- ggdraw(volcano_RG) + 
  draw_label("664 genes", 
             x = 0.15, 
             y = 0.82,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "bold") +
  draw_label("Down \n247 genes", 
             x = 0.15,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0) +
  draw_label("Up \n417 genes", 
             x = 0.77,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0)  

volcano_panel2 <- ggdraw(volcano_IP) + 
  draw_label("405 genes", 
             x = 0.15, 
             y = 0.82,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "bold") +
  draw_label("Down \n164 genes", 
             x = 0.15,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0) +
  draw_label("Up \n241 genes", 
             x = 0.77,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0)  

volcano_panel3 <- ggdraw(volcano_neu) + 
  draw_label("33 genes", 
             x = 0.15, 
             y = 0.82,
             size = 12,
             hjust = 0,
             vjust = 0,
             fontface = "bold") +
  draw_label("Down \n21 genes", 
             x = 0.15,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0) +
  draw_label("Up \n12 genes", 
             x = 0.8,
             y = 0.2,
             size = 12,
             hjust = 0,
             vjust = 0)  

# Align volcano plots
volcano_grid <- plot_grid(volcano_panel1,
                          volcano_panel2,
                          volcano_panel3,
                          ncol = 3)

```

We will explore adding statistical comparison results in the next lesson.

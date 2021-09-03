Pivoting to Science

[Science submission instructions](https://www.sciencemag.org/authors/instructions-preparing-initial-manuscript)

- The figure’s title should be at the beginning of the figure legend, not in the figure itself
- **Panels should be set close to each other, and common axis labels should not be repeated.**
- Use scale bars in place of, or in addition to, magnifications. Do not use minor tick marks in scales or grid lines. Avoid using y-axis labels on the right that repeat those on the left.
- Avoid using red and green together. Color blind individuals will not be able read the figure.
- **Please do not use colors that are close in hue to identify different parts of a figure.**
- Avoid using grayscale.
- Use white type and scale bars over darker areas of images.
- Units should be metric and follow SI convention.
- **Use a sans-serif font whenever possible (we prefer Helvetica).**
- Capitalize the first letter in a label only, not every word (and proper nouns, of course).
- **Variables are always set in italics or as plain Greek letters (e.g., P, T, m). The rest of the text in the figure should be plain or bold text.**
- Type on top of color in a color figure should be in bold face. Avoid using color type
- **Use capital letters for part labels in multipart figures – A, B, C, etc. These should be 9 pt and bold in the final figure. When possible, place part labels at the upper left-hand corner of each figure part; if a part is an image, set labels inside the perimeter so as not to waste space. (not exactly sure how to do this)**

## Talk about altering personal_template() and personal_palette() to adhere to the requirements of the journal. 
  - We can change the personal_theme() to:
    - change all font to Helvetica
  - Change palette to be color-blind friendly and of different hues (which we already did, so we could just discuss this)
  - Show how we could do the other bold changes above - plain text adheres to our current figures, I think, but should double-check


***************

When submitting a manuscript for publication, we often aim for the highest impact journal to which we feel there is a decent probability of acceptance. However, often our manuscripts are not accepted at the first choice journal, and we need to pivot the format and figures to meet the guidelines of the next chosen journal. We will discuss coding features which can ease the resubmission process, many of which we have introduced previously, including:

- Creating a personal theme
- Creating a personal color palette
- Incorporating variables for plot features likely to change during the resubmission process

Some of the plotting features likely to change between journals include:

- text font type
- text font style
- text size
- figure labels (A, B, C, etc.)
- figure resolution
- figure size
- output file type (tiff, svg, eps, etc.)

## Altering the theme

Often different journals will have differing stylistic requirements pertaining the the thematic elements of the plot. We recommend creating a personal theme function to allow easy transitioning to a different set of requirements. Let's explore this a bit more. We'll start by viewing our current `personal_theme()`.

```r
View(personal_theme)

function(){ 
  theme_bw() +
    theme(axis.title = element_text(size = rel(1.25)),
          axis.text = element_text(size = rel(1.15)),
          plot.title = element_text(size = rel(1.5),
                                    hjust = 0.5),
          panel.grid = element_blank(),
          legend.position = "none")
}
```

We can add to this personal theme to ensure we are meeting the requirements for the journals. For instance, if we wanted to apply to the journal _Science_, we would need:
  - sans-serif font with Helvectica if possible
  - font size not smaller than 5 points
  - line width minimum of 0.5 pt

We could add our preferred text font and size (greater than the minimum listed in the journal's requirements) to our personal theme.

```r
# Adding journal requirements to theme
personal_theme <- function(){ 
  theme_bw() +
    theme(text = element_text(family = "Helvetica",
                              size = 6),
          line = element_line(size = 0.5),
          axis.title = element_text(size = rel(1.25)),
          axis.text = element_text(size = rel(1.15)),
          plot.title = element_text(size = rel(1.5),
                                    hjust = 0.5),
          panel.grid = element_blank(),
          legend.position = "none")
}
```

Alternatively, if we knew these were variables likely to change for resubmission, we could code for them as variables, such as:

```r
# Adding journal requirements to theme with variables
# Define likely to change variables at beginning of plotting script
font <- "Helvetica"
text_size <- 6
line_size <- 0.5

# Create personal theme with variables
personal_theme <- function(){ 
  theme_bw() +
    theme(text = element_text(family = font,
                              size = text_size),
          line = element_line(size = line_size),
          axis.title = element_text(size = rel(1.25)),
          axis.text = element_text(size = rel(1.15)),
          plot.title = element_text(size = rel(1.5),
                                    hjust = 0.5),
          panel.grid = element_blank(),
          legend.position = "none")
}
```

## Altering personal color palettes

Usually, a color-blind friendly palette comprised of differing hues should be acceptable to most journals. However, if we had followed the manuscript and not changed to a color-blind friendly palette, there are many journals that would not have accepted our color choices. For instance, the journal _Science_ among others has the following requirements:

  - Avoid using red and green together
  - Do not use colors that are close in hue 
 
 Luckily we have achieved both of these requirements by following best practices for color choices; however this is something to be aware of. We could easily pivot to a different color palette if we had created a personal palette containing our colors for each type of plot. Even though we used a ggplot2 layer for the viridis colors, it may be easier to pivot if you defined your personal palette to use. For instance, if we wanted to change our color palette we could easily change the palette:
 
```r
# Define new palette
personal_boxplot_palette <- c("#440154FF", "#228C8DFF", "#FDE725FF")

# ggplot2 code to remain the same
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, 
                   y=normalized_counts, 
                   fill=group)) +
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
  scale_fill_manual(values = personal_boxplot_palette)
```
 
  - Use capital letters for part labels in multipart figures – A, B, C, etc. These should be 9 pt and bold in the he upper left-hand corner of each figure part
  - place part labels at the upper left-hand corner of each figure part

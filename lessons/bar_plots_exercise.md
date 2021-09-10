Now that we have learned the basics of ggplot2 and how to combine different images using `cowplot`, let's create the bar plot figures in the publication (Figs 4I and 4J). We will complete this as an exercise and have the answers available.


<p align="center">
<img src="../img/fig4IJ.png" width="700">
</p>

## Set up
1. Let's read in our gene ontology data.

  ```r
  # Read in data for bar plot
  enriched_go_results <- read.csv("data/pp_gene_ontology_results.csv")
  ```
  < image of dataset >
  
2. Now, let's split the dataset into the Pax6 up-regulated, Pax6 down-regulated, Tbr2 up-regulated and Tbr2 down-regulated.
  
  ```r
  # Separate into the four datasets for the figure
  pax6_up_go <- enriched_go_results %>%
  filter(regulated == "up" & 
           cell.type == "Radial glia")

  pax6_down_go <- enriched_go_results %>%
    filter(regulated == "down" & 
             cell.type == "Radial glia")

  tbr2_up_go <- enriched_go_results %>%
    filter(regulated == "up" & 
             cell.type == "Intermediate progenitors")

  tbr2_down_go <- enriched_go_results %>%
    filter(regulated == "down" & 
             cell.type == "Intermediate progenitors")
  ```

3. Order the factor levels for the term names in the order you would like them to appear in the plot (from bottom to top) based on the `p.value`.

  ```r
  # Order terms
  ## Pax6 up
  pax6_up_order_levels <- pax6_up_go %>%
    arrange(p.value) %>%
    pull(term.name)

  pax6_up_go$term.name <- factor(pax6_up_go$term.name, 
                                 levels = rev(pax6_up_order_levels))
                                 
  ## Pax6 down
  pax6_down_order_levels <- pax6_down_go %>%
    arrange(p.value) %>%
    pull(term.name)

  pax6_down_go$term.name <- factor(pax6_down_go$term.name, 
                                 levels = rev(pax6_down_order_levels)) 
                                 
  ## Tbr2 up
  tbr2_up_order_levels <- tbr2_up_go %>%
    arrange(p.value) %>%
    pull(term.name)

  tbr2_up_go$term.name <- factor(tbr2_up_go$term.name, 
                                 levels = rev(tbr2_up_order_levels))       
                                 
  ## Tbr2 down
  tbr2_down_order_levels <- tbr2_down_go %>%
    arrange(p.value) %>%
    pull(term.name)

  tbr2_down_go$term.name <- factor(tbr2_down_go$term.name, 
                                   levels = rev(tbr2_down_order_levels))                                  
  ```

## Basic plot

4. Create a basic bar plot for the Pax6 up-regulated genes using `geom_col()`. Use the `coord_flip()` function as a layer to get the bars to go horizontally.

  < image of basic plot >

<details>
  <summary>Solution</summary>
  
 <p><pre>
  # Create the base of the bar plot
  ggplot(pax6_up_go) +
    geom_col(aes(x = term.name, 
                 y = -log10(p.value))) + 
    coord_flip()
  </pre></p>
  
</details>
  

## Add layers

5. Alter the color of the bars to be green inside and black outside.
6. Remove the title of the flipped x-axis
7. For the flipped y-axis, do the following:
  - Change the title to be `"-Log(P)"`.
  - Change the title to be in italics.
  - Set the limits to be 0 and 20.
  - Remove any expansion of the plot past the limits by adding the argument `expand(0,0)`.
8. Add a plot title with an extra line of space beneath it using a `ggtitle("Up-regulated genes \n")` layer.
9. Alter the thematic elements as follows:
  - Add your personal theme to ensure consistency of font sizing and background, etc.
  - Within the `theme()` function:
    - Remove the panel border
    - Make the axis lines black and of 0.5 size
    - Remove the y-axis text and ticks
10. Add the term names to the bars by adding a `geom_text()` layer with the same `x` and `y` aesthetics as the `geom_col()` layer. The `label` aesthetics should map to `term.name`.
  
  < image of altered plot >

<details>
  <summary>Solution</summary>
  
 <p><pre>
  ggplot(pax6_up_go) +
  geom_col(aes(x = term.name, 
               y = -log10(p.value)), 
           fill = "peachpuff", 
           color = "black") + 
  coord_flip() +
  scale_x_discrete(name = "") +
  scale_y_continuous(name = "-log(P)", 
                     limits = c(0, 20),
                     expand = c(0, 0)) +
  ggtitle("Up-regulated genes \n") +
  personal_theme() +
  theme(panel.border = element_blank(),
        axis.line = element_line(color = 'black', size = 0.5),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank()) +
  geom_text(aes(x = term.name,
                y = -log10(p.value),
                label = term.name,
                size = 4))
  </pre></p>
  
</details>
  

## Refine plot

11. Notice the terms are centered at the value we give to `y` in the `geom_text()` function. We can provide a different value to center the terms inside the bars. Let's instead make `y = -log10(p.value) / 2`.
12. Change the appearance of the term names within the `geom_text()` layer by doing the following:
  - Change the terms to sentence case (capital letter at the beginning) by changing the `label` argument in aesthetics to `label = str_to_sentence(term.name)`. The `str_to_sentence()` function is from the `stringr` package from the Tidyverse.
  - Add an argument to make all labels to be size of 4.
14. Change the appearance of the bars within the `geom_col()` layer by doing the following:
  - Create smaller bars using argument: `width = 0.6`
  - Add spacing between the bars using argument: `position = position_dodge(width=0.4)`

  < image of refined plot >
  
<details>
  <summary>Solution</summary>
  
 <p><pre>
  ggplot(pax6_up_go) +
    geom_col(aes(x = term.name, 
                 y = -log10(p.value)), 
             fill = "peachpuff", 
             color = "black",
             width=0.6, 
             position = position_dodge(width=0.4)) + 
    coord_flip() +
    scale_x_discrete(name = "") +
    scale_y_continuous(name = "-Log(P)", 
                       limits = c(0, 20),
                       expand = c(0, 0)) +
    ggtitle("Up-regulated genes \n") +
    personal_theme() +
    theme(panel.border = element_blank(),
          axis.line = element_line(color = 'black', size = 0.5),
          axis.text.y = element_blank(),
          axis.ticks.y = element_blank(),
          axis.title.x = element_text(face = "italic")) +
    geom_text(aes(x = term.name,
                  y = -log10(p.value)/2,
                  label = str_to_sentence(term.name),
                  size = 4))
  </pre></p>
  
</details>

## Adapt for the other three plots

15. Adapt the code from the Pax6 up-regulated genes to create the three other plots.

<details>
  <summary>Solution</summary>
  
 <p><pre>
  # Pax6 down-regulated
  ggplot(pax6_down_go) +
    geom_col(aes(x = term.name, 
                 y = -log10(p.value)), 
             fill = "peachpuff", 
             color = "black",
             width=0.6, 
             position = position_dodge(width=0.4)) + 
    coord_flip() +
    scale_x_discrete(name = "") +
    scale_y_continuous(name = "-log(P)", 
                       limits = c(0, 10)) +
    scale_y_reverse(name = "-log(P)",
                    expand = c(0, 0),
                    breaks = c(0, 2, 4, 6, 8, 10)) +
    ggtitle("Down-regulated genes \n") +
    personal_theme() +
    theme(panel.border = element_blank(),
          axis.line.x = element_line(color = 'black', size = 0.5),
          axis.line.y.right = element_line(colour = 'black', size = 0.5),
          axis.text.y = element_blank(),
          axis.ticks.y = element_blank()) +
    geom_text(aes(x = term.name,
                  y = -log10(p.value)/2,
                  label = str_to_sentence(term.name),
                  size = 4))
                
  # Tbr2 up-regulated
  ggplot(tbr2_up_go) +
    geom_col(aes(x = term.name, 
                 y = -log10(p.value)), 
             fill = "darkseagreen", 
             color = "black",
             width=0.6, 
             position = position_dodge(width=0.4)) + 
    coord_flip() +
    scale_x_discrete(name = "") +
    scale_y_continuous(name = "-log(P)", 
                       limits = c(0, 20),
                       expand = c(0, 0)) +
    ggtitle("Up-regulated genes \n") +
    personal_theme() +
    theme(panel.border = element_blank(),
          axis.line = element_line(color = 'black', size = 0.5),
          axis.text.y = element_blank(),
          axis.ticks.y = element_blank()) +
    geom_text(aes(x = term.name,
                  y = -log10(p.value)/2,
                  label = str_to_sentence(term.name),
                  size = 4))
                
  # Tbr2 down-regulated
  ggplot(tbr2_down_go) +
    geom_col(aes(x = term.name, 
                 y = -log10(p.value)), 
             fill = "darkseagreen", 
             color = "black",
             width=0.6, position = position_dodge(width=0.4)) + 
    coord_flip() +
    scale_x_discrete(name = "",
                     position = "top") +
    scale_y_continuous(name = "-log(P)", 
                       limits = c(0, 20)) +
    scale_y_reverse(expand = c(0, 0)) +
    ggtitle("Down-regulated genes \n") +
    personal_theme() +
    theme(panel.border = element_blank(),
          axis.line.x = element_line(colour = 'black', size = 0.5),
          axis.line.y.right = element_line(colour = 'black', size = 0.5),
          axis.text.y = element_blank(),
          axis.ticks.y = element_blank()) +
    geom_text(aes(x = term.name,
                  y = -log10(p.value)/2,
                  label = str_to_title(term.name)),
              hjust = 0.5,
              vjust = 0.5,
              size = 4)
  </pre></p>
  
</details>
  
  < image of all plots >
  
16. The last step is to use `cowplot` to put the remaining annotations into the images and align them into a single row in the figure.

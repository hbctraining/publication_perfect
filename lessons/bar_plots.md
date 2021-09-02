Now that we have learned the basics of ggplot2 and how to combine different images using `cowplot`, let's create the bar plot figures in the publication (Figs 4I and 4J). We will complete this as an exercise and go through it together.

1. Let's read in our gene ontology data.

  ```r
  # Read in data for bar plot
  enriched_go_results <- read.csv("data/pp_gene_ontology_results.csv")
  ```
  < image of dataset >
  
2. Now, let's split the dataset into the Pax6 up-regulated, Pax6 down-regulated, Tbr2 up-regulated and Tbr2 down-regulated.
  
  ```r
  Separate into the four datasets for the figure

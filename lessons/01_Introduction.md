## Workshop overview

Novel and exciting discoveries in scientific research require extensive labor and dedication, but even when we have made scientific revelations, it is not meaningful unless we can communicate those breakthroughs with other members of our community. A primary means of communicating our findings is through the visualization of our results. Yet, one of the biggest challenges in disseminating research findings is visualizing the results in a way that is meaningful, easy to interpret, aesthetically pleasing, and reproducible. In this workshop, we will provide the tools and resources to aid in developing visualizations most appropriate for your data and the methods to combine those visualizations into publication figures.

## Introduction to data visualization

Visualization theory is an important for the creation of interpretable graphics; however, we ill not be exploring it in-depth. Instead, we will focus on it's application through the use of the `ggplot2` R package, which incorporates the tenets of visualization theory into practice. That being said, understanding the key elements of visualization theory can only help when creating data graphics, and there are a wealth of books and other resources available to learn more about the theory of visualization, including the fundamental book on visualization theory by Edward Tufte, [The Visual Display of Quantitative Information](https://www.edwardtufte.com/tufte/books_vdqi). 

In this workshop, we will focus specifically on statistical graphics using the `ggplot2` package. This package is based on the visualization theory outlined by Leland Wilkinson in the book [The Grammar of Graphics](https://www.springer.com/gp/book/9780387245447). The `ggplot2` package incorporates the visualization theory presented in the book to generate and customize quantitative graphics. This workshop will focus on the **application of the `ggplot2` package for the production of meaningful, interpretable graphics**, while extending it's functionality using additional ggplot2 extension packages and external packages.

## Types of graphics

Oftentimes, the most basic visualization question about what type of plot to create for your data is the most difficult to decide. The following questions often plague researchers looking for the perfect visualization:

1. What type of graphics are available?
2. Is my desired graphic appropriate for my data?
3. How do I create this graphic using my data?

Recently, a fabulous website was released, [from Data to Viz](https://www.data-to-viz.com), that can help address some of these fundamental questions. Regardless of whether your data to plot is from genomics, chemistry, economics, business, etc., the data type generally be grouped as:
 
 - Numeric
 - Categoric
 - Numeric and categoric
 - Maps
 - Network
 - Time series

Depending on the type of data you are working with will determine which graphic options are available to you. The [from Data to Viz](https://www.data-to-viz.com) website provides decision trees based on the type of data you have. 

Let's navigate to the [website](https://www.data-to-viz.com) and explore. Let's assume we have categoric data with one variable:

1. Click on the `Categoric` button.
2. Choose decision tree with `One categorical variable`. Note that if you hover your cursor over this node, an example of this type of data will appear.
3. Listed beneath the node are a multitude of plots that would be appropriate for our categoric data. Let's click on the `Lollipop` graphic.
4. A brief description of the plot pops up, and if you scroll to the bottom, a link to the `Dedicated page` is available. Click on the link to the dedicated page.
5. Notice this page gives us information about when to use this plot, the variations that are available, as well as, clickable code for creating this plot in R. Let's click on the `CODE` button for the first plot (in the Definition section). 

Using this resource can help us navigate all of our basic questions, from using the decision tree to decide the type of plots available and appropriate for our data and how to create that plot using R code. However, the code may not be very interpretable to you now, since it is using the `ggplot2` package to create the plot. We will learn how to use the `ggplot2` package, and by the end of this workshop you should understand the code used to create this figure. 

## Introduction to the dataset



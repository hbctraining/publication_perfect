
## Exercises

Let's explore the other graphical elements using the position scales.

**1. Change the limits of the **y-axis** to range from 0 to 100 and have breaks (where the ticks are labeled) to be every 20 instead of every 10.**

```r
  scale_y_continuous((name="Normalized Counts"),
                     limits = c(0, 100), 
                     breaks = c(20, 40, 60, 80, 100)) 
```


**2. Use a log10 scale for the **y-axis** using a `scale_` function.**

```r
scale_y_log10()
```

## Exercise

**1. Use the ggtitle layer to add the plot title.**

```
  ggtitle('Radial Glia')

```

**2. Increase the size of the plot title to be 1.5 times the default value.**

```
  theme(plot.title = element_text(size = rel(1.5))) 
  
```

**3.Remove the legend by adding a theme() layer with the argument legend.position.**

```
theme(legend.position = "none") 
```

**4. Remove the gridlines by adding another theme() layer with the argument panel.grid.**

```
theme(panel.grid = element_blank()) 
```

**5. Add the following new layer to the code chunk theme(plot.title=element_text(hjust=0.5)). What does it change?**

It centers the plot title.

```
theme(plot.title=element_text(hjust=0.5)) 
```

**6. How many theme() layers can be added to a ggplot code chunk, in your estimation?**

There is no upper limit.

## Exercise

**1. Create a boxplot using the geom_boxplot() layer to plot the normalized counts of the different groups (neg:WT, Pax6:WT, Tbr2:WT).**

```
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts)) 

```


**2. Use the fill aesthetic to look at differences between groups (neg:WT, Pax6:WT, Tbr2:WT).**

```
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill = group)) 
```

**3. Add a title matching the published figure.**

```
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill = group)) +
  ggtitle("Pax6") 
```


**4. Re-factor the x-axis variable 'group' to be in the same order as the paper using the following code:**

```
### Re-factor the x-axis variable 'group' to be in the correct order
pax6_exp$group <- factor(pax6_exp$group, levels = c("Pax6:WT", "Tbr2:WT", "neg:WT"))

ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill = group)) +
  ggtitle("Pax6") 

```

**5. Change the angle of the x-axis labels to match the figure below using the theme() function (this resource can be helpful): the hjust and vjust arguments can help your plot look more appealing.**

```
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill = group)) +
  ggtitle("Pax6") + 
  theme(axis.text.x = element_text(angle=45, vjust = 1, hjust =1))
```

**6. Use your personal theme to keep your plots consistent.**

```
ggplot(pax6_exp) +
  geom_boxplot(aes(x=group, y=normalized_counts, fill = group)) +
  ggtitle("Pax6") + 
  theme(axis.text.x = element_text(angle=45, vjust = 1, hjust =1)) +
  personal_theme() 
```

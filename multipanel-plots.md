Multipanel Figures
================

## Making single panels

We will start by making four plots, from four different datasets, all
tagged with labels A-D. Note the different themes used, and the control
of figure width and height in the options for each chunk.

``` r
mtcars %>% 
  filter(cyl %in% c(4,6,8)) %>% 
  mutate(cyl = as.factor(cyl)) %>% 
  ggplot(aes(cyl, mpg)) +
  geom_boxplot() + labs(tag = 'A') +
  theme_linedraw() ->
p1
print(p1)
```

![](multipanel-plots_files/figure-gfm/panel1-1.png)<!-- -->

``` r
iris %>% 
  ggplot(aes(Species, Sepal.Length)) +
  geom_violin() +
  geom_jitter(alpha = 0.3, width = 0.2) + labs(tag = "B") +
  theme_cowplot() ->
p2
print(p2)
```

![](multipanel-plots_files/figure-gfm/panel2-1.png)<!-- -->

``` r
df <- data.frame(co2, month = 1:468) 
df %>% 
  ggplot(aes(month, co2)) +
  geom_line() +
  labs(x = 'month', y = 'CO2 concentration\nin ppm at Mauna Loa',
       caption = '1959 - 1997', tag = "C") +
  theme_dark() ->
p3
print(p3)
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

![](multipanel-plots_files/figure-gfm/panel3-1.png)<!-- -->

``` r
DNase %>% 
  ggplot(aes(conc, density)) +
  geom_jitter(alpha =0.4, width = 0.2) +
  geom_smooth() +
  labs(x = 'concentration of DNAse', y = 'Optical Density',
       caption = '11 replicates', tag = "D") +
  theme_half_open() ->
p4
print(p4)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/panel4-1.png)<!-- --> \#\# Now put
them together

Let’s start with patchwork, which is described here:
<https://github.com/thomasp85/patchwork>

### Self-arranging grid

``` r
p1 + p2 + p3 + p4
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/patches-1.png)<!-- -->

### One figure on top, then 3 below

``` r
p1 / (p2 + p3 + p4)
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/patches2-1.png)<!-- --> \#\#\# One
row of figures

``` r
p1 + p2 + p3 + p4 + plot_layout(nrow =1)
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/patches3-1.png)<!-- -->

### 3 on top, then one below

``` r
(p1 + p2 + p3) / p4
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/patches4-1.png)<!-- -->

### One figure on top, two in middle row, then one below

``` r
p1 + (p2 + p3) + p4 + plot_layout(ncol = 1) & theme_bw()
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/patches5-1.png)<!-- -->

## Now to cowplot

Cowplot uses a slightly different approach. the cowplot package can be
found at
<https://cran.r-project.org/web/packages/cowplot/vignettes/introduction.html>

Details here
<https://cran.r-project.org/web/packages/cowplot/cowplot.pdf>

### first remove tags, then 2 plots

``` r
# remove tags
p1 <- p1 + labs(tag = "")
p2 <- p2 + labs(tag = "")
p3 <- p3 + labs(tag = "")
p4 <- p4 + labs(tag = "")
plot_grid(p1, p2, labels = c('A', 'B'), label_size = 12)
```

![](multipanel-plots_files/figure-gfm/cowplot1-1.png)<!-- -->

### Now align axes to make it neater

``` r
plot_grid(p1, p2, labels = c('A', 'B'), label_size = 12, align = 'h')
```

![](multipanel-plots_files/figure-gfm/cowplot2-1.png)<!-- -->

### Single column

``` r
plot_grid(p1, p3, labels = c('A', 'B'), label_size = 12, align = 'v', ncol = 1)
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

![](multipanel-plots_files/figure-gfm/cowplot3-1.png)<!-- -->

### Control widths

``` r
plot_grid(p2, p4, labels = c('B', 'D'), label_size = 12, rel_widths = c(1, 0.5),
          align = 'h')
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/cowplot4-1.png)<!-- -->

### Auto label with caps, control scale.

``` r
plot_grid(p1, p2, p3, p4, labels = c('AUTO'), scale = c(1, 0.9, 0.9, 0.6))
```

    ## Don't know how to automatically pick scale for object of type ts. Defaulting to continuous.

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](multipanel-plots_files/figure-gfm/cowplot5-1.png)<!-- -->

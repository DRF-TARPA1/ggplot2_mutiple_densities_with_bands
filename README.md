# ggplot2_mutiple_densities_with_bands
This example illustrates showing multiple densities with ggplot2 by displaying bands of +/- 10% around the mean like this:


<img src="doc/images/example_ggplot2_densities_plot_with_color_bands.png">


with that:

```r
getwd()

if(!file.exists("~/.Rprofile")) # only create if not already there
  file.create("~/.Rprofile")    # (don't overwrite it)
file.edit("~/.Rprofile")

```


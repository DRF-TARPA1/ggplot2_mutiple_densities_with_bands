# ggplot2_mutiple_densities_with_bands
This example illustrates showing multiple probability densities with ggplot2 by displaying bands of +/- 10% around the mean like this:


<img src="doc/images/example_ggplot2_densities_plot_with_color_bands.png">


with that:



```r
# ---
# title: "ggplot2 example of fill density plot in R
# author: "Patrice Tardif"
# date: "2019-10-14"
# context: Example for  by Catherine Périé for project_cc (climatic change)
#
# This example illustrates showing multiple densities with ggplot2 by displaying bands around the average
# ---

library(tidyverse)


#Loading data in a dataframe with the classic Iris dataset
df <- iris


# Get density information for each group
densities <- df %>%
  group_by(Species) %>%
  do(., dens = density(.$Sepal.Length)[c("x","y")])   


# Bind these into a data frame in order to plot them using ggplot2
densities_df <-
  lapply(1:nrow(densities), function(r){cbind(ess = densities$Species[r],
                                              as.data.frame(densities$dens[r]),
                                              xbar = mean(unlist(densities$dens[r][[1]]["x"]),na.rm = T))}) %>% 
  Reduce(function(...) merge(..., all = T), .) %>% 
  mutate(
    gr_class = case_when(
      (x < 0.9*xbar) ~ "low",
      (x >=  0.9*xbar & x <  1.1*xbar) ~ "middle",
      (x >=  1.1*xbar) ~ "high",
      TRUE ~ NA_character_
    )
  )


# Extract the mean per group (xbar) to allow ggplot to display a line where the group average is located
mean_df <- densities_df %>% 
  group_by(ess) %>% 
  summarise(xmean = mean(xbar))


# Plot data
ggplot(densities_df, aes(x = x, y = y)) + 
  geom_area(data = filter(densities_df, gr_class == 'low'), fill = '#00AFBB') +
  geom_area(data = filter(densities_df, gr_class == 'middle'), fill = '#E7B800') +
  geom_area(data = filter(densities_df, gr_class == 'high'), fill = '#FC4E07') +
  geom_line(size = 1) +
  geom_vline(aes(xintercept = xmean), mean_df, colour = "red") +
  facet_wrap(~ess, ncol = 1)
 
```


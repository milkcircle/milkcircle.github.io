---
layout: post
title: Visualizing pre-transfusion blood parameters
date: 2019-01-29 13:00:00 -0700
categories: explanatory
math: true
comments: true
author: Aaron
---

There is no easy way for patients who are chronically transfused to visualize important blood parameters. For patients with aplastic anemia, MDS, thalassemia, etc. who are transfused on a regular basis (typically about once a month), parameters of interest include ferritin (a proxy for total body iron levels) and hemoglobin. Beloow I share a script to generate a visual plot of both parameters, which may be useful for patients who are able to access their numbers from their hospital system.  

The generated plot looks like this, and allows for easy comparison of ferritin and pre-transfusion hemoglobin trends:  

![]({{ site.url }}/images/transfusion_ferritin_hgb.png)  

Though this image only shows data in 2018, the script may be used for as much data as a patient readily has on hand. As I had to manually input these values, I decided to use one year as a starting point. Even from this limited plot, one might notice at least a small inverse correlation between ferritin and pre-transfused hemoglobin (PTH); a rise in ferritin may be correlated with a low PTH. One potential reason may be that a low PTH triggers a greater transfusional blood volume, which will in turn increase ferritin. If this were the case, the rise in ferritin should be offset by one measurement when compared to the PTH.  

Next steps include 1) correlating days between transfusions and change in Hgb, 2) incorporating transfused blood volume into the model, and 3) generating a predictive model for guessing the next measured Hgb based on information from prior transfusions. Eventually this may be adapted into a suite of tools with an improved user interface for patients and families to track their transfusions and spark more data-driven discussions with their physicians about transfusion frequency/volume and chelator dosing.

~~~ R
# A script to visualize ferritin and pre-transfusion Hgb
library(ggplot2)
library(dplyr)
library(scales)
library(grid)
library(reshape2)

# Import ferritin trends
blood <- as_tibble(read.csv('data/2018-Ferritin.csv', header = T, stringsAsFactors = F))
blood <- as_tibble(transform(blood, Date = as.Date(Date)))
ferritins <- dplyr::select(blood, Date, Ferritin)
hgb <- as_tibble(dplyr::select(blood, Date, Hgb))

# PLOT FERRITIN OVER TIME
fplot <- ggplot(data=ferritins, aes(x=Date, y=Ferritin)) +
  geom_point(color="#008db9") +
  geom_smooth(method = "loess", color="#385a7c") + 
  scale_x_date(date_breaks = "1 month",
               labels = date_format("%b"),
               expand = c(0,0)) + 
  labs(title = "2018 Ferritins",
       subtitle = "2018-01-05 to 2018-12-15",
       x = "Month", 
       y = "Ferritin in ng/mL") +
  theme_classic() +
  theme(plot.title = element_text(hjust=0.5), 
        plot.subtitle = element_text(hjust=0.5))

# PLOT PRE-TRANSFUSION HGB OVER TIME
hplot <- ggplot(data=hgb, aes(x=Date, y=Hgb)) +
  geom_point(color="#e8606a") +
  geom_smooth(method="loess", color="#d0565f") +
  scale_x_date(date_breaks = "1 month",
               labels = date_format("%b"),
               expand = c(0,0)) +
  labs(title = "2018 Pre-transfusion Hgbs",
       subtitle = "2018-01-05 to 2018-12-15",
       x = "Month", 
       y = "Hgb in g/dL") +
  theme_classic() +
  theme(plot.title = element_text(hjust = 0.5),
        plot.subtitle = element_text(hjust=0.5))

# This is a finicky bit of code that converts the plot into a Grob 
# so the points at either extreme of the graph are not cut off.
fp <- ggplotGrob(fplot)
fp$layout$clip[fp$layout$name=="panel"] <- "off"

hp <- ggplotGrob(hplot)
hp$layout$clip[hp$layout$name=="panel"] <- "off"

# Draw separate graphs
grid.draw(fp) # Draw ferritin plot 
grid.draw(hp) # Draw hemoglobin plot

# Stacking plots on top of each other
# First, melt the input dataframe to have each value in one column, and the other
# column as the variable label
melted <- melt(blood, measure.vars = c("Ferritin", "Hgb"))

# Manually choose colors
group.colors <- c(Ferritin = "#008db9", Hgb = "#e8606a")

# Include units in melted
graph_labeller <- c(`Ferritin` = "Ferritin (ng/mL)",
                    `Hgb` = "Hgb (g/dL)")

# PLOT STACKED FERRITIN/HGB OVER TIME
ggplot(melted, aes(x=Date, y=value, color=variable)) + 
  geom_point() + 
  scale_colour_manual(values = group.colors, guide=F) + 
  geom_smooth(method = "loess") + 
  scale_x_date(date_breaks = "1 month",
               labels = date_format("%b"),
               expand = c(0,0)) +
  labs(title = "2018 Transfusions",
       subtitle = "1/05/2018 to 12/15/2018", 
       x="Month",
       y="") + 
  theme_classic() +
  theme(plot.title = element_text(hjust = 0.5),
        plot.subtitle = element_text(hjust=0.5)) +
  facet_grid(variable~., 
             scales="free_y",
             labeller = as_labeller(graph_labeller))
~~~
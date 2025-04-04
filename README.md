---
title: "payoff Table without probability"
author: "Kyaw Min Khaing"
date: "2025-03-22"
output: html_document
---
##### Load library
```{r}
library(pacman)
p_load(tidyverse,knitr)
```

##### Load dataset build-in R 

```{r}
data("PlantGrowth")
```

##### Create payoff table

```{r pressure, echo=FALSE}
payoff_table <- PlantGrowth %>%
  group_by(group) %>%
  summarise(
    High_Payoff = max(weight),
    Low_Payoff = min(weight)
  )

# Display table
payoff_table
```
##### Optimistic Approach (MAXIMAX)
```{r}
# MAXIMAX Approach
maximax_decision <- payoff_table %>%
  filter(High_Payoff == max(High_Payoff))

maximax_decision

```
##### Optimistic Approach (MAXIMAX)
```{r}
# MAXIMIN Approach
maximin_decision <- payoff_table %>%
  filter(Low_Payoff == max(Low_Payoff))

maximin_decision

```
##### (Opportunity Loss Table)
```{r}
# Create Regret Table
regret_table <- payoff_table %>%
  mutate(
    Regret_High = max(High_Payoff) - High_Payoff,
    Regret_Low = max(Low_Payoff) - Low_Payoff
  )

# Display Regret Table
regret_table

```
##### Minimax Regret Approach 
```{r}
minimax_regret <- regret_table %>%
  mutate(Max_Regret = pmax(Regret_High, Regret_Low)) %>%
  filter(Max_Regret == min(Max_Regret))

minimax_regret

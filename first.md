---
title: "R Tutorial 2: Data structures"
author: "SH"
date: "22/06/2019"
output: 
    html_document:
      toc: TRUE
      toc_float: TRUE
      code_folding: hide
---

# LOAD PACKAGES

Note: Install the "*tidyverse*" and "*psych*" if you have not already done so.

```{r, results="hide"}
# Load package
library(tidyverse)  # for data wrangling
library(psych) # for required for descriptive stats
```
---
# A. DATA FRAMES

## 1. Traditional method of creating data frames
Data frames (similar to table) can be created, first by constructing vectors, then combinind them:

```{r}
# Create 3 vectors a, b, and q:
a <- c(1:10)
b <- 30:21 # c can be omited in consecutive assignment
q <- c("USYD", "UNSW", "USYD", "UWS", "UNSW", "USYD", "UWS", "WOL", "WOL", "NEW")
```

Combine these vectors to form a **data frame**:
``` {r}
# Combine vectors
theDF <- data.frame(a, b, q)
theDF

# Check the structure of data frame
str(theDF)
```

### *Summarize data*
```{r}
levels(theDF$q)
summary(theDF$q)
summary(theDF)
describe(theDF) # Need "psych" packages
```

## 2. Using *tibble* to create data frame

``` {r}
# Need "tidyverse" package
altTB <- tibble(
  a = c(1:10),
  b = 30:21, # c can be omited in consecutive assignment
  q = c("USYD", "UNSW", "USYD", "UWS", "UNSW", "USYD", "UWS", "WOL", "WOL", "NEW"),
)
altTB
```

### *Can convert df into tibble*
```{r}
# * Convert df to tibble ----
theDFc <- as_tibble(theDF)
theDFc
```


# B. Modifying the dataframe

## 1. Renaming columns/variables
```{r}
# * re-naming columns/variables ----
theDFc <- rename(theDFc, first = a, second = b, uni = q)
theDFc
```

## 2. Adding variables
```{r}
# * Adding another variable to dataframe ----
theDFc$new <- rep(c(1,2),5)
theDFc
```

```{r}
# Adding a new calculated variable based on existing variables
theDFc$cal <- theDFc$first * theDFc$second
theDFc
```

### *Using pipes*
```{r}
# ** Use pipes ----
theDFc <- theDFc %>%
  mutate(sq = first^2)
theDFc
```


# C. DISPLAYING SPECIFIC DATA IN A DATA FRAME

## 1. Subsetting 

``` {r}
theDFc[3,2]  # display [row, column]
theDFc[c(3, 5), 2:3]  # [row 3 & 5, columns 2 to 3]
theDFc[3, ] # All of row 3
theDFc[2:6,]

theDFc[,3] # display all rows of column 3
theDFc[[3]]
theDFc[["uni"]]
theDFc[,2:3]
theDFc[, c("first", "uni")]
```

### *Using Piping to subset*

#### **Filter** *for subsetting row*
``` {r}
# Require "tidyverse" package
theDFc %>% 
  filter(uni=="UNSW")
```

``` {r}
# AND operator
theDFc %>% 
  filter(first > 5 & second < 24)
```

``` {r}
# OR operator
theDFc %>% 
  filter(first > 5 | second > 28) %>%
  filter(uni == "USYD" | uni == "UWS")
```

#### **Select** *for subsetting solumn*
``` {r}
# Selecting coloumn
theDFc %>% 
  select(first, uni)
```

#### *Subsetting based on both row & column*
```{r}
theDFc %>% 
  select(first, uni) %>% 
  filter(first > 5) %>% 
  filter(uni == "WOL")
```

# D. SAVING & LOADING
```
save(theDFc, file = "theDFex.rda") # *.rda is an R data file
rm(theDFc)
theDFc
load("theDFex.rda")  # Note "theDFex.rda" is the filename, however the DF is still "theDFc"
theDFex
theDFc
```

# E. CREATING A MATRIX

Note how the numbers are arranged by default:
``` {r}
# Create a 2 x 4 matrix, arranged by column ----
A <-  matrix(c(1:8), nrow = 2)
A

# Define row and column names
rownames(A) = c("survived", "dead") # Naming colomns and rows
colnames(A) = c("control", "cyanide", "juice", "coffee")
A

# Define number of rows and column
B <-  matrix(c(1,2,3,4,5,6), nrow = 2, ncol = 3)
B

# Arrange by row first
C <-  matrix(c(1,2,3,4,5,6), nrow = 2, ncol = 3, byrow = TRUE)
C
```

``` {r}
# Create a matrix and add row and column names at the same time
D <- matrix(c(2, 10, 15, 3), nrow = 2,
            dimnames = list(Treatment =c("Control", "Intervention"),
                            Outcome = c("Survied", "Dead")))
D
```

## Simple Strength of association test for tables
``` {r}
fisher.test(D, conf.int = TRUE)
chisq.test(D)
```

# F. EDIT DATA ====

```
edit(A)
A <- edit(A)
fix(A)
```

# G. EXERCISE: Create a 2 x 2 matrix with proper headings using the following information ====
    Placebo, survived = 24
    Placebo, dead = 27
    Bigmac, survived = 3
    Bigmac, dead = 63
## Answer: ----

## Is Bigmac consumption associated with higher mortality risk?  

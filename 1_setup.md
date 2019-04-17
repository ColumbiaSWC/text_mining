---
title: "Text Mining Setup"
author: "Michael Weisner"
date: "February 14, 2019"
output: ''
layout: base
---

This tutorial was based on the Fall 2018 Data Mining course at Columbia University lead by Ben Goodrich.

# Packages

Packages for this tutorial include

* tidyverse
* tidytext
* dplyr
* gutenbergr
* janeaustenr
* wordcloud
* tm
* topicmodels
* text2vec
* ggplot2

## Package Installation (Optional)

```python
import random

x = 10
random.randint(1,10)
```

```R
# install.packages(c("tidyverse", "dplyr", "ggraph", "tidytext", "gutenbergr", "janeaustenr", "tm", "wordcloud", "topicmodels", "text2vec", "ggplot2", "quanteda"))
```

## Basic Packages to Load
```{r basic packages}
library(tidyverse)
library(dplyr)
library(ggplot2)
```


# Tidy Text

In general, a "tidy" `data.frame`, which is what we will use for R text analysis, has

* One "observation" per row
* Each column is a variable
* Each type of observational unit can be represented as a table

Which can be seen in the images below:

![Source: https://r4ds.had.co.nz/tidy-data.html](tidy-1.png)

When applied to text data, "tidy" means a table with one "token" per row, where a "token" can be a single word or set of adjacent words. The main strength of this approach is that it integrates well with the rest of the packages in the **tidyverse**.

Non-tidy approaches (that can be made tidy) to text include

* Character vectors
* Corpora with additional metadata
* Document-term matrices


## Ways to Break Up Text Data
The `unnest_tokens` function in the **tidytext** package can parse words, sentences, paragraphs, and more into tokens, in which is removes punctuation and converts to lowercase.

### Tidytext Examples:

Fundamentally you can break up your units of textual analysis by words, word pairs, sentences, paragraphs, chapters, etc.
```{r}
library(tidytext)
example(unnest_tokens)
```

# Preparing Tidy Text Data
## Stop Words

### Joins and Anti Joins

The **tidyverse** also uses some database-style logic in order to merge `data.frame`s together. 

* `left_join` returns all rows of the first `data.frame` when merged with another `data.frame`.
* `inner_join` is like a `left_join` but only keeps rows that match between the two `data.frame`s according to the columns in `by` that define a key. 
* `outer_join` keeps all the rows that appear in either of the two `data.frame`s. 
* `anti_join` drops all the observations from the first `data.frame` that match with the second `data.frame`.

To eliminate stop words we can do so with an `anti_join`:

```{r}
library(gutenbergr)
hgwells <- gutenberg_download(c(35, 36, 5230, 159)) # Books by H.G. Wells

tidy_hgwells_stop <- hgwells %>%
  unnest_tokens(word, text)
tidy_hgwells_stop
```


### Remove Stopwords
```{r}
tidy_hgwells <- tidy_hgwells_stop %>% 
  anti_join(stop_words)
tidy_hgwells
```


### Sort By Frequency
```{r}
tidy_hgwells %>%
  count(word, sort = TRUE)
```


# Sentiment Analysis in Text

Text may have a sentiment that is easy for a human to pick up on but the sentiment of individual words is subject to negation, context, sarcasm, and other linguistic problems.

Still, there are efforts to allow for analysis of the sentiment of text. The **tidytext** package includes a `data.frame` call `sentiments`

```R
sentiments
table(sentiments$score)
```

These are scored by three different sets of researchers. We can then `left_join` a tidy `data.frame` of texts with the `sentiments` `data.frame` to investigate whether the words used tend to be negative or positive, for example using the books writen by Jane Austen.








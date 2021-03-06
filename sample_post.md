---
title: "A slow start"
tags: [data science]
date: 2018-12-10
header:
  overlay_image: "/images/sheffield.png"
  overlay_filter: 0.5
excerpt: "Sample post, markdown"
mathjax: "true"
sidebar:
  title: "Sidebar"
  nav: sidebar-sample
toc: true
toc_label: "Unique Title"
---

# H1 Heading

## H2 Heading

### H3 Heading

Basic Text

text in *italics*

**bold** text

What about a [link](http://davidburn.gitub.io)

## Lists

### Heres a bulleted list:

* First item
+ Second item
- Third item

### Numbered list:

1. First
2. Second
3. Third

## Code

### Python code block:
```python
    import numpy as np

    def test_function(x,y):
        z = np.sum(x,y)
        return z
```

### R code block:
```r
library(tidyverse)
df <- read_csv("some_file.csv")
head(df)
```

Here's some inline code `x+y`.

Here's an image:
<img src="{{ site.url }}{{ site.baseurl }}/images/skyline.jpg" alt="Sheffield skyline">

Here's a kramdown image:
![alt]({{ site.url }}{{ site.baseurl }}/images/skyline.jpg)

Here's some math:

$$z=x+y$$

You can also put it inline $$z=x+y$$




---
layout: post
title:  "Jupyter Notebook with Stata"
date:   2021-12-11 14:32:04 +0700
categories: [STATA, Python, Jupyter Notebook]
---

Jupyter Notebook is a powerful and easy-to-use web application that allows you to combine executable code, visualizations, mathematical equations and formulas, narrative text, and other rich media in a single document (a "notebook") for interactive computing and developing. Jupyter notebooks have been widely used by researchers and scientists to share their ideas and results for collaboration and innovation.

Now, you can invoke Stata and Mata from Jupyter Notebooks with the IPython (interactive Python) kernel, meaning you can combine the capabilities of both Python and Stata in a single environment to make your work easily reproducible and shareable with others.

Let's see it work
In Jupyter Notebook, you can use two set of tools provided by the pystata Python package to interact with Stata:

Three IPython (interactive Python) magic commands: stata, mata, and pystata
A suite of API functions
Before showing you how to use these tools, we configure the pystata package. Suppose you have Stata installed in C:\Program Files\Stata17\ and you use the Stata/MP edition. Stata can be initialized as follows:


```bash
import stata_setup
stata_setup.config("C:/Program Files/Stata17", "mp")
```

If you get output similar to what is shown above for your edition of Stata, it means that everything is configured properly; see Configuration for more ways to configure pystata.


Call Stata using magic commands

The stata magic is used to execute Stata commands in an IPython environment. In a notebook cell, we put Stata commands underneath the %%stata cell magic to direct the cell to call Stata. The following commands load the auto dataset and summarize the mpg variable. The Stata output is displayed underneath the cell.


```bash
%%stata
sysuse auto, clear
summarize mpg
```

Stata's graphs can also be displayed in the IPython environment. Here we create a scatterplot of car mileage against price by using the %stata line magic.

```
%stata scatter mpg price
```

---
layout: post
title:  "New features in the Do-file Editor"
date:   2021-12-12 19:52:00 +0700
categories: [stata, do]
---
Syntax highlighting for Java and XML and autocompletion of quotes, parentheses, and brackets around a selection are now available in Stata's Do-file Editor.

Bookmarks can now be declared by the special bookmark comment **#. 

You can use the GUI to add or remove a bookmark by clicking in the bookmark margin of the current do-file. Stata will insert a line with the bookmark comment and display a bookmark icon in the margin. Or you can simply type the bookmark comment in your do-file. Adding text to a line that contains a bookmark will add a label to the bookmark. Because a bookmark is simply a special kind of comment, bookmarks are now persistent and can be saved with your do-file.

Stata's Do-file Editor now includes a Navigation Control that lists all the bookmarks in a file. Selecting a bookmark from the Navigation Control moves the Do-file Editor to the line that contains the bookmark. Remember how you can label a bookmark by adding text after a bookmark comment? The label for the bookmark is displayed in the Navigation Control to identify your bookmarks. In addition to bookmarks, the Navigation Control will also display all programs within a do-file, and you can use it to go directly to a program's definition.


## Let's see it work
We have a do-file that performs some data management and statistics, as well as creates a graph. Although the do-file has been organized into sections and thoroughly commented, it can still be difficult to find the section we're interested in, especially if the do-file is long. So we add a bookmark to the first line of each section of our do-file and label the bookmark as to the purpose of each section. Now if we want to display more summary statistics to our do-file, we can simply click on the Navigation Control and select the bookmark for Summary statistics. Because our do-file also contains a program, the program is automatically added to the Navigation Control, and we can use it to go directly to the program as well.

![nhanes](https://www.stata.com/new-in-stata/do-file-editor/img/nhanes.png)

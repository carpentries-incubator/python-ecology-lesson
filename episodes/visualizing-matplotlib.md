---
title: Data visualization with Pandas and Matplotlib
teaching: 90
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objectives

- Import the pyplot toolbox to create figures in Python.
- Use matplotlib to make adjustments to Pandas or plotnine objects.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- What other tools can I use to create plots apart from ggplot?
- Why should I use Python to create plots?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Loading libraries

A great feature in Python is the ability to import **libraries** to extend its capabilities.
For now, we'll focus on two of the most widely used libraries for data analysis: pandas and matplotlib.
We'll be using `pandas` for data wrangling and manipulation, and `matplotlib` for (you guessed it) making plots.

To be able to use these libraries in our code, we have to **install** and **import** them.
Installation is needed as pandas and matplotlib are third-party libraries that aren't built into Python.
You should have gone through the installation process during the setup for the workshop (if not, visit the [setup page](../index.md)), so we'll jump to the show you how to import libraries.

You only have to install a given library once in your computer, but you need to import it in each of your notebooks or scripts, as Python doesn't load all installed libraries by default.
To import a library, we use the syntax `import libraryName`.
If we want to give the library a nickname to shorten the command each time we call it, we can add `as nickNameHere`.
Here is how you'd import pandas and matplotlib using the common nicknames `pd` and `plt`, respectively.

```python
import pandas as pd
import matplotlib.pyplot as plt
```

:::::::::::::::::::::::::::::::::::::::::: spoiler

### Possible questions and errors with the import command
If you got an error similar to `ModuleNotFoundError: No module named '___'`, check that you have installed the library and you typed it correctly.

If you're asking yourself why we used `matplotlib.pyplot` instead of just `matplotlib`, good question!
We are not importing the entire matplotlib library, we are only importing the fraction of it called pyplot.
But we are just starting, we don't want to confuse you with more details.
::::::::::::::::::::::::::::::::::::::::::::::::::

## Loading and exploring data

For this lesson, we will be using the Portal Teaching data, a subset of the data
from Ernst et al.
[Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA](https://www.esapubs.org/archive/ecol/E090/118/default.htm).

We will be using files from the
[Portal Project Teaching Database](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459).
This section will use the `combined.csv` file that can be downloaded here:
[https://ndownloader.figshare.com/files/2292172](https://ndownloader.figshare.com/files/2292172)

We are studying the weight, hindfoot lenght, and sex of animals caught in sites of our study
area.
The data is stored as a `.csv` file: each row holds information for a single animal, and the columns represent:

| Column            | Description                                 |
| ----------------- | -----------------------------               |
| record\_id        | Unique id for the observation               |
| month             | month of observation                        |
| day               | day of observation                          | 
| year              | year of observation                         | 
| plot\_id          | ID of a particular site                     | 
| species\_id       | 2-letter code                               | 
| sex               | sex of animal ("M", "F")                    | 
| hindfoot\_length  | length of the hindfoot in mm                | 
| weight            | weight of the animal in grams               | 
| genus             | the genus of the species                    |
| species           | the latin species name                      |
| taxa              | general taxonomic category                  |
| plot_type         | type of experimental manipulation conducted |

We'll load the data in the csv into Python and name this new object `complete_old`.
For this, we can use the `pandas` library and its `.read_csv()` function, like shown here:

```python
complete_old = pd.read_csv("combined.csv")
```

Here we have created a new object that we can reference later in our code.
All objects in Python have a `type`, which determine what is possible to do with them.
To know the type of an object, we can use the `type()` function.

```python
type(complete_old)
```
```output
pandas.core.frame.DataFrame
```

From this we can read that `complete_old` is an object of Pandas DataFrame type.
We'll explore in depth what a DataFrame is in the next episode.
For now, we only need to keep in mind that our data is now contained in a DataFrame.
And this is important as the methods we'll cover now -`.head()`, `.info()`, and `.plot()`-
are only available to DataFrame objects.

:::::::::::::::::::::::::::::::::::::::::  callout

## TODO: Add here a little explanation on functions and methods?

Thinking about where to introduce functions and methods.
I realize this can be a lot, not beneficial for cognitive load.
But how can a novice understand the syntax on when you do library.function() and when you need object.method() or object.property ?

::::::::::::::::::::::::::::::::::::::::::::::::::

Now that our data is in Python, we can start working with it.
A good place to start is taking a look at the data.
With the `.head()` method, we can see the first five rows of the data set.

```python
complete_old.head()
```

```output
	record_id	month	day	year	plot_id	species_id	sex	hindfoot_length	weight	genus	species	taxa	plot_type
0	1	7	16	1977	2	NL	M	32.0	NaN	Neotoma	albigula	Rodent	Control
1	72	8	19	1977	2	NL	M	31.0	NaN	Neotoma	albigula	Rodent	Control
2	224	9	13	1977	2	NL	NaN	NaN	NaN	Neotoma	albigula	Rodent	Control
3	266	10	16	1977	2	NL	NaN	NaN	NaN	Neotoma	albigula	Rodent	Control
4	349	11	12	1977	2	NL	NaN	NaN	NaN	Neotoma	albigula	Rodent	Control
```

Another useful method to get a glimpse of the data is `.info()`.
This tells us how many rows the data set has (# of entries),  the number of columns, and for each of the columns it says its name, the number of non-null (or non-empty) rows it contains, and its data type.

```python
complete_old.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 34786 entries, 0 to 34785
Data columns (total 13 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   record_id        34786 non-null  int64  
 1   month            34786 non-null  int64  
 2   day              34786 non-null  int64  
 3   year             34786 non-null  int64  
 4   plot_id          34786 non-null  int64  
 5   species_id       34786 non-null  object 
 6   sex              33038 non-null  object 
 7   hindfoot_length  31438 non-null  float64
 8   weight           32283 non-null  float64
 9   genus            34786 non-null  object 
 10  species          34786 non-null  object 
 11  taxa             34786 non-null  object 
 12  plot_type        34786 non-null  object 
dtypes: float64(2), int64(5), object(6)
memory usage: 3.5+ MB
```

## Basic plotting with Pandas

Pandas makes it simple to start plotting your data, using the `.plot()` function.
In order to tell it what data to use, we need to specify the data argument.
An **argument** is an input that a function takes, and you set arguments using the `=` sign.
As arguments in this function, we add the kind of plot we want, and how our data is mapped in the plot.

With the following code, we will make a scatter plot (argument `kind = "scatter"`) to analyze the relationship between the weight (which will plot in the x axis, argument `x = "weight"`) and the hindfoot length (in the y axis, argument `y = "hindfoot_length"`) of the animals sampled at the study site.

```python
scatter_plot = complete_old.plot(x = "weight", y = "hindfoot_length", kind = "scatter")
```

This scatter plot shows there seems to be a positive relation between weight and hindfoot length, were heavier animal tend to have bigger hindfoots.

Similarly, we could make a [box plot](https://en.wikipedia.org/wiki/Box_plot) to explore the distribution of the hindfoot length across all samples.
In this case, Pandas wants us to use different arguments.
We'll use the `column` argument to specify what is the column we want to analyze.

```python
box_plot = complete_old.plot(column = "hindfoot_length", kind = "box")
```

The boxplot shows the median hindfoot length is around 32mm (represented by the line inside the box) and most values lie between 20 and 35 mm (which are the borders of the box, representing the 1st and 3rd quartile of the data, respectively).

We could further expand this analysis, and see the distribution of this variable across different plot types.
We can add an additional `by` argument, saying by which variable we want do disaggregate the box plot.

```python
box_plot_type = complete_old.plot(column = "hindfoot_length", by = "plot_type", kind = "box")
```

As shown in the previous image, the x-axis labels overlap with each other, which makes them unreadable.
At this point we realize a more fine-grained control over our graph is needed, and here is where Matplotlib shows in the picture.




## Advanced plots with Matplotlib

[Matplotlib](https://matplotlib.org/) is a Python library that is widely used throughout the scientific Python community to create high-quality and publication-ready graphics.
It supports a wide range of raster and vector graphics formats including PNG, PostScript, EPS, PDF and SVG.

Moreover, matplotlib is the actual engine behind the plotting capabilities of Pandas, and other plotting libraries like seaborn and plotnine.
For example, when we call the `.plot()` methods on Pandas data objects, we are actually using the matplotlib library in the backstage.

Let's start by recreating our scatter plot, but this time, using matplotlib.
We'll do it one step at a time.
The first thing we need is to create our figure and our axes (or plots), using the `.subplots()` function.
The `fig` object we are creating is the entire plot area, which can contain one or multiple axes.
In this case, we will have only one set of axes, which is the `ax` object.
Only this line of code will result in an empty plot.
We show our plot with the `plt.show()` function

```python
fig, ax = plt.subplots()
plt.show()
```

If we want more than one plot in the same figure, we could specify the number of rows (`nrows` argument) and the number of columns (`ncols`) in this function.
For example, let's say we want two plots (or axes), organized in two columns and one row.

```python
fig, ax = plt.subplots(nrows = 1, ncols =2)
plt.show()
```

Let's focus for now only in making our scatter plot, so just one set of axes.
For this, in our created `ax` axes, we'll modify it with the `.scatter()` function.
In the x axis, we'll use the `weight` column, so we use the argument `x = complete_old["weight"]`.
In the y axis, we'll use the `handfoot_length` column, so we use the argument `y = complete_old["hindfoot_length"]`.

```python
fig, ax = plt.subplots()
ax.scatter(x = complete_old["weight"], y = complete_old["hindfoot_length"])
plt.show()
```

We could further adjust our scatter plot, by changing the transparency of the points (`alpha` argument)

## Changing aesthetics
Making plots is often an iterative process, so we’ll continue developing the scatter plot we just made.
You may have noticed that parts of our scatter plot have many overlapping points, making it difficult to see all the data.
We can adjust the transparency of the points using the `alpha` argument, which takes a value between 0 and 1.
Additionally, we could change the color of the points, with the `c` argument.

```python
fig, ax = plt.subplots()
ax.scatter(x = complete_old["weight"], y = complete_old["hindfoot_length"], alpha = 0.2, c = "green")
plt.show()
```



## Alternative
All with pandas.

:::::::::::::::::::::::::::::::::::::::::  callout

## Tip: Saving figures in different formats

Matplotlib recognizes the extension used in the filename and
supports (on most computers) png, pdf, ps, eps and svg formats.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Saving figure to file

Check the documentation of the `savefig` method and check how
you can comply to journals requiring figures as `pdf` file with
dpi >= 300.

:::::::::::::::  solution

## Answers

```python
fig.savefig("my_plot_name.pdf", dpi=300)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Make other types of plots:

Matplotlib can make many other types of plots in much the same way that it makes two-dimensional line plots. Look through the examples in
[https://matplotlib.org/stable/plot_types/](https://matplotlib.org/stable/plot_types/) and try a few of them (click on the
"Source code" link and copy and paste into a new cell in Jupyter Notebook or
save as a text file with a `.py` extension and run in the command line).

:::::::::::::::::::::::::::::::::::::::  challenge

## Challenge - Final Plot

Display your data using one or more plot types from the example gallery. Which
ones to choose will depend on the content of your own data file. If you are
using the streamgage file [`bouldercreek_09_2013.txt`](data/bouldercreek_09_2013.txt), you could make a
histogram of the number of days with a given mean discharge, use bar plots
to display daily discharge statistics, or explore the different ways matplotlib
can handle dates and times for figures.


::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Matplotlib is the engine behind plotnine and Pandas plots.
- The object-based nature of matplotlib plots enables their detailed customization after they have been created.
- Export plots to a file using the `savefig` method.

::::::::::::::::::::::::::::::::::::::::::::::::::



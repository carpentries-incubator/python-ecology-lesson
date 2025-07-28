---
title: Exploring and understanding data
teaching: 45
exercises: 30
---

:::::::::::::::::::::::::::::::::::::: questions 

- Q1
- Q2
- Q3

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explore the structure and content of pandas dataframes
- Understand data types and missing data
- Use vectors as function arguments
- Create and convert factors
- Understand how Python assigns values to objects

::::::::::::::::::::::::::::::::::::::::::::::::

## The pandas `Dataframe`
We just spent quite a bit of time learning how to create visualisations from the `complete_old` data, but we did not talk much about what `complete_old` is.
You may remember that we loaded the data into Python with the `pandas.read_csv` function.
The output of `read_csv` is a _data frame_: a common way of representing tabular data in a programming language.
To be precise, `complete_old` is an _object_ of _type_ `DataFrame`.
In Python, pretty much everything you work with is an object of some type.
The `type` function can be used to tell you the type of any object you pass to it.

```python
type(complete_old)
```

```output
pandas.core.frame.DataFrame
```

This output tells us that the `DataFrame` object type is defined by pandas, i.e. it is a special type of object not included in the core functionality of Python.

### Exploring data in a dataframe
We can view the first few rows with the `head` _method_ and the last few rows with `tail`:

```python
complete_old.head()
```

```output
           month  day  year  plot_id  species_id  sex  hindfoot_length  weight
record_id 								
        1      7   16  1977        2          NL    M             32.0     NaN
        2      7   16  1977        3          NL    M             33.0     NaN
        3      7   16  1977        2          DM    F             37.0     NaN
        4      7   16  1977        7          DM    M             36.0     NaN
        5      7   16  1977        3          DM    M             35.0     NaN
```

```python
complete_old.tail()
```

```output
           month  day  year  plot_id  species_id  sex  hindfoot_length  weight
record_id 								
    35545     12   31  2002       15          AH  NaN              NaN     NaN
    35546     12   31  2002       15          AH  NaN              NaN     NaN
    35547     12   31  2002       10          RM    F             15.0    14.0
    35548     12   31  2002        7          DO    M             36.0    51.0
    35549     12   31  2002        5         NaN  NaN              NaN     NaN
```

These _methods_ are functions the pandas `DataFrame` object carries around with it, containing the instructions for actions that users commonly wish to take when working with tabular data.
In the case of `head` and `tail`, those actions were looking at the first and last few lines in the dataframe respectively but the objects carries with it many other methods too.
We saw the `plot` method in the previous epsiode.

As with the other functions we have encountered so far, `head` and `tail` can accept _arguments_ to modify their behaviour. 
For example, to adjust the number of lines shown.

```python
complete_old.head(n=10)
```

```output
           month  day  year  plot_id  species_id  sex  hindfoot_length  weight
record_id 								
        1      7   16  1977        2          NL    M             32.0     NaN
        2      7   16  1977        3          NL    M             33.0     NaN
        3      7   16  1977        2          DM    F             37.0     NaN
        4      7   16  1977        7          DM    M             36.0     NaN
        5      7   16  1977        3          DM    M             35.0     NaN
        6      7   16  1977        1          PF 	M     	      14.0     NaN
        7 	   7   16  1977        2          PE 	F 	           NaN     NaN
        8 	   7   16  1977        1          DM 	M 	          37.0     NaN
        9 	   7   16  1977        1          DM 	F 	          34.0     NaN
        10     7   16  1977        6          PF 	F 	          20.0     NaN
```

Note that you can also leave out the `n=` and specify the number of rows with an integer value only, e.g. `complete_old.tail(10)`.

`describe` is another useful method when exploring data:

```python
complete_old.describe()
```

```output
                month 	        day          year       plot_id  hindfoot_length        weight
count    35549.000000  35549.000000  35549.000000  35549.000000     31438.000000  32283.000000
mean         6.474022     16.105966   1990.475231     11.397001 	   29.287932     42.672428
std          3.396583      8.256691      7.493355      6.799406 	    9.564759     36.631259
min          1.000000      1.000000   1977.000000      1.000000 	    2.000000      4.000000
25%          4.000000      9.000000   1984.000000      5.000000 	   21.000000     20.000000
50%          6.000000     16.000000   1990.000000     11.000000 	   32.000000     37.000000
75%          9.000000     23.000000   1997.000000     17.000000 	   36.000000     48.000000
max         12.000000     31.000000   2002.000000     24.000000 	   70.000000    280.000000
```

While `info` provides an overview of the columns included in the dataframe:

```python
complete_old.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
Index: 35549 entries, 1 to 35549
Data columns (total 8 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   month            35549 non-null  int64  
 1   day              35549 non-null  int64  
 2   year             35549 non-null  int64  
 3   plot_id          35549 non-null  int64  
 4   species_id       34786 non-null  object 
 5   sex              33038 non-null  object 
 6   hindfoot_length  31438 non-null  float64
 7   weight           32283 non-null  float64
dtypes: float64(2), int64(4), object(2)
memory usage: 2.4+ MB
```

We get quite a bit of useful information here. 
First, we are told that we have a `DataFrame` of 35549 entries, or rows, and 8 variables, or columns.

Next, we get a bit of information on each variable, including its column title, a count of the _non-null_ values (that is, values that are not missing), and something called the `Dtype` of the column.

### Data types
The `Dtype` property of a dataframe column describes the _data type_ of the values stored in that column.
There are three in the example above: 

* `int64`: this column contains integer (whole number) values.
* `object`: this column contains string (non-numeric sequence of characters) values.
* `float64`: this column contains "floating point" values i.e. numeric values containing a decimal point.

The `64` after `int` and `float` represents the level of precision with which the values in the column are stored in the computer's memory.
Other types with lower levels of precision are available for numeric values, e.g. `int32` and `float16`, which will take up less memory on your system but limit the size and level of precision of the numbers they can store.

The `Dtype` of a column is important because it determines the kinds of operation that can be performed on the values in that column.
Let's work with a couple of the columns independently to demonstrate this.

### The `Series` object
To work with a single column of a dataframe, we can refer to it by name in two different ways:

```python
complete_old["species_id"]
```

or

```python
complete_old.species_id # this only works if there are no spaces in the column name (note the underscore used here)
```

```output
record_id
1          NL
2          NL
3          DM
4          DM
5          DM
         ... 
35545      AH
35546      AH
35547      RM
35548      DO
35549     NaN
Name: species_id, Length: 35549, dtype: object
```

:::::::::::::::::::::::::::::::::::::::::: callout

### Tip: use tab completion on column names
Tab completion, where you start typing the name of a variable, function, etc before hitting <kbd>Tab</kbd> to auto-complete the rest, also works on column names of a dataframe.
Since this tab completion saves time and reduces the chance of including typos, we recommend you use it as frequently as possible.

::::::::::::::::::::::::::::::::::::::::::::::::::

The result of that operation is a _series_ of data: a one-dimensional sequence of values that all have the same `Dtype` (`object` in this case).
Dataframe objects are collections of the series "glued together" with a shared _index_: the column of unique identifiers we associate with each row.
`record_id` is the index of the series summarised above; the values carried by the series are `NL`, `DM`, `AH`, etc (short species identification codes).

If we choose a different column of the dataframe, we get another series with a different data type:

```python
complete_old.weight
```

```output
record_id
1          NaN
2          NaN
3          NaN
4          NaN
5          NaN
         ... 
35545      NaN
35546      NaN
35547     14.0
35548     51.0
35549      NaN
Name: weight, Length: 35549, dtype: float64
```

The data type of the series influences the things that can be done with/to it.
For example, sorting works differently for these two series, with the numeric values in the `weight` series sorted from largest to smallest and the character strings in `species_id` sorted alphabetically:

```python
complete_old.weight.sort_values()
```

```output
record_id
9909       4.0
4290       4.0
9794       4.0
9790       4.0
5346       4.0
         ... 
35531      NaN
35544      NaN
35545      NaN
35546      NaN
35549      NaN
Name: weight, Length: 35549, dtype: float64
```

```python
complete_old.species_id.sort_values()
```

```output
record_id
28959     AB
12211     AB
5256      AB
14055     AB
14054     AB
        ... 
34757    NaN
34970    NaN
35188    NaN
35385    NaN
35549    NaN
Name: species_id, Length: 35549, dtype: object
```

This pattern of behaviour, where the type of an object determines what can be done with it and influences how it is done, is a defining characteristic of Python.
As you gain more experience with the language, you will become more familiar with this way of working with data.
For now, as you begin on your learning journey with the language, we recommend using the `type` function frequently to make sure that you know what kind of data/object you are working with, and do not be afraid to ask for help whenever you are unsure or encounter a problem.

## Aside: Getting Help


## `Series` and data types

## Missing data

## Assignment, objects, and values

::::::::::::::::::::::::::::::::::::: keypoints 

- KP1
- KP2
- KP3

::::::::::::::::::::::::::::::::::::::::::::::::

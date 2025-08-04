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
As we learned previously, the first few rows of a dataframe can be viewed with the `head` _method_. 

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

Similarly, the last few rows can be viewed with `tail`.

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

These methods are functions that the pandas `DataFrame` object carries around with it, containing the instructions for actions that users commonly wish to take when working with tabular data.
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

Next, we get a bit of information on each variable, including its column title, a count of the _non-null_ values (that is, values that are not missing), and something called the `dtype` of the column.

### Data types
The `Dtype` property of a dataframe column describes the _data type_ of the values stored in that column.
There are three in the example above: 

* `int64`: this column contains integer (whole number) values.
* `object`: this column contains string (non-numeric sequence of characters) values.
* `float64`: this column contains "floating point" values i.e. numeric values containing a decimal point.

The `64` after `int` and `float` represents the level of precision with which the values in the column are stored in the computer's memory.
Other types with lower levels of precision are available for numeric values, e.g. `int32` and `float16`, which will take up less memory on your system but limit the size and level of precision of the numbers they can store.

The `Ddype` of a column is important because it determines the kinds of operation that can be performed on the values in that column.
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

The result of that operation is a _series_ of data: a one-dimensional sequence of values that all have the same `dtype` (`object` in this case).
Dataframe objects are collections of the series "glued together" with a shared _index_: the column of unique identifiers we associate with each row.
`record_id` is the index of the series summarised above; the values carried by the series are `NL`, `DM`, `AH`, etc (short species identification codes).

If we choose a different column of the dataframe, we get another series with a different data type:

```python
complete_old['weight']
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
complete_old['weight'].sort_values()
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
complete_old['species_id'].sort_values()
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
* Searching for help
* Generative AI
* Anatomy of an error message

## Unexpected data types
Series objects can be created directly within Python.
But before we can do that, we need to learn about another type of data in Python: the list.

```python
years = pd.Series([2010, 2025, 2019, 2020, 2025])
years
```

```output
0    2010
1    2025
2    2019
3    2020
4    2025
dtype: int64
```

The `dtype: int64` was determined automatically based on the values passed in.
But what if the values provided are of several different types?

```python
ages = pd.Series([2, 3, 5.5, 6, 8])
ages
```

```output
0    2.0
1    3.0
2    5.5
3    6.0
4    8.0
dtype: float64
```

Pandas assigns a `dtype` that allows it to account for all of the values it is given, converting some values to another `dtype` if needed, in a process called _coercion_.
In the case above, all of the integer values were coerced to floating point numbers to account for the `5.5`.

:::::::::::::::::::::::::::::::::::::::: challenge

### Exercise: Coercion
Can you guess the `dtype` of each series created below?
Run the code to check whether you were right.

```python
int_str = pd.Series([1, "two", 3])
str_flt = pd.Series(["four", 5.0, "six"])
```

::::::::::::::::::::: solution

```python
int_str
```

```output
0      1
1    two
2      3
dtype: object
```

```python
str_flt
```

```output
0   four
1    5.0
2    six
dtype: object
```

In both cases, the numeric values are coerced into strings.
When automatically coercing values between types like this, Python aims to minimise the amount of information lost.

::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

In practice, it is much more common to read data from elsewhere (e.g. with `read_csv`) than to enter it manually within Python.
When reading data from a file, pandas tries to guess the appropriate `dtype` to assign to each column (series) of the dataframe.
This is usually very helpful but the process is sensitive to inconsistencies and data entry errors in the input: a stray character in one cell can cause an entire column to be coerced to a different `dtype` than you might have wanted.

For example, if the raw data includes a symbol added by a typo mistake (`=` instead of `-`):

```csv
name,latitude,longitude
Superior,47.7,-87.5
Victoria,-1.0,33.0
Tanganyika,=6.0,29.3
```

We see a non-numeric `dtype` for the latitude column (`object`) when we load the data into a dataframe.

```python
lakes = pd.read_csv('lakes.csv')
lakes.info()
```

```output
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3 entries, 0 to 2
Data columns (total 3 columns):
 #   Column     Non-Null Count  Dtype  
---  ------     --------------  -----  
 0   name       3 non-null      object 
 1   latitude   3 non-null      object 
 2   longitude  3 non-null      float64
dtypes: float64(1), object(2)
memory usage: 200.0+ bytes
```

It is a good idea to run the `info` method on a new dataframe after you have loaded data for the first time: if one or more of the columns has a different `dtype` than you expected, this may be a signal that you need to clean up the raw data.

### Recasting
A column can be manually coerced (or _recast_) into a different `dtype`, provided that pandas knows how to handle that conversion.
For example, the integer values in the `plot_id` column of our dataframe can be converted to floating point numbers:

```python
complete_old['plot_id'] = complete_old['plot_id'].astype('float')
complete_old['plot_id']
```

```output
record_id
1         2.0
2         3.0
3         2.0
4         7.0
5         3.0
         ... 
35545    15.0
35546    15.0
35547    10.0
35548     7.0
35549     5.0
Name: plot_id, Length: 35549, dtype: float64
```

But the string values of `species_id` cannot be converted to numeric data:

```python
complete_old.species_id = complete_old.species_id.astype('int')
```

```error
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
Cell In[101], line 1
----> 1 complete_old.species_id = complete_old.species_id.astype('int64')

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/generic.py:6662, in NDFrame.astype(self, dtype, copy, errors)
   6656     results = [
   6657         ser.astype(dtype, copy=copy, errors=errors) for _, ser in self.items()
   6658     ]
   6660 else:
   6661     # else, only a single dtype is given
-> 6662     new_data = self._mgr.astype(dtype=dtype, copy=copy, errors=errors)
   6663     res = self._constructor_from_mgr(new_data, axes=new_data.axes)
   6664     return res.__finalize__(self, method="astype")

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/internals/managers.py:430, in BaseBlockManager.astype(self, dtype, copy, errors)
    427 elif using_copy_on_write():
    428     copy = False
--> 430 return self.apply(
    431     "astype",
    432     dtype=dtype,
    433     copy=copy,
    434     errors=errors,
    435     using_cow=using_copy_on_write(),
    436 )

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/internals/managers.py:363, in BaseBlockManager.apply(self, f, align_keys, **kwargs)
    361         applied = b.apply(f, **kwargs)
    362     else:
--> 363         applied = getattr(b, f)(**kwargs)
    364     result_blocks = extend_blocks(applied, result_blocks)
    366 out = type(self).from_blocks(result_blocks, self.axes)

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/internals/blocks.py:784, in Block.astype(self, dtype, copy, errors, using_cow, squeeze)
    781         raise ValueError("Can not squeeze with more than one column.")
    782     values = values[0, :]  # type: ignore[call-overload]
--> 784 new_values = astype_array_safe(values, dtype, copy=copy, errors=errors)
    786 new_values = maybe_coerce_values(new_values)
    788 refs = None

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/dtypes/astype.py:237, in astype_array_safe(values, dtype, copy, errors)
    234     dtype = dtype.numpy_dtype
    236 try:
--> 237     new_values = astype_array(values, dtype, copy=copy)
    238 except (ValueError, TypeError):
    239     # e.g. _astype_nansafe can fail on object-dtype of strings
    240     #  trying to convert to float
    241     if errors == "ignore":

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/dtypes/astype.py:182, in astype_array(values, dtype, copy)
    179     values = values.astype(dtype, copy=copy)
    181 else:
--> 182     values = _astype_nansafe(values, dtype, copy=copy)
    184 # in pandas we don't store numpy str dtypes, so convert to object
    185 if isinstance(dtype, np.dtype) and issubclass(values.dtype.type, str):

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/dtypes/astype.py:133, in _astype_nansafe(arr, dtype, copy, skipna)
    129     raise ValueError(msg)
    131 if copy or arr.dtype == object or dtype == object:
    132     # Explicit copy, or required since NumPy can't view from / to object.
--> 133     return arr.astype(dtype, copy=True)
    135 return arr.astype(dtype, copy=copy)

ValueError: invalid literal for int() with base 10: 'NL'
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Changing Types

1. Convert the values in the column `plot_id` back to integers.
2. Now try converting `weight` to an integer. 
   What goes wrong here?
   What is pandas telling you?
   We will talk about some solutions to this later.

::::::::::::::::::::::: solution

```python
surveys_df['plot_id'].astype("int")
```

```output
record_id
1         2
2         3
3         2
4         7
5         3
         ..
35545    15
35546    15
35547    10
35548     7
35549     5
Name: plot_id, Length: 35549, dtype: int64
```

```python
surveys_df['weight'].astype("int")
```

```error
pandas.errors.IntCastingNaNError: Cannot convert non-finite values (NA or inf) to integer
```

Pandas cannot convert types from float to int if the column contains missing values.

::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Missing Data
In addition to data entry errors, it is common to encounter missing values in large volumns of data.
A value may be missing because it was not possible to make a complete observation, because data was lost, or for any number of other reasons.
It is important to consider missing values while processing data because they can influence downstream analysis -- that is, data analysis that will be done later -- in unwanted ways when not handled correctly.

Depending on the `dtype` of the column/series, missing values may appear as `NaN` (_"Not a Number"_), `NA`, `<NA>`, or `NaT` (_"Not a Time"_).
You may have noticed some during our initial exploration of the dataframe.
(Note the `NaN` values in the first five rows of the `weight` column below.)

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

The output of the `info` method includes a count of the _non-null_ values -- that is, the values that are _not_ missing -- for each column:


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

This shows that pandas can distinguish these `NaN` values from the actual data and indeed they will be ignored for some tasks, such as calculation of the summary statistics provided by `describe`.

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

In some circumstances, like the recasting we attempted in the previous exercise, the missing values can cause trouble.
It is up to us to decide how best to handle those missing values.
We could remove the rows containing missing data, accepting the loss of all data for that observation:

```python
complete_old.dropna().head()
```

```output
           month  day  year  plot_id species_id sex  hindfoot_length  weight
record_id                                                                   
63             8   19  1977        3         DM   M             35.0    40.0
64             8   19  1977        7         DM   M             37.0    48.0
65             8   19  1977        4         DM   F             34.0    29.0
66             8   19  1977        4         DM   F             35.0    46.0
67             8   19  1977        7         DM   M             35.0    36.0
```

But we should take note that this removes almost 5000 rows from the dataframe!

```python
len(complete_old)
```

```output
35549
```

```python
len(complete_old.dropna())
```

```output
30676
```

Instead, we could _fill_ all of the missing values with something else.
For example, let's make a copy of the `complete_old` dataframe then populate the missing values in the `weight` column of that copy with the mean of all the non-missing weights.
There are a few parts to that operation, which are tackled one at a time below.

```python
df1 = complete_old.copy() # making a copy to work with so that we do not edit our original data
mean_weight = complete_old['weight'].mean() # the 'mean' method calculates the mean of the non-null values in the column
df1['weight'] = complete_old['weight'].fillna(mean_weight) # the 'fillna' method fills all missing values with the provided value
```

The choice to fill in these missing values rather than removing the rows that contain them can have implications for the result of your analysis.
It is important to consider your approach carefully.
Think about how the data will be used and how these values will impact the scientific conclusions made from the analysis.
pandas gives us all of the tools that we need to account for these issues.
But we need to be cautious about how the decisions that we make impact scientific results.


## Assignment, evaluation, and mutability
Stepping away from dataframes for a moment, the time has come to explore the behaviour of Python a little more.

:::::::::::::::::::::::::::::::::::::::: challenge

### Exercise: variable assignments
What is the value of `y` after running the following lines?

```python
x = 5
y = x
x = 10
```

::::::::::::::::::::: solution

```python
x = 2
y = x*3
x = 10
y
```

```output
6
```

::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Understanding what’s going on here will help you avoid a lot of confusion when working in Python.
When we assign something to a variable, the first thing that happens is the righthand side gets _evaluated_.
So when we first ran the line `y = x*3`, `x*3` first gets evaluated to the value of 6, and this gets assigned to `y`.
The variables `x` and `y` are independent objects, so when we change the value of `x` to 10, `y` is unaffected.
This behaviour may be different to what you are used to, e.g. from experience working with data in spreadsheets where cells can be linked such that modifying the value in one cell triggers changes in others.

Multiple evaluations can take place in a single line of Python code and learning to trace the order and impact of these evaluations is a key skill.

```python
x = 10
y = 5
z = 3-(x/y)
z
```

```output
1.0
```

In the example above, `x/y` is evaluated first before the result is subtracted from 3 and the final calculated value is assigned to `z`.
(The brackets `()` are not needed in the calculation above but are included to make the order of evaluation clearer.)
Python makes each evaluation as it needs to in order to proceed with the next, before assigning the final result to the variable on the lefthand side of the `=` operator.

This means that we could have filled the missing values in the weight column of our dataframe copy in a single line:

```python
df1["weight"] = complete_old['weight'].fillna(complete_old["weight"].mean())
```

First, the mean weight is calculated (`complete_old["weight"].mean()` is evaluated).
Then the result of that evaluation is passed into `fillna` and the result of the filling operation (`complete_old['weight'].fillna(<RESULT OF PREVIOUS>)`) is assigned to `df1["weight"]`.

#### Variable naming
You are going to name a lot of variables in Python!
There are some rules you ahve to stick to when doing so, as well as recommendations that will make your life easier.

* Make names clear without being too long
    * `wkg` is probably too short.
    * `weight_in_kilograms` is probably too long.
    * `weight_kg` is good.
* Names cannot begin with a number.
* Names cannot contain spaces; use underscores instead.
* Names are case sensitive: `weight_kg` is a different name from `Weight_kg`.
  Avoid uppercase characters at the beginning of variable names.
* Names cannot contain most non-letter characters: `+&-/*.` etc.
* Two common formats of variable name are `snake_case` and `camelCase`. 
  A third "case" of naming convention, `kebab-case`, is not allowed in Python (see the rule above).
* Aim to be consistent in how you name things within your projects.
  It is easier to follow an established style guide, such as [Google's](https://google.github.io/styleguide/pyguide.html), than to come up with your own.

:::::::::::::::::::::::::::::::::::::::::: challenge

### Exercise; good variable names
Identify at least one good variable name and at least one variable name that could be improved in this episode.
Refer to the rules and recommendations listed above to suggest how these variable names could be better.

::::::::::::::::::::: solution

`mean_weight` and `surveys_df` are examples of reasonably good variable names: they are relatively short yet descriptive.

`df2` is not descriptive enough and could be potentially confusing if encountered by somebody else/ourselves in a few weeks' time.
The name could be improved by making it more descriptive, e.g. `surveys_df_duplicate`.

::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::::

#### Mutability
Why did we need to use the `copy` method to duplicate the dataframe above if variables are not linked to each other?
Why not assign a new variable with the value of the existing dataframe object?

```python
df2 = complete_old
```

This gets to _mutablity_: a feature of Python that has caused headaches for many novices in the past!
In the interests of memory management, Python avoids making copies of objects unless it has to.
Some types of objects are _immutable_, meaning that their value cannot be modified once set.
Immutable object types we have already encountered include strings, integers, and floats.
If we want to adjust the value of an integer variable, we must explicitly overwrite it.

Other types of object are _mutable_, meaning that their value can be changed "in-place" without needing to be explictly overwritten.
This includes lists **and pandas `DataFrame` objects**, which can be reordered etc. after they are created.

When a new variable is assigned the value of an existing **immutable** object, Python duplicates the value and assigns it to the new variable.

```
a = 3.5 # new float object, called "a"
b = a   # another new float object, called "b", which also has the value 3.5
```

When a new variable is assigned the value of an existing **mutable** object, Python makes a new "pointer" towards the value of the existing object instead of duplicating it.

```
some_species = ['NL', 'DM', 'PF', 'PE', 'DS'] # new list object, called "some_species"
some_more_species = some_species # another name for the same list object
```

This can have unintended consequences and lead to much confusion!

```
some_more_species[2] = "CV"
some_species
```

```output
['NL', 'DM', 'CV', 'PE', 'DS']
```

This takes practice and time to get used to.
The key thing to remember is that **you should use the `copy` method to make a copy of your dataframes** to avoid accidentally modifying the data in the original.

```python
df2 = complete_old.copy()
```


::::::::::::::::::::::::::::::::::::: keypoints 

- KP1
- KP2
- KP3

::::::::::::::::::::::::::::::::::::::::::::::::

---
title: Exploring and understanding data
teaching: 80
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I do exploratory data analysis in Python?
- How do I get help when I am stuck?
- What impact does an object's type have on what I can do with it?
- How are expressions evaluated and values assigned to variables?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explore the structure and content of pandas dataframes
- Convert data types and handle missing data
- Interpret error messages and develop strategies to get help with Python
- Trace how Python assigns values to objects

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
We encountered the `plot`, `head` and `tail` methods in the previous epsiode. 
Dataframe objects carry many other methods, including some that are useful when exploring a dataset for the first time.
Consider the output of `describe`:

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

These summary statistics give an immediate impression of the distribution of the data.
It is always worth performing an initial "sniff test" with these: if there are major issues with the data or its formatting, they may become apparent at this stage.

`info` provides an overview of the columns included in the dataframe:

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

We get quite a bit of useful information here too. 
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
We have already encountered several errors while following the lesson and this is a good time to take a step back and discuss good strategies to get help when something goes wrong.

### 1. The built-in `help` function
Use `help` to view documentation for an object or function.

```python
help(round)
```

```output
Help on built-in function round in module builtins:

round(number, ndigits=None)
    Round a number to a given precision in decimal digits.
    
    The return value is an integer if ndigits is omitted or None.  Otherwise
    the return value has the same type as the number.  ndigits may be negative.
```

### The Jupyter Notebook has two ways to get help.
If you are working in Jupyter (Notebook or Lab), the platform offers some additional ways to see documentation/get help:

* Option 1: Type the function name in a cell with a question mark after it, e.g. `round?`. 
  Then run the cell.
* Option 2: (Not available on all systems)
  Place the cursor near where the function is invoked in a cell
  (i.e., the function name or its parameters),
  - Hold down <kbd>Shift</kbd>, and press <kbd>Tab</kbd>.
  - Do this several times to expand the information returned.


### Understanding error messages
The error messages returned when something goes wrong can be (very) long but contain information about the problem, which can be very useful once you know how to interpret it.
For example, you might receive a `SyntaxError` if you mistyped a line and the resulting code was invalid:

```python
# Forgot to close the quote marks around the string.
name = 'Feng
```

```error
  Cell In[129], line 1
    name = 'Feng
           ^
SyntaxError: unterminated string literal (detected at line 1)
```

There are three parts to this error message:

```error
  Cell In[129], line 1
```

This tells us where the error occured.
This is of limited help in Jupyter, since we know that the error is in the cell we just ran (`Cell In[129]`), but the line number can be helpful especially when the cell is quite long.
But when running a larger program written in Python, perhaps built up from multiple individual scripts, this can be more useful, e.g.

```error
  data_visualisation.py, line 42
```

Next, we see a copy of the line where the error was encountered, often annotated with an arrow pointing out exactly where Python thinks the problem is:

```error
    name = 'Feng
           ^
```

Python is not _exactly_ right in this case: from context you might be able to guess that the issue is really the lack of a closing quotation mark at the end of the line.
But an arrow pointing to the opening quotation mark can give us a push in the right direction.
Sometimes Python gets these annotations exactly right. 
Occasionally, it gets them completely wrong.
In the vast majority of cases they are at least somewhat helpful.

Finally, we get the error message itself:

```error
SyntaxError: unterminated string literal (detected at line 1)
```

This always begins with a statement of the _type_ of error encountered: in this case, a `SyntaxError`.
That provides a broad categorisation for what went wrong.
The rest of the message is a description of exactly what the problem was from Python's perspective.
Error messages can be loaded with jargon and quite difficult to understand when you are first starting out.
In this example, `unterminated string literal` is a technical way of saying "you opened some quotes, which I think means you were trying to define a string value, but the quotes did not get closed before the end of the line."

It is normal not to understand exactly what these error messages mean the first time you encounter them.
Since programming involves making lots of mistakes (for everyone!), you will start to become familiar with many of them over time.
As you continue learning, we recommend that you ask others for help: more experienced programmers have made all of these mistakes before you and will probably be better at spotting what has gone wrong.
(More on asking for help below.)

#### Error output can get really long!
Especially when using functions from libraries you have imported into your program, the middle part of the error message (the _traceback_) can get rather long.
For example, what happens if we try to access a column that does not exist in our dataframe?

```python
complete_old["wegiht"] # misspelling the 'weight' column name
```

```error
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/indexes/base.py:3812, in Index.get_loc(self, key)
   3811 try:
-> 3812     return self._engine.get_loc(casted_key)
   3813 except KeyError as err:

File pandas/_libs/index.pyx:167, in pandas._libs.index.IndexEngine.get_loc()

File pandas/_libs/index.pyx:196, in pandas._libs.index.IndexEngine.get_loc()

File pandas/_libs/hashtable_class_helper.pxi:7088, in pandas._libs.hashtable.PyObjectHashTable.get_item()

File pandas/_libs/hashtable_class_helper.pxi:7096, in pandas._libs.hashtable.PyObjectHashTable.get_item()

KeyError: 'wegiht'

The above exception was the direct cause of the following exception:

KeyError                                  Traceback (most recent call last)
Cell In[131], line 1
----> 1 complete_old["wegiht"]

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/frame.py:4107, in DataFrame.__getitem__(self, key)
   4105 if self.columns.nlevels > 1:
   4106     return self._getitem_multilevel(key)
-> 4107 indexer = self.columns.get_loc(key)
   4108 if is_integer(indexer):
   4109     indexer = [indexer]

File ~/miniforge3/envs/carpentries/lib/python3.11/site-packages/pandas/core/indexes/base.py:3819, in Index.get_loc(self, key)
   3814     if isinstance(casted_key, slice) or (
   3815         isinstance(casted_key, abc.Iterable)
   3816         and any(isinstance(x, slice) for x in casted_key)
   3817     ):
   3818         raise InvalidIndexError(key)
-> 3819     raise KeyError(key) from err
   3820 except TypeError:
   3821     # If we have a listlike key, _check_indexing_error will raise
   3822     #  InvalidIndexError. Otherwise we fall through and re-raise
   3823     #  the TypeError.
   3824     self._check_indexing_error(key)

KeyError: 'wegiht'
```

(This is still relatively short compared to some errors messages we have seen!)

When you encounter a long error like this one, do not panic!
Our advice is to focus on the first couple of lines and the last couple of lines. 
Everything in the middle (as the name traceback suggests) is retracing steps through the program, identifying where problems were encountered along the way.
That information is only really useful to somebody interested in the inner workings of the pandas library, which is well beyond the scope of this lesson!
If we ignore everything in the middle, the parts of the error message we want to focus on are:

```error
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)

[... skipping these parts ...]

KeyError: 'wegiht'
```

This tells us that the problem is the "key": the value we used to lookup the column in the dataframe.
Hopefully, the repetition of the value we provided would be enough to help us realise our mistake.

### Other ways to get help
There are several other ways that people often get help when they are stuck with their Python code.

* Search the internet: 
  paste the last line of your error message or the word "python" and a short description of what you want to do into your favourite search engine 
  and you will usually find several examples where other people have encountered the same problem and came looking for help.
    * [StackOverflow](https://stackoverflow.com/questions) can be particularly helpful for this: answers to questions are presented as a ranked thread ordered according to how useful other users found them to be.
    * **Take care:** copying and pasting code written by somebody else is risky unless you understand exactly what it is doing!
* ask somebody "in the real world". 
  If you have a colleague or friend with more expertise in Python than you have, show them the problem you are having and ask them for help.
* Sometimes, the act of articulating your question can help you to identify what is going wrong.
  This is known as ["rubber duck debugging"](https://en.wikipedia.org/wiki/Rubber_duck_debugging) among programmers.

#### Generative AI

::::::::::::::::::::::::::::: instructor

### Choose how to teach this section
The section on generative AI is intended to be concise but Instructors may choose to devote more time to the topic in a workshop.
Depending on your own level of experience and comfort with talking about and using these tools, you could choose to do any of the following:

* Explain how large language models work and are trained, and/or the difference between generative AI, other forms of AI that currently exist, and the limits of what LLMs can do (e.g., they can't "reason").
* Demonstrate how you recommend that learners use generative AI.
* Discuss the ethical concerns listed below, as well as others that you are aware of, to help learners make an informed choice about whether or not to use generative AI tools.

This is a fast-moving technology. 
If you are preparing to teach this section and you feel it has become outdated, please open an issue on the lesson repository to let the Maintainers know and/or a pull request to suggest updates and improvements.

::::::::::::::::::::::::::::::::::::::::

It is increasingly common for people to use _generative AI_ chatbots such as ChatGPT to get help while coding. 
You will probably receive some useful guidance by presenting your error message to the chatbot and asking it what went wrong. 
However, the way this help is provided by the chatbot is different. 
Answers on StackOverflow have (probably) been given by a human as a direct response to the question asked. 
But generative AI chatbots, which are based on an advanced statistical model, respond by generating the _most likely_ sequence of text that would follow the prompt they are given.

While responses from generative AI tools can often be helpful, they are not always reliable. 
These tools sometimes generate plausible but incorrect or misleading information, so (just as with an answer found on the internet) it is essential to verify their accuracy.
You need the knowledge and skills to be able to understand these responses, to judge whether or not they are accurate, and to fix any errors in the code it offers you.

In addition to asking for help, programmers can use generative AI tools to generate code from scratch; extend, improve and reorganise existing code; translate code between programming languages; figure out what terms to use in a search of the internet; and more.
However, there are drawbacks that you should be aware of.

The models used by these tools have been "trained" on very large volumes of data, much of it taken from the internet, and the responses they produce reflect that training data, and may recapitulate its inaccuracies or biases.
The environmental costs (energy and water use) of LLMs are a lot higher than other technologies, both during development (known as training) and when an individual user uses one (also called inference). For more information see the [AI Environmental Impact Primer](https://huggingface.co/blog/sasha/ai-environment-primer) developed by researchers at HuggingFace, an AI hosting platform. 
Concerns also exist about the way the data for this training was obtained, with questions raised about whether the people developing the LLMs had permission to use it.
Other ethical concerns have also been raised, such as reports that workers were exploited during the training process.

**We recommend that you avoid getting help from generative AI during the workshop** for several reasons:

1. For most problems you will encounter at this stage, help and answers can be found among the first results returned by searching the internet.
2. The foundational knowledge and skills you will learn in this lesson by writing and fixing your own programs  are essential to be able to evaluate the correctness and safety of any code you receive from online help or a generative AI chatbot. 
   If you choose to use these tools in the future, the expertise you gain from learning and practising these fundamentals on your own will help you use them more effectively.
3. As you start out with programming, the mistakes you make will be the kinds that have also been made -- and overcome! -- by everybody else who learned to program before you. 
  Since these mistakes and the questions you are likely to have at this stage are common, they are also better represented than other, more specialised problems and tasks in the data that was used to train generative AI tools.
  This means that a generative AI chatbot is _more likely to produce accurate responses_ to questions that novices ask, which could give you a false impression of how reliable they will be when you are ready to do things that are more advanced.


## Data input within Python
Although it is more common (and faster) to input data in another format e.g. a spreadsheet and read it in, Series and DataFrame objects can be created directly within Python.
Before we can make a new Series, we need to learn about another type of data in Python: the list.

### Lists
Lists are one of the standard _data structures_ built into Python.
A data structure is an object that contains more than one piece of information.
(DataFrames and Series are also data structures.)
The list is designed to contain multiple values in an ordered sequence: they are a great choice if you want to build up and modify a collection of values over time and/or handle each of those values one at a time.
We can create a new list in Python by capturing the values we want it to store inside square brackets `[]`:

```python
years_list = [2020, 2025, 2010]
years_list
```

```output
[2020, 2025, 2010]
```

New values can be added to the end of a list with the `append` method:

```python
years_list.append(2015)
years_list
```

```output
[2020, 2025, 2010, 2015]
```

:::::::::::::::::::::::::::::::::::::::: challenge

### Exploring list methods
The `append` method allows us to add a value to the end of a list but how could we insert a new value into a given position instead?
Applying what you have learned about how to find out the methods that an object has, can you figure out how to place the value `2019` into the third position in `years_list`  (shifting the values after it up one more position)?
Recall that the indexing used to specify positions in a sequence begins at 0 in Python.

::::::::::::::::::::: solution

Using tab completion, the `help` function, or looking up the documentation online, we can discover the `insert` method and learn how it works.
`insert` takes two arguments: the position for the new list entry and the value to be placed in that position:

```python
years_list.insert(2, 2019)
years_list
```

```output
[2020, 2025, 2019, 2010, 2015]
```

::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Among many other methods is `sort`, which can be used to sort the values in the list:

```python
years_list.sort()
years_list
```

```output
[2010, 2015, 2019, 2020, 2025]
```

The easiest way to create a new Series is with a list:

```python
years_series = pd.Series(years_list)
years_series
```

```output
0    2010
1    2015
2    2019
3    2020
4    2025
dtype: int64
```

With the data in a Series, we can no longer do some of the things we were able to do with the list, such as adding new values.
But we do gain access to some new possibilities, which can be very helpful.
For example, if we wanted to increase all of the values by 1000, this would be easy with a Series but more complicated with a list:

```python
years_series + 1000
```

```output
0    3010
1    3015
2    3019
3    3020
4    3025
dtype: int64
```

```python
years_list + 1000
```

```error
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[126], line 1
----> 1 years_list + 1000

TypeError: can only concatenate list (not "int") to list
```

This illustrates an important principle of Python: different data structures are suitable for different "modes" of working with data.
It can be helpful to work with a list when building up an initial set of data from scratch, but when you are ready to begin operating on that dataset as a whole (performing calculations with it, visualising it, etc), you will be rewarded for switching to a more specialised datatype like a Series or DataFrame from pandas.

## Unexpected data types
Operations like the addition of 1000 we performed on `years_series` work because pandas knows how to add a number to the integer values in the series.
That behaviour is determind by the `dtype` of the series, which makes that `dtype` really important for how you want to work with your data.
Let's explore how the `dtype` is chosen.
Returning to the `years_series` object we created above:

```python
years_series
```

```output
0    3010
1    3025
2    3019
3    3020
4    3025
dtype: int64
```

The `dtype: int64` was determined automatically based on the values passed in.
But what if the values provided are of several different types?

```python
ages_series = pd.Series([2, 3, 5.5, 6, 8])
ages_series
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

[... a lot more lines of traceback ...]

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

- pandas DataFrames carry many methods that can help you explore the properties and distribution of data.
- Using the `help` function, reading error messages, and asking for help are all good strategies when things go wrong.
- The type of an object determines what kinds of operations you can perform on and with it.
- Python evaluates expressions in a line one by one before assigning the final result to a variable.

::::::::::::::::::::::::::::::::::::::::::::::::

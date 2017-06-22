---
layout: post
published: true
title: Pandas Indexing & Selecting using iloc, loc and ix
---

In Pandas 19.02, the indexing follows the same paradigms as Numpy. There's not much of a difference if a newbie starts to slice pandas `Dataframe` according to the numpy conventions. However, there comes a time when things take turn for the worse when he/she encounters the three musketeers: `iloc, loc and ix`. 

![Three musketeers](http://www.frostclick.com/wp/wp-content/uploads/2013/08/THREE-MUSKETEERS.jpg)
**Source: [Frost Click: Three Musketeers](http://www.frostclick.com/wp/index.php/2013/08/21/alexandre-dumas-the-three-musketeers/)**

Here's to the many hours that I spent pulling out my hair in understanding this lot.

We'll start with the basic definitions that Pandas documentation has to offer:

> .loc is primarily label based, but may also be used with a boolean array. .loc will raise KeyError when the items are not found.

> .iloc is primarily integer position based (from 0 to length-1 of the axis), but may also be used with a boolean array. .iloc will raise IndexError if a requested indexer is out-of-bounds, except slice indexers which allow out-of-bounds indexing. (this conforms with python/numpy slice semantics). 

> .ix supports mixed integer and label based access. It is primarily label based, but will fall back to integer positional access unless the corresponding axis is of integer type. .ix is the most general and will support any of the inputs in .loc and .iloc. .ix also supports floating point label schemes. .ix is exceptionally useful when dealing with mixed positional and label based hierarchical indexes.

> However, when an axis is integer based, ONLY label based access and not positional access is supported. Thus, in such cases, it’s usually better to be explicit and use .iloc or .loc.

Hmmm, sounds too technical. Let's take the help of code to understand these.

Consider a dataframe `df`


```python
import pandas as pd
import numpy as np
```


```python
df = pd.DataFrame(np.random.randint(0,100,size=(100, 5)), columns=list('ABCDE'))
```


```python
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>71</td>
      <td>64</td>
      <td>93</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>17</td>
      <td>12</td>
      <td>67</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>70</td>
      <td>12</td>
      <td>83</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>98</td>
      <td>5</td>
      <td>88</td>
      <td>90</td>
    </tr>
    <tr>
      <th>4</th>
      <td>31</td>
      <td>70</td>
      <td>98</td>
      <td>26</td>
      <td>33</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (100, 5)



We'll apply all the three on our dataset:


```python
# Selecting first three rows?
df.iloc[:3]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>71</td>
      <td>64</td>
      <td>93</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>17</td>
      <td>12</td>
      <td>67</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>70</td>
      <td>12</td>
      <td>83</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Selecting first three rows?
df.loc[:3]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>71</td>
      <td>64</td>
      <td>93</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>17</td>
      <td>12</td>
      <td>67</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>70</td>
      <td>12</td>
      <td>83</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>98</td>
      <td>5</td>
      <td>88</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Selecting first three rows?
df.ix[:3]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>71</td>
      <td>64</td>
      <td>93</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>17</td>
      <td>12</td>
      <td>67</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>70</td>
      <td>12</td>
      <td>83</td>
      <td>90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>98</td>
      <td>5</td>
      <td>88</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>



Apart from `iloc`, the rest two don't really follow the same that points out the first difference: `iloc` is integer based while the rest aren't (Note: `loc` is exclusively label based, while `ix` plays the devil's advocate).

`loc` and `ix` select values till the index label _3_ i.e. we get values of _[0,1,2,3]_

Let's correct the `loc` and `ix` to select `first 3 rows`:


```python
# since both behave as label based, let's write them as:
print ("::::The output for loc::::")
print (df.loc[:2])
print ("::::The output for ix::::")
print (df.ix[:2])
```

    ::::The output for loc::::
        A   B   C   D   E
    0  84  71  64  93  65
    1  87  17  12  67  80
    2  45  70  12  83  90
    ::::The output for ix::::
        A   B   C   D   E
    0  84  71  64  93  65
    1  87  17  12  67  80
    2  45  70  12  83  90


Now, the question arises why do we need `loc` or `ix` when the insanely simple `iloc` solves our problems?

Let's consider the following scenario where we need to slice select columns from the dataframe using `iloc`:


```python
# Selecting the column's [A,C] for the first 3 rows
df.iloc[:3, ['A', 'C']]
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-9-2f83469716a2> in <module>()
          1 # Selecting the column's [A,C] for the first 3 rows
    ----> 2 df.iloc[:3, ['A', 'C']]
    

    /home/pratos/miniconda3/lib/python3.5/site-packages/pandas/core/indexing.py in __getitem__(self, key)
       1308 
       1309         if type(key) is tuple:
    -> 1310             return self._getitem_tuple(key)
       1311         else:
       1312             return self._getitem_axis(key, axis=0)


    /home/pratos/miniconda3/lib/python3.5/site-packages/pandas/core/indexing.py in _getitem_tuple(self, tup)
       1558     def _getitem_tuple(self, tup):
       1559 
    -> 1560         self._has_valid_tuple(tup)
       1561         try:
       1562             return self._getitem_lowerdim(tup)


    /home/pratos/miniconda3/lib/python3.5/site-packages/pandas/core/indexing.py in _has_valid_tuple(self, key)
        149             if i >= self.obj.ndim:
        150                 raise IndexingError('Too many indexers')
    --> 151             if not self._has_valid_type(k, i):
        152                 raise ValueError("Location based indexing can only have [%s] "
        153                                  "types" % self._valid_types)


    /home/pratos/miniconda3/lib/python3.5/site-packages/pandas/core/indexing.py in _has_valid_type(self, key, axis)
       1528             return self._is_valid_integer(key, axis)
       1529         elif is_list_like_indexer(key):
    -> 1530             return self._is_valid_list_like(key, axis)
       1531         return False
       1532 


    /home/pratos/miniconda3/lib/python3.5/site-packages/pandas/core/indexing.py in _is_valid_list_like(self, key, axis)
       1551         ax = self.obj._get_axis(axis)
       1552         l = len(ax)
    -> 1553         if len(arr) and (arr.max() >= l or arr.min() < -l):
       1554             raise IndexError("positional indexers are out-of-bounds")
       1555 


    /home/pratos/miniconda3/lib/python3.5/site-packages/numpy/core/_methods.py in _amax(a, axis, out, keepdims)
         24 # small reductions
         25 def _amax(a, axis=None, out=None, keepdims=False):
    ---> 26     return umr_maximum(a, axis, None, out, keepdims)
         27 
         28 def _amin(a, axis=None, out=None, keepdims=False):


    TypeError: cannot perform reduce with flexible type


**Whoa! Up there's a mess homie!**

Unlike numpy, pandas have a `label` based columns and there's a need for a better control over selection of columns in a dataframe. `loc` provides that with it's label based selection (sometimes if not used properly might leave the practitioner with a numb brain). Let's see whether we can do that with `loc`


```python
df.loc[:2, ['A', 'C']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>84</td>
      <td>64</td>
    </tr>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>



**Note: `loc` includes last element, `iloc` doesn't. So, the above behaviour while selecting rows explains that.**

Sweet! It seems `loc` is awesome! What about `ix`, would it be the same?

**Note: `ix` has a very dividing opinion, in the pandas documentation itself the maintainers point towards using `iloc` and `loc` whenever a sane dataframe is being presented. Just to avoid the confusion that prevails from mixture label based and integer based slicing.**

If `loc` is a [Katana](https://en.wikipedia.org/wiki/Katana), then `ix` is [Iron aged double edged swords](https://en.wikipedia.org/wiki/Iron_Age_sword). Let's refer the documentation definition of `ix` again:

> ix supports mixed integer and label based access. It is primarily label based, but will fall back to integer positional access unless the corresponding axis is of integer type. .ix is the most general and will support any of the inputs in .loc and .iloc. .ix also supports floating point label schemes. .ix is exceptionally useful when dealing with mixed positional and label based hierarchical indexes.

The base behavior of `ix` is illustrated as below:


```python
df.ix[3]
```




    A    56
    B    98
    C     5
    D    88
    E    90
    Name: 3, dtype: int64



`df.ix[3]` returns by default the value of the _3rd row_. `df.ix[3,]` would give the similar results.


```python
df.ix[3,]
```




    A    56
    B    98
    C     5
    D    88
    E    90
    Name: 3, dtype: int64




```python
# A more "complex" query using ix
# Hybrid approach
df.ix[1:3, ['A', 'C']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>87</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>12</td>
    </tr>
    <tr>
      <th>3</th>
      <td>56</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Selecting rows from 1 to 3, and columns 1 to 2
# Pure Index based
df.ix[1:3, 1:2]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>98</td>
    </tr>
  </tbody>
</table>
</div>



From the above two examples, we can understand how handy `ix`. Almost like being rescued by Gandalf himself with his sword, Glamdrig! (At least I felt like today 😊)

![Glamdring sword](http://www.cultjer.com/img/ug_photo/2013_11/BYKqJjNIYAA9SHL20131103223218.jpg)
**Source: [Cultjer](http://www.cultjer.com/gandalf-and-sword)**

With all the small snippets, I hope the basic demons about our three musketeers are exorcised. A small real life dataset would help in getting a feel of the things.


```python
winered = pd.read_csv('winequality-red.csv', sep=';')
```


```python
winered.head(5)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fixed acidity</th>
      <th>volatile acidity</th>
      <th>citric acid</th>
      <th>residual sugar</th>
      <th>chlorides</th>
      <th>free sulfur dioxide</th>
      <th>total sulfur dioxide</th>
      <th>density</th>
      <th>pH</th>
      <th>sulphates</th>
      <th>alcohol</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7.8</td>
      <td>0.88</td>
      <td>0.00</td>
      <td>2.6</td>
      <td>0.098</td>
      <td>25.0</td>
      <td>67.0</td>
      <td>0.9968</td>
      <td>3.20</td>
      <td>0.68</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.8</td>
      <td>0.76</td>
      <td>0.04</td>
      <td>2.3</td>
      <td>0.092</td>
      <td>15.0</td>
      <td>54.0</td>
      <td>0.9970</td>
      <td>3.26</td>
      <td>0.65</td>
      <td>9.8</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11.2</td>
      <td>0.28</td>
      <td>0.56</td>
      <td>1.9</td>
      <td>0.075</td>
      <td>17.0</td>
      <td>60.0</td>
      <td>0.9980</td>
      <td>3.16</td>
      <td>0.58</td>
      <td>9.8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.4</td>
      <td>0.70</td>
      <td>0.00</td>
      <td>1.9</td>
      <td>0.076</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>0.9978</td>
      <td>3.51</td>
      <td>0.56</td>
      <td>9.4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Select pH scores having less than 3 with quality equal to 6
winered.ix[:,['pH', 'quality']].query('pH < 3 & quality == 6')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pH</th>
      <th>quality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>86</th>
      <td>2.93</td>
      <td>6</td>
    </tr>
    <tr>
      <th>91</th>
      <td>2.93</td>
      <td>6</td>
    </tr>
    <tr>
      <th>464</th>
      <td>2.98</td>
      <td>6</td>
    </tr>
    <tr>
      <th>544</th>
      <td>2.86</td>
      <td>6</td>
    </tr>
    <tr>
      <th>614</th>
      <td>2.87</td>
      <td>6</td>
    </tr>
    <tr>
      <th>667</th>
      <td>2.94</td>
      <td>6</td>
    </tr>
    <tr>
      <th>669</th>
      <td>2.94</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1017</th>
      <td>2.89</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1018</th>
      <td>2.89</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1319</th>
      <td>2.90</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>



As the above query, we can go on with many such. Hope this has been helpful for the readers. Do comment and correct if any inconsistencies.

__Sources:__

1. [Pandas: Indexing and Selecting](http://pandas.pydata.org/pandas-docs/version/0.14.0/indexing.html)
2. [Is ix always better than loc and iloc?](http://stackoverflow.com/questions/27667759/is-ix-always-better-than-loc-and-iloc-since-it-is-faster-and-supports-i)
3. [Must read Github issue on iloc, loc and ix](https://github.com/pandas-dev/pandas/issues/6683)

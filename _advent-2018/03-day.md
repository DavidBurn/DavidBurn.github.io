---
layout: single
classes: wide
date: 2019-02-11
title: "Advent of code 2018 - Day 3"
excerpt: "A simple walkthrough of my solution"
header:
  overlay_image: "images/advent-doors.png"
  overlay_filter: 0.5
permalink: /advent-2018/day3/
sidebar:
  nav: advent-2018-sidebar
---

# Advent of code - Day 3

[Day 3 instructions](https://adventofcode.com/2018/day/3)
### Part 1
Read in data into a pandas dataframe. 


```python
import pandas as pd
import numpy as np

df = pd.read_csv('day3input.txt',delimiter=' ', skip_blank_lines=True,header=None)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1</td>
      <td>@</td>
      <td>387,801:</td>
      <td>11x22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>#2</td>
      <td>@</td>
      <td>101,301:</td>
      <td>19x14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>#3</td>
      <td>@</td>
      <td>472,755:</td>
      <td>11x16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>#4</td>
      <td>@</td>
      <td>518,720:</td>
      <td>23x17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>#5</td>
      <td>@</td>
      <td>481,939:</td>
      <td>29x20</td>
    </tr>
  </tbody>
</table>
</div>



- Strip column 2 of its trailing colon, split the column on the comma into x_space and y_space.
- Split column 3 into x_length and y_length by the 'x' inbetween the to values.


```python
df['x_space'], df['y_space'] = df[2].str.strip(':').str.split(',',1).str
df['x_length'], df['y_length'] = df[3].str.split('x',1).str

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>x_space</th>
      <th>y_space</th>
      <th>x_length</th>
      <th>y_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>#1</td>
      <td>@</td>
      <td>387,801:</td>
      <td>11x22</td>
      <td>387</td>
      <td>801</td>
      <td>11</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>#2</td>
      <td>@</td>
      <td>101,301:</td>
      <td>19x14</td>
      <td>101</td>
      <td>301</td>
      <td>19</td>
      <td>14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>#3</td>
      <td>@</td>
      <td>472,755:</td>
      <td>11x16</td>
      <td>472</td>
      <td>755</td>
      <td>11</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>#4</td>
      <td>@</td>
      <td>518,720:</td>
      <td>23x17</td>
      <td>518</td>
      <td>720</td>
      <td>23</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>#5</td>
      <td>@</td>
      <td>481,939:</td>
      <td>29x20</td>
      <td>481</td>
      <td>939</td>
      <td>29</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



- Drop unnecessary columns
- Change datatypes to integers


```python
df.drop(columns=[0,1,2,3],inplace=True)
df = df.astype(int)

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>x_space</th>
      <th>y_space</th>
      <th>x_length</th>
      <th>y_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>387</td>
      <td>801</td>
      <td>11</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>301</td>
      <td>19</td>
      <td>14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>472</td>
      <td>755</td>
      <td>11</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>518</td>
      <td>720</td>
      <td>23</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>481</td>
      <td>939</td>
      <td>29</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



Find max x and y coordinates for mapping out fabric.


```python
max_x = df['x_space'].max() + df['x_length'].max()
max_y = df['y_space'].max() + df['y_length'].max()

print('Max x value : {}'.format(max_x))
print('Max y value : {}'.format(max_y))
```

    Max x value : 1016
    Max y value : 1012


Initialise array of zeros of dimensions (max_x, max_y) representing the fabric.


```python
fabric = np.zeros((max_x,max_y))
```

For each row in the dataframe:

- Read values for x, y, x_length and y_length
- Initialize array of ones of dimensions (x_length, y_length) representing the size of the claim. 
- Add the claim array onto the fabric array at the corresponding coordinates


```python
for i in range(len(df)):
    x, y, x_length, y_length = df.loc[i,:]
    claim = np.ones((x_length, y_length))
    fabric[x:x+x_length, y:y+y_length] += claim
```

Fabric representation and mapping example:


```python
sample = np.zeros((5,5))
sample
```




    array([[0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.]])




```python
sample_claim = np.ones((2,3))
sample_claim
```




    array([[1., 1., 1.],
           [1., 1., 1.]])




```python
sample[0:2, 1:4] += sample_claim
sample
```




    array([[0., 1., 1., 1., 0.],
           [0., 1., 1., 1., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.]])



Find counts of overlapping claims.


```python
unique, counts = np.unique(fabric,return_counts=True)
print(sorted(zip(unique, counts)))
```

    [(0.0, 679490), (1.0, 233398), (2.0, 85755), (3.0, 22904), (4.0, 5526), (5.0, 931), (6.0, 172), (7.0, 16)]


Find total that is overlapping with at least one other claim.


```python
total_double_matched = counts[2:].sum()
print('Total double matched : {}'.format(total_double_matched))
```

    Total double matched : 115304


## Part 2 - Find the only claim with no overlap
We already have an array holding the fabric, with all the claims added in the correct place. 


```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12,12))
plt.title('Visual representation of fabric')
plt.xlabel('x-coordinates')
plt.ylabel('y-coordinates')
plt.imshow(fabric)
plt.colorbar()
plt.show()
```


![png](/images/advent-2018/output_23_0.png)


So to find the claim that does not overlap we need to loop through the claims again, checking whether the claim (array of ones) is equal to the fabric at the coordinates of the claim.


```python
for i in range(len(df)):
    x, y, x_length, y_length = df.loc[i,:]
    claim = np.ones((x_length, y_length))
    if np.array_equal(fabric[x:x+x_length, y:y+y_length], claim):
        print('Thats the one! :\n', df.loc[i,:])
```

    Thats the one! :
     x_space     336
    y_space     615
    x_length     28
    y_length     21
    Name: 274, dtype: int64


Name : 274 is the entry we are looking for, my answer is 275 because we deleted the index column earlier and pandas dataframes index from 0 and not 1.

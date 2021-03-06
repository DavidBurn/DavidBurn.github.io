---
layout: single
classes: wide
date: 2019-02-10
title: "Advent of code 2018 - Day 1 and 2"
excerpt: "A brief intro and my solutions for day 1 and 2"
header:
  overlay_image: /images/advent-doors.png
  overlay-filter: 0.5
permalink: /advent-2018/day1-2/
sidebar:
  nav: advent-2018-sidebar
---
# Advent of code 2018 - Day 1 and 2

In December 2018 a friend challenged me to have a go at the [Advent Of Code](https://adventofcode.com/2018), a new coding challenge for each day of advent, complete with a leaderboard for those that way inclined. The challenges start off simple and quickly get very tricky, being busy and with each challenge taking me a while to complete I didn't get very far in December so I will try and complete them throughout the year.

The instructions for each day are quite long and detailed so I will post a link to the instructions each day and walk through my solution.

## Day 1
[Day 1 instructions](https://adventofcode.com/2018/day/1)

### Part 1
**Find the sum of all values in the input file**

Open input file, split on whitespace and preview data. 


```
with open('day1input.txt') as file:
    data = file.read().split()
```

Use a generator expression to sum all of the values and print.


```
first_total = sum(int(x) for x in data)
print('Sum of all entries : {}'.format(first_total))
```

    Sum of all entries : 582


### Part 2
**Find the first repeated value when the input values are summed in a continuous loop**

Initialise emtpy counter for running total, and empty set for recording the running total after each addition.


```
total = 0
subtotals = {0}
```

For each entry in the data, add to the total, check if the total has already been recording and print if so. If not then record the total in subtotals. This process repeats, cycling through every entry in the input file over and over again until a repeated subtotal is found.


```
run=True
while run:
    for x in data:
        total += int(x)
        if total in subtotals:
            print('First repeated total : {}'.format(total))
            run = False
            break
        subtotals.add(total)
```

    First repeated total : 488

## Day 2 
[Day 2 instructions](https://adventofcode.com/2018/day/2)
### Part 1
**Find counts of input strings that contain exactly two or three instances of a character, the answer is the sum of these counts**

Open input file, read in data and split it on whitespace.


```
from collections import defaultdict
import itertools
import pprint

with open('day2input.txt') as file:
    data = file.read().split()
```

Preview data.


```
data[:10]
```




    ['qywzphxoiseldjrbaoagvkmanu',
     'qywzphxeisulpjrbtfcgvkmanu',
     'qywzxhooiseldjrbtfcgvcmanu',
     'qywzphjojseldjubtfcgvkmanu',
     'qtwzphxoieeldjrbtfcgvrmanu',
     'tywzphzoiseldjritfcgvkmanu',
     'qyuzphxoiseldjrbtfcgykbanu',
     'qswzmhxoiseldjrbtfcgvkaanu',
     'qyczqhxoiseldjrbtfcgvkbanu',
     'qybzpqxooseldjrbtfcgvkmanu']



Initialise counter for sets of twos and threes.


```
twos = 0
threes = 0
```

### *check_pairs* function
- Initialise *letters*, an empty dictionary with default value of 0.
- For each letter in the string, add one to the corresponding key in *letters*.
- Check for letters with exactly two entries in the string
- Check for letters with exactly three entries in the string
- Return updated twos and threes counters.


```
def check_pairs(string, twos, threes):
    letters = defaultdict(int)
    for s in string:
        letters[s] += 1
    if 2 in letters.values():
        twos += 1
    if 3 in letters.values():
        threes += 1
    return twos, threes
```

Run through of what happens inside the function :


```
data[0]
```




    'qywzphxoiseldjrbaoagvkmanu'




```
letters = defaultdict(int)
for s in data[0]:
    letters[s] += 1
if 2 in letters.values():
    twos += 1
if 3 in letters.values():
    threes += 1

pprint.pprint(letters)
```

    defaultdict(<class 'int'>,
                {'a': 3,
                 'b': 1,
                 'd': 1,
                 'e': 1,
                 'g': 1,
                 'h': 1,
                 'i': 1,
                 'j': 1,
                 'k': 1,
                 'l': 1,
                 'm': 1,
                 'n': 1,
                 'o': 2,
                 'p': 1,
                 'q': 1,
                 'r': 1,
                 's': 1,
                 'u': 1,
                 'v': 1,
                 'w': 1,
                 'x': 1,
                 'y': 1,
                 'z': 1})

The dictionary values here show the count of each letter in the string, these values are what is being considered when checking for twos and threes.

Now we can use the *check_pairs* function to loop over all of the strings. 


```
twos = 0
threes = 0

for d in data:
    twos, threes = check_pairs(d, twos, threes)
    
print('Twos = {}, Threes = {}'.format(twos, threes))

print('Checksum = {}'.format(twos*threes))
```

    Twos = 249, Threes = 22
    Checksum = 5478

## Part 2
**Find the two boxes that have identical IDs except for one character**

To find the correct strings, the function ***check_matches*** takes two strings as arguments, zipping them together and comparing each value one by one. Each matching charater is added to a list *matched*, if they do not match, the diffs counter increases by one. If diffs is greater than the *num_diffs* (default 1 for this task) then the function is cut short. Once the two strings are found the matching characters are printed out. 

To generate the combinations of strings, we use *itertools.combinations* to generate every combination of two strings in the input data.
```
def check_matches(string1, string2, num_diffs=1):
    diffs = 0
    matched = []
    for l1, l2 in zip(string1, string2):
        if l1 != l2:
            diffs += 1
            if diffs > num_diffs:
                pass
        else:
            matched.append(l1)
    if diffs == num_diffs:
        print('String 1 : {}. String 2 : {}.'.format(string1, string2))
        print('Matching characters : {}'.format(''.join(matched)))
        
for x in itertools.combinations(data,2):
    check_matches(*x)
```


    String 1 : qywzphxoiseldjrntfygvdmanu. String 2 : qyszphxoiseldjrntfygvdmanu.
    Matching characters : qyzphxoiseldjrntfygvdmanu

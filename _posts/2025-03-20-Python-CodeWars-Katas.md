---
categories:
- Python
- Freetime
- Fundamentals
layout: post
image:
    path: images/media/python.png
media_subpath: /assets/posts/2025-03-20-Python-CodeWars
tags:
- Python
title: Python - CodeWars - Kata's
---

### Introduction

I was curious about my programming skills, specifically with Python and wanted to measure those skills with CodeWars. I gathered some tasks here, which I have completed at the moment. CodeWars is a great way to maintain/learn any programming language skills with repetitive learning and in a small scale. **There is no AI involved in these solutions.**

### Aliens speak - The Story:

Aliens from Kepler 27b have immigrated to Earth! They have learned English and go to our stores, eat our food, dress like us, ride Ubers, use Google, etc. However, they speak English a little differently. Can you write a program that converts our English to their Alien English?

#### Task:

Write a function that receives a lowercase string and converts it from our English to Alien English. They tend to speak the letter a like o and o like a u.

```
"hello" ---> "hellu"
"codewars" ---> "cudewors"
```

#### Solution (Test passed)

```
def convert(st):
    # Code here
    newstr = []
    alienstr = ["a","o"]
    for x in st:
        if x == alienstr[0]:
            newstr.append("o")
        elif x == alienstr[1]:
            newstr.append("u")
        else:
            newstr.append(x)
    s = "".join(newstr)
    return s
```

#### Best practices (Other coders)

```
def convert(st):
    return st.replace('o','u').replace('a','o')
```
#### Task Author:
user2514386 (CodeWars User)

### Returning strings

Create a function that accepts a parameter representing a name and returns the message: "Hello, <name> how are you doing today?".

[Make sure you type the exact thing I wrote or the program may not execute properly]

#### Solution (Test passed)

```
def greet(name):
    #Good Luck (like you need it) <-- this was in the code by default

    return f"Hello, {name} how are you doing today?" 

    pass <-- this was in the code by default
```
#### Task Author:
Aweson1 (CodeWars User)

### Beginner Series #1 School Paperwork

#### Description:

Your classmates asked you to copy some paperwork for them. You know that there are 'n' classmates and the paperwork has 'm' pages.

Your task is to calculate how many blank pages do you need. If n < 0 or m < 0 return 0.
Example:
```
n= 5, m=5: 25
n=-5, m=5:  0
```

#### Solution (Test Passed)

```
def paperwork(n, m):
    # Happy Coding! ^_^
    if n < 0 or m < 0:
        return 0
    else:
        return n * m
```
#### Task Author:
Vortus (CodeWars User)

### Beginner Series #2 Clock

#### Description:

Clock shows h hours, m minutes and s seconds after midnight.

Your task is to write a function which returns the time since midnight in milliseconds.

Example:
```
h = 0
m = 1
s = 1

result = 61000
```
Input constraints:
```
0 <= h <= 23
0 <= m <= 59
0 <= s <= 59
```

#### Solution (Test passed)

```
def past(h, m, s):
    # Good Luck!
    return (3600 * h + 60 * m + s)*1000
```
#### Task Author:
Vortus (CodeWars User)

### Remove String Spaces

#### Description:

Write a function that removes the spaces from the string, then return the resultant string.

Examples (Input -> Output):
````
"8 j 8   mBliB8g  imjB8B8  jl  B" -> "8j8mBliB8gimjB8B8jlB"
"8 8 Bi fk8h B 8 BB8B B B  B888 c hl8 BhB fd" -> "88Bifk8hB8BB8BBBB888chl8BhBfd"
"8aaaaa dddd r     " -> "8aaaaaddddr"
````

#### Solution:
````
def no_space(x):
    #your code here
    return x.replace(" ","")
````

#### Task Author:
PG1 (CodeWars User)

### Convert a String to a Number!

#### Description:

We need a function that can transform a string into a number. What ways of achieving this do you know?

Note: Don't worry, all inputs will be strings, and every string is a perfectly valid representation of an integral number.
Examples:
````
"1234" --> 1234
"605"  --> 605
"1405" --> 1405
"-7" --> -7
````

#### Solution:

````
def string_to_number(s):
    # your code here
    return int(s)
    pass
````

#### Task Author:
bkaes (CodeWars User)

### Transportation on vacation

#### Description:

After a hard quarter in the office you decide to get some rest on a vacation. So you will book a flight for you and your girlfriend and try to leave all the mess behind you.

You will need a rental car in order for you to get around in your vacation. The manager of the car rental makes you some good offers.

Every day you rent the car costs $40. If you rent the car for 7 or more days, you get $50 off your total. Alternatively, if you rent the car for 3 or more days, you get $20 off your total.

Write a code that gives out the total amount for different days(d).

#### Solution:

````
def rental_car_cost(d):
    # your code
    if d >= 7:
        return d*40 - 50
    if d >= 3 and d < 7:
        return d*40 - 20
    else:
        return d*40
````
#### Task Author:
Milford (CodeWars User)
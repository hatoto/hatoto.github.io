---
layout: post
title:  "Python Basic: dictionary"
date:   2017-07-07
categories: python
tags: python
---

* content
{:toc}


## Dictionary

#### 做一個dictionary

方式一

```python
diction = {'a':1, 'b':2}
print(diction)
#{'a': 1, 'b': 2}
```

方式二

```python
diction = dict(c = 3, d = 4)
print(diction)
#{'c': 3, 'd': 4}
```



 
#### 取出dictionary 中某key的值

```python
diction = {'a':1, 'b':2, 'c':3, 'd':4}

print("diction['c'] -> {}".format( diction['c'] ))
#diction['c'] -> 3
```

#### 在dictionary加入元素

```python
diction = {'a':1, 'b':2, 'c':3, 'd':4}

print("dictionary original {}:".format(diction))
#dictionary original :{'a': 1, 'b': 2, 'c': 3, 'd': 4}

diction["e"] = 99

print("dictionary after {}:".format(diction))
#dictionary after :{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 99}
```

#### 列出dictionary中的value以及使用sum()加總

dictionary.value()會產出以dictionary中value的list
用sum( list obj) 可取得list中的加總值

```python
diction = {'a':2, 'b':4, 'c':6, 'd':8}

print("diction.values(): {}".format(diction.values()))
#diction.values(): dict_values([2, 4, 6, 8])

print("sum(diction.values(): {}".format(sum(diction.values())))
#sum(diction.values(): 20
```

#### 列出dictionary中所有item

```python
diction = {"a": 1, "b": 2, "c": 3, "d":4, "e":5, "f":6, "g":7, "h":8, "i":9 , "j":10}

print( "diction.items(): {}".format( diction.items() ))
#diction.items(): dict_items([('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5), ('f', 6), ('g', 7), ('h', 8), ('i', 9), ('j', 10)])

for key, value in diction.items():
    print(key , 'has value:', value)
    
#a has value: 1
#b has value: 2
#c has value: 3
#d has value: 4
#e has value: 5
#f has value: 6
#g has value: 7
#h has value: 8
#i has value: 9
#j has value: 10
```


#### 依條件過濾dictionary，並產生新dictionary

篩選dictionary中可被2整除的value，並產生篩選後的dictionary

```python
diction = {"a": 1, "b": 2, "c": 3, "d":4, "e":5, "f":6, "g":7, "h":8, "i":9 , "j":10}

diction = dict((key, value) for key, value in diction.items() if (value % 2 == 0) )

print("diction after filter value % 2 == 0 -> {}".format(diction))

#diction after filter value % 2 == 0 -> {'b': 2, 'd': 4, 'f': 6, 'h': 8, 'j': 10}

```

#### 用pprint印出排列好看的dictionary內容

```python
diction = dict(a = list(range(1, 11)), b = list(range(11, 21)), c = list(range(21, 31)))

print('user print---->')
print(diction)

#user print---->
#{'a': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 'b': [11, 12, 13, 14, 15, 16, 17, 18, 19, 20], 'c': [21, 22, 23, 24, 25, 26, 27, 28, 29, 30]}


from pprint import pprint
print('user pprint---->')
pprint(diction)
#user pprint---->
#{'a': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
# 'b': [11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
# 'c': [21, 22, 23, 24, 25, 26, 27, 28, 29, 30]}
```

#### 用dictionary內容印成json字串

```python
classroom = {"students":[{"firstName": "Stupid", "lastName": "Apple"},
                {"firstName": "Strong", "lastName": "Bee"},
                {"firstName": "Sweet", "lastName": "Cake"}],
"teachers":[{"firstName": "Knock", "lastName": "Door"},
          {"firstName": "Out", "lastName": "Door"}]}

import json
from io import StringIO

io = StringIO()
json.dump(classroom, io)

print('classroomInJson: ', io.getvalue())

#classroomInJson {"students": [{"firstName": "Stupid", "lastName": "Apple"}, {"firstName": "Strong", "lastName": "Bee"}, {"firstName": "Sweet", "lastName": "Cake"}], "teachers": [{"firstName": "Knock", "lastName": "Door"}, {"firstName": "Out", "lastName": "Door"}]}

```



#### 其他-印出 a-z
```python

import string

print(string.ascii_lowercase)
#abcdefghijklmnopqrstuvwxyz

print(string.ascii_uppercase)
#ABCDEFGHIJKLMNOPQRSTUVWXYZ

for achar in string.ascii_uppercase:
    print(achar)
#A
#B
#C
#D
#E
#F
#G
#H
#I
#J
#K
#L
#M
#N
#O
#P
#Q
#R
#S
#T
#U
#V
#W
#X
#Y
#Z
```



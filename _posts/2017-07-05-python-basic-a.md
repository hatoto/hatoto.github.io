---
layout: post
title:  "Python Basic: list and range"
date:   2017-07-05
categories: python
tags: python
---

* content
{:toc}


## list操作
#### 取list中一段區間的內容


```python
characters = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]

print("characters[4:7] => {}".format( characters[4:7] ))

#characters[4:7] => ['e', 'f', 'g']
```
區間是由第4的元素「e」到第7個元素(不包含第7個)，所以最後只會拿到g(第6個元素)




#### 取由list開頭算起的一段內容
```python
characters = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]

print("characters[:4] => {}".format( characters[:4] ))

#characters[:4] => ['a', 'b', 'c', 'd']
```
[:4]就是[0:4]的意思

#### 取得由list 尾端算起的一個元素

```python
characters = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]

print("characters[-4] => {}".format( characters[-4] ))

#characters[-4] => g
```
由尾端算時，最後一個元素的index是-1

#### 取得由list 尾端算起的一段內容
```python
characters = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]

print("characters[-4:] => {}".format( characters[-4:] ))

#characters[-4:] => ['g', 'h', 'i', 'j']
```

#### 依規則取得list 中內容
取得由第一個元素開始每隔2個位置的元素，[start : end : step]

```python
characters = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"]

print("characters[::2] => {}".format( characters[::2] ))

#characters[::2] => ['a', 'c', 'e', 'g', 'i']
```

#### 移除list 中重複的項目
用set來移除list中重複的項目，但是元素的順序會重排

```python
alist = ["3", "3", 1, 3, "1", 2]
alist = list(set(alist))
print(alist)
#[1, 2, 3, '3', '1']
```

用OrderedDict來移除list中重複的項目，保持原本的順序

```python
from collections import OrderedDict

alist = ["3", "3", 1, 3, "1", 2]
alist = list(OrderedDict.fromkeys(alist))
print(alist)
#['3', 1, 3, '1', 2]
```

#### 遍歷list時取得每個元素的index

```python
theList = ['a', 'b', 'c']
for index, item in enumerate(theList):
    print("Item %s has index %s" % (item, index))

#Item a has index 0
#Item b has index 1
#Item c has index 2
```

在進行迴圈時把list轉成enumerate，然後就可以取到index和元素本身


## range

內建函數range() ，可產生整數序列，
產生的參數：
range( size ) -> 從0到size
range( start, stop ) -> 從start到 stop -1
range( start, stop, step ) -> 從start到 stop -1, 每隔 step產生一個

```python
print(list(range(10)))
 # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(list(range(5, 11)))
 # [5, 6, 7, 8, 9, 10]
print(list(range(1, 10, 2)))
 # [1, 3, 5, 7, 9]
```

#### 用range產生一個[100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200]的list

```python
my_range_a = range(10, 21)
print([10 * var for var in my_range_a])

#[100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200]
```
這邊比較難記，「for var in my_range_a」就是跑my_range_a的迴圈，然後var就是每個元素，然後拿出來乘10再放回去list。

#### 把range產生的 int list轉換成string list 


```python
my_range = range(1, 21)

print(list(map(str, my_range)))

#['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20']
```


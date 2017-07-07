---
layout: post
title:  "Python Basic: function"
date:   2017-07-07
categories: python
tags: python
---

* content
{:toc}


## Function

#### 定義 function

用 def 來建立function，不需要像java 指定回傳型別

* 沒有回傳東西的時候回傳的是None

```python
def function(a, b):
    print('infunction a+b:', a + b )

print('call function(3,4):', function(3, 4))

#infunction a+b: 7
#cll function(3,4): None
```

* funcion有定義參數，但是呼叫時沒帶值

```python
def function(a, b):
    print('infunction a+b:', a + b )
    
print('call function():', function())
#TypeError: function() missing 2 required positional arguments: 'a' and 'b'
```

* 創建function 時，參數可設定預設值

```python
def function(a = 1, b = 2):
    return a + b

print(type(function))
#用type()來看function
#<class 'function'>

print(type(function(3,4)))
#<class 'int'>

print(function())
#3
#沒帶參數，使用預設的a = 1, b = 2

print(function(3, 4))
#7
```

* 如果function 的參數中有設定預設值，該參數之後的參數都要建立預設值，不然會有SyntaxError: non-default argument follows default argument。

```python
def function(a =1 , b):
    return a + b

print(function(3, 4))


#    def function(a =1 , b):
#                 ^
#SyntaxError: non-default argument follows default argument
```

#### 變數

* 全域變數 global vairables

```python
c = 1
def function():
    return c
c = 3

print('value from call function :', function())
#value from call function : 3
```

* 區域變數 local vairables

```python
c = 1
def function():
    c = 2
    return c
c = 3
print('value from call function :',function())
#value from call function : 2
```

* 在function中定義全域變數

```python
def function():
    global va
    va = 99
    return va

try:
    print('get va value before call function :',va)
except Exception as exp:
    print(type(exp))
    print(exp.args)

function()

print('get va value after call function :',va)

#<class 'NameError'>
#("name 'va' is not defined",)
#get va value after call function : 99
```

在執行function前先印一次va，此區塊用try except包起來；執行到這邊時會發生錯誤，原因是「name 'va' is not defined」。
執行functino後再印一次va，這時va有存在並可以印出值。

#### 使用string.split()

* 建立一個funtion 傳入文字後計算有幾個字

```python
def words_count(string, splitter = None):
    word_list = string.split(splitter)

    return len(word_list)

print('split with blank, words count:', words_count("python is A:good, B:great!"))
print('split with [,], words count:',words_count("python is A:good, B:great!", ','))

#split with blank, words count: 4
#split with [,], words count: 2
```

這邊的words_count可帶2個參數，第一個是要計算字數的字串，後面的參數是要以什麼符號作為切分單位。沒有傳入時帶入None。

使用split()沒帶參數預設以空白為切分單位，所以第一次words_count沒帶參數，會切成4個字，第2次使用「,」會切成2個字。

* 建立一個funtion 讀取一個txt檔後取得其中的文字，判斷其中有幾個字

wordA.txt

```
I have a pen, I hava a pencil.
```

```python
def words_count(filepath):
    with open(filepath, 'r') as file:
        string = file.read()
        print("sentence in txt file:", string)
        strng_list = string.split()
        return len(strng_list)

print("sentence words count in txt file:", words_count("wordsA.txt"))

#sentence in txt file I have a pen, I hava a pencil.
#sentence words count in txt file 8
```

* 計算字數時，排除符號與文字相連的情況

```python
def words_count(string):
    string_list = string.split()
    print('直接split，沒有排除符號連文字的情況:', string_list)
    print('字數：',len(string_list))
    string = string.replace(",", " ")
    string_list = string.split()
    print('把符號替換成空白在split:', string_list)
    print('字數：',len(string_list))

words_count("I like coffee, wine,and milk.")
#直接split，沒有排除符號連文字的情況: ['I', 'like', 'coffee,', 'wine,and', 'milk.']
#字數： 5
#把符號替換成空白在split: ['I', 'like', 'coffee', 'wine', 'and', 'milk.']
#字數： 6
```

* 使用re取代文字中的符號

```python

import re
def words_count(string):
    string = re.sub('[,]', ' ', string)
    print(string)
    string_list = string.split()
    print('直接split，把符號替換成空白:', string_list)
    print('字數：',len(string_list))

words_count("I like coffee, wine,and milk.")

#I like coffee  wine and milk.
#直接split，把符號替換成空白: ['I', 'like', 'coffee', 'wine', 'and', 'milk.']
#字數： 6

```



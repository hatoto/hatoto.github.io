---
layout: post
title:  "Python basic: IO"
date:   2017-07-09
categories: python
tags: python
---

* content
{:toc}


## Write and Read File

#### 在一個檔案中寫入26個英文字母，每個字母一行

```python
import string

with open("letters.txt", "w") as file:
    for l in string.ascii_lowercase:
        file.write(l + "\n")
```

#### 依26個字母製作26個txt檔案

* 每個檔案檔名為A.txt-Z.txt,每個檔案中有一個和檔名相同的字母

```python
import string, os

if not os.path.exists("AtoZ"):
    os.makedirs("AtoZ")

for letter in string.ascii_uppercase:
    with open("AtoZ/" + letter + ".txt", "w") as file:
        file.write( letter + "\n")
```

先判斷將被執行的程式目錄中是否有AtoZ的資料匣，沒有的話先做一個，然後把相關檔案產生致該資料匣中。

#### 由26個字母檔的每個字並放入list中

上面的練習是在AtoZ 資料匣中寫入A到Z的字母檔案，下面是把該資料匣中的文字讀入，並放到list裡

```python
import glob

letters = []
file_list = glob.glob("AtoZ/*.txt")

for filename in file_list:
    with open(filename, 'r') as file:
        letters.append(file.read().strip('\n'))

print(letters)
#['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
```

這邊使用glob，用來搜尋資料匣中的文件，使用glob可以像在linux中使用find指令，例如在執行此python 程式的路徑中，有AtoZ的資料匣，則在linux 的console中，可以下 ```find AtoZ/*.txt``` 列出所以有AtoZ中txt檔案。



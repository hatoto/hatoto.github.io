---
layout: post
title:  "Java Stream findFirst"
date:   2017-07-03
categories: java
tags: java
---

* content
{:toc}


用Stream找到List裡面第一個符合條件的物件，用Optional來接查找後的結果。

下表是不同條件的商品規則，這邊要來用Stream來取得搜尋條件中符合的第一個項目。

### 製作商品規則List


```java
ProductRule ruleA = new ProductRule("001", "cake BANANA", "TWD", "Y");
ProductRule ruleB = new ProductRule("002", "cake APPLE", "TWD", "Y");
ProductRule ruleC = new ProductRule("003", "cake GRAPE", "JPY", "N");
ProductRule ruleD = new ProductRule("004", "cake ORANGE", "JPY", "N");
ProductRule ruleE = new ProductRule("005", "cake KIWI", "HKD", "N");

List<ProductRule> ruleList = new ArrayList<ProductRule>();
        ruleList.add(ruleA);
        ruleList.add(ruleB);
        ruleList.add(ruleC);
        ruleList.add(ruleD);
        ruleList.add(ruleE);
```


### 沒找到符合條件的情況

用**filter**過濾條件，用**peek**印出進行每個項目時的情況，最後用**findFirst()**取得結果。



```java
Optional<ProductRule> ruleNotFound = ruleList.stream()
                .peek( kx -> System.out.println("Currency: " + kx.getCurrency() + ", hasVoucher: " + kx.getHasVoucher() ))
                .filter(kx -> StringUtils.equals("USD", kx.getCurrency() )  &&  StringUtils.equals("Y", kx.getHasVoucher() )   )
                .findFirst();

System.out.println("ruleNotFound : " + ruleNotFound);
System.out.println("rule.isPresent(): " + ruleNotFound.isPresent());
/* 輸出結果
Currency: TWD, hasVoucher: Y
Currency: TWD, hasVoucher: Y
Currency: JPY, hasVoucher: N
Currency: JPY, hasVoucher: N
Currency: HKD, hasVoucher: N
ruleNotFound : Optional.empty
rule.isPresent(): false
*/
```

查找結果沒有符合條件的物件，印出ruleNotFound時不是顯示null，而是顯示Optional.empty，在判斷是否有結果時用.isPresent()。

### 找到符合條件的情況


```java
Optional<ProductRule> ruleFound = ruleList.stream()
                .peek( kx -> System.out.println("Currency: " + kx.getCurrency() + ", hasVoucher: " + kx.getHasVoucher() ))
                .filter(kx -> StringUtils.equals("JPY", kx.getCurrency() )  &&  StringUtils.equals("N", kx.getHasVoucher() )   )
                .findFirst();

System.out.println("ruleFound : " + ruleFound);
System.out.println("ruleFound.isPresent(): " + ruleFound.isPresent());
if( ruleFound.isPresent() ){
    System.out.println("Product Name: " + ruleFound.get().getName());
}
/*
Currency: TWD, hasVoucher: Y
Currency: TWD, hasVoucher: Y
Currency: JPY, hasVoucher: N
ruleFound : Optional[hatoto.test.PlayStreamFindFirst$ProductRule@161cd475]
ruleFound.isPresent(): true
Product Name: cake GRAPE
*/
```

有符合條件的項目時，先用.isPresent判斷，再用.get()來取得原本類別的物件。




gist：[streamFindFirst.java](https://gist.github.com/hatoto/9e6a8795cff7f5a1af69fc4db893af7b)


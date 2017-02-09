---
layout: post
title:  "Java Stream 應用"
date:   2017-02-10
categories: java
tags: java
---

* content
{:toc}


Java 8 新增Stream Package，使用Stream可以少寫很多for 和 if。

下表是不同幣別的商品及金額，這邊要來用Stream來取得表中各幣別的金額合計。


ProductName | Currency | Price
------------| ---------| ------
bookTW1 | TWD | 100
bookTW2 | TWD | 200
bookJP1 | JPY | 1000
bookJP2 | JPY | 10000
bookUS1 | USD | 10
bookUS2 | USD | 15


### 取得所有幣別

首先是拿到表中每種幣別的集合，呼叫createProduct()拿到含所有product的productList後，把productList.stream()後，用.map() get product中的currency，最後以.collect 把取得的currency 放到Set中，就可先將不重複的幣別拿出來。


```java
List<Product> productList = createProduct();
Map<String, BigDecimal> currencyValuMap = new HashMap<>();
		
Set<String> currencyStringSet = productList.stream().filter(x -> null != x.getCurrency()  )
				.map(Product::getCurrency)
				.collect(Collectors.toSet());
System.out.println(currencyStringSet);
//output --> [TWD, JPY, USD]
```


### 加總每個幣的price

直接用forEach 去跑 currencyStringSet中的每種幣別，在.filter中比對到相同幣別時，用.mapToDouble 來拿price 這個double field，並可直接下.sum()來加總每個商品的price。



```java
if( CollectionUtils.isNotEmpty(currencyStringSet)){
	currencyStringSet.forEach( item -> {
		double sum = productList.stream().filter( d ->
		StringUtils.isNotBlank(d.getCurrency()) 
		&& StringUtils.equals(item, d.getCurrency() ) )
			.mapToDouble(Product::getPrice).sum();
		currencyValuMap.put(item, new BigDecimal(sum));
	});
}
System.out.println(currencyValuMap);
//output --> {TWD=300, JPY=11000, USD=25}

```







gist：[https://gist.github.com/hatoto/d9f2a99481e7764da15b941c732da116](https://gist.github.com/hatoto/d9f2a99481e7764da15b941c732da116)


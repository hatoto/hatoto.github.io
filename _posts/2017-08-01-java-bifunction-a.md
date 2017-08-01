---
layout: post
title:  "Java8 使用Function, compose, andThen"
date:   2017-08-01
categories: java
tags: java
---

* content
{:toc}


使用 Java 8 的 Predicate，可以先針對特定class設定過濾條件，在`stream().filter()`時，把Predicate放在`filter()`中。

而BiFunction, Function是在建立method時，直接依條件把傳入的List參數篩選好再回傳出去。
使用的方式是對BiFunction/Function 用 `.apply(arg....)` 把要過濾的list 傳入。


### 測試資料

[Books]


 Title| PublishDate | Author | Tags
------| ------------| ------ | -----
BookA | 20150101 | Apple Bee |Art, Comics
BookB | 20160101 | Apple Bee |Comics
BookC | 20151001 | Cake Door |Law
BookD | 20150601 | Egg Frog |History
BookE | 20161001 | Goose Horse |Art, Education
BookF | 20141001 | Ibis Jellyfish |Art, Business



### 建立Publication interface
```java
interface Publication{
	public String getTitle();
	public String getAuthor();
	public List<String> getTags();
	public LocalDate getPublished();
}
```

### 建立Book class

```java
class Book implements Publication {
    String title;
    String author;
    List<String> tags;
    LocalDate published;
.
.
//getter, setter....
}
```


### 建立BiFunction

建立BiFunction/Function時所用的型別是Publication, 因此在method中，
寫 pubs -> 時，pubs 代表的即是 List<Publication>, 可由其中直接取出相關欄位來進行條件篩選。


* 傳入作者名和List<Publication>，回傳該作者的Publication列表

```java
BiFunction<String, List<Publication>, List<Publication>> getByAuthor = 
(name, pubs) -> pubs.stream()
.filter(a -> a.getAuthor().equals(name)).collect(Collectors.toList());
```

* 傳入Tag名稱和List<Publication>，回傳包含Tag名稱的Publication列表

```java	
BiFunction<String, List<Publication>, List<Publication>> getByTag = (tag, pubs) -> pubs.stream()
	  .filter(a -> a.getTags().contains(tag))
	  .collect(Collectors.toList());
```
* 傳入List<Publication>，依日期由近到遠排序後回傳整個列表

```java	
Function<List<Publication>, List<Publication>> sortByDate = 
         pubs -> pubs.stream()
			.sorted((x, y) -> y.getPublished()
			.compareTo(x.getPublished()))
			.collect(Collectors.toList());
```

* 傳入List<Publication>，取得第一筆Publication

```java	
Function<List<Publication>, Optional<Publication>> getFirst = 
a -> a.stream().findFirst();
```	
	
* 傳入List<Publication>，依日期由近到遠排序後，取得第一筆Publication
	
```java	
Function<List<Publication>, Optional<Publication>> getNewest = 
getFirst.compose(sortByDate);
```
這邊建立的的Function，是用`compose()`結合`getFirst` 和`sortByDate` 二個Function。

`sortByDate`作為`compose()`的傳入參數，會先執行，然後再執行getFirst。


* 傳入作者名和List<Publication>，回傳該作者的最新Publication

```java	
BiFunction<String, List<Publication>, Optional<Publication>> newestByAuthor = 
getByAuthor.andThen(getNewest);
```	
這邊建立的的BiFunction，是用`andThen()`結合`getByAuthor` 和`getNewest` 二個Function。

使用`andThen()`時，`getByAuthor()`先執行，然後再執行`getNewest`。


* 傳入作者名和List<Publication>，回傳該作者依日期由近到遠排序的Publication列表

```java	
BiFunction<String, List<Publication>, List<Publication>> byAuthorSortedByDate = 
getByAuthor.andThen(sortByDate);
```

* 傳入Tag名稱和List<Publication>，回傳包含Tag名稱依日期由近到遠排序的Publication列表

```java	
BiFunction<String, List<Publication>, Optional<Publication>> newestByTag = 
getByTag.andThen(getNewest);
```




### 建立測試資料

```java
List<Publication> allBookList = Arrays.asList(
new Book("BookA", "Apple Bee", Arrays.asList("Art", "Comics"), LocalDate.parse("20150101", formatter)),
new Book("BookB", "Apple Bee", Arrays.asList("Comics"), LocalDate.parse("20160101", formatter)),
new Book("BookC", "Cake Door", Arrays.asList("Law"), LocalDate.parse("20151001", formatter)),
new Book("BookD", "Egg Frog", Arrays.asList("History"), LocalDate.parse("20150601", formatter)),
new Book("BookE", "Goose Horse", Arrays.asList("Education", "Art"),
						LocalDate.parse("20161001", formatter)),
new Book("BookF", "Ibis Jellyfish", Arrays.asList("Business","Art"), LocalDate.parse("20141001", formatter)));

```

### 進行過濾

```java
System.out.println(" :: getByAuthor (get book by Egg Frog) :: ");
List<Publication> booksByCake = getByAuthor.apply("Egg Frog", allBookList);
booksByCake.forEach( c -> System.out.println(c));

/*
 :: getByAuthor (get book by Egg Frog) :: 
Book [title=BookD, published=2015-06-01, author=Egg Frog, tags=[History]]
*/
```


```java
System.out.println(" :: getByTag (get books have 'Art' tag) :: ");
List<Publication> booksTagArt = getByTag.apply("Art", allBookList);
booksTagArt.forEach( c -> System.out.println(c));
/*
 :: getByTag (get books have 'Art' tag) :: 
Book [title=BookA, published=2015-01-01, author=Apple Bee, tags=[Art, Comics]]
Book [title=BookE, published=2016-10-01, author=Goose Horse, tags=[Education, Art]]
Book [title=BookF, published=2014-10-01, author=Ibis Jellyfish, tags=[Business, Art]]
*/
```

```java
System.out.println(" :: sortByDate (sort book by publish date desc):: ");
List<Publication> booksSortedByDate = sortByDate.apply(allBookList);
booksSortedByDate.forEach( c -> System.out.println(c));
/*
 :: sortByDate (sort book by publish date desc):: 
Book [title=BookE, published=2016-10-01, author=Goose Horse, tags=[Education, Art]]
Book [title=BookB, published=2016-01-01, author=Apple Bee, tags=[Comics]]
Book [title=BookC, published=2015-10-01, author=Cake Door, tags=[Law]]
Book [title=BookD, published=2015-06-01, author=Egg Frog, tags=[History]]
Book [title=BookA, published=2015-01-01, author=Apple Bee, tags=[Art, Comics]]
Book [title=BookF, published=2014-10-01, author=Ibis Jellyfish, tags=[Business, Art]]
*/
```

```java
System.out.println(" :: getFirst (get first book):: ");
Optional<Publication> firstBook = getFirst.apply(allBookList);	System.out.println(firstBook.get().toString());
/*
 :: getFirst (get first book):: 
Book [title=BookA, published=2015-01-01, author=Apple Bee, tags=[Art, Comics]]
*/
```

```java
System.out.println(" :: getNewest (get newest book):: ");
Optional<Publication> newestBook = getNewest.apply(allBookList)	System.out.println(newestBook.get().toString());
/*
 :: getNewest (get newest book):: 
Book [title=BookE, published=2016-10-01, author=Goose Horse, tags=[Education, Art]]
*/
```

```java
System.out.println(" :: byAuthorSortedByDate (get books by Apple Bee and order by publish date desc):: ");
List<Publication> booksByAppleBeeSortByDate = byAuthorSortedByDate.apply("Apple Bee", allBookList);
booksByAppleBeeSortByDate.forEach( c -> System.out.println(c));
/*
 :: byAuthorSortedByDate (get books by Apple Bee and order by publish date desc):: 
Book [title=BookB, published=2016-01-01, author=Apple Bee, tags=[Comics]]
Book [title=BookA, published=2015-01-01, author=Apple Bee, tags=[Art, Comics]]
*/
```

```java
System.out.println(" :: newestByAuthor (get Apple Bee's newest book):: ");
Optional<Publication> newestBookByAppleBee = newestByAuthor.apply("Apple Bee", allBookList);
System.out.println(newestBookByAppleBee.get().toString());
/*
 :: newestByAuthor (get Apple Bee's newest book):: 
Book [title=BookB, published=2016-01-01, author=Apple Bee, tags=[Comics]]
*/
```


```java
System.out.println(" :: newestByTag (get books have 'Art' tag and order by publish date desc):: ");
Optional<Publication> newestBookTagIsArt = newestByTag.apply("Art", allBookList);
System.out.println(newestBookTagIsArt.get().toString());
/*
 :: newestByTag (get books have 'Art' tag and order by publish date desc):: 
Book [title=BookE, published=2016-10-01, author=Goose Horse, tags=[Education, Art]]
*/
```


此篇筆記參考 [Java 8: Composing functions using compose and andThen](http://www.deadcoderising.com/2015-09-07-java-8-functional-composition-using-compose-and-andthen/)



gist：[functionComposeAndThen.java](https://gist.github.com/hatoto/18bdd7612a7bfcf49e909380c955d1b1)


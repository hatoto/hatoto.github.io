---
layout: post
title:  "Java8 Predicate 練習"
date:   2017-07-06
categories: java
tags: java
---

* content
{:toc}


使用 Java 8 的 Predicate，可以先針對特定class的過濾條件。
在需要使用不同條件篩選出list的結果時，可以重複套用，或組合套用。

### 測試資料

[Customers]


 LastName| Gender | Age | LastAmount | TotalBuyCount
------------| ---------| ------ | -----| ------
Bee     | M | 23 |10000.0 | 5
Horse   | F | 13 |300.0 | 2
Monkey  | M | 43 |100.0 | 1
Leopard | M | 23 |40000.0 | 10
Moose   | F | 19 |700.0 | 7
Frog    | M | 23 |2300.0 | 5
Rat     | M | 63 |9000.0 | 2
Goose   | M | 70 |4000.0 | 8
Seal    | F | 15 |670.0 | 1
Jaguar  | M | 45 |100000.0 | 39



### 建立Customer class

```java
class Customer {

	private Integer id;
	private Integer age;
	private String gender;
	private String firstName;
	private String lastName;
	private Double lastPurchaseAmount;
	private Integer totalPurchaseCount;
.
.
//尚有getter, setter....
}
```


### 建立用來過濾的predicate

建立Predicate時所用的型別是Customer, 因此在該method中，
寫 p -> 時，p 代表的即是Customer, 可由其中直接取出相關欄位來進行條件篩選。


* 已成年

```java
public static Predicate<Customer> isAdult() {
    return p -> p.getAge() > 18 ;
}
```

* 是男的


```java	
public static Predicate<Customer> isMale() {
    return p -> p.getGender().equalsIgnoreCase("M");
}
```
* 是女性

```java	
public static Predicate<Customer> isFemale() {
    return p -> p.getGender().equalsIgnoreCase("F");
}
```

* 年紀超過x歲

```java	
public static Predicate<Customer> isAgeMoreThan(Integer age) {
	    return p -> p.getAge() > age;
}
```	
	
* 總購買次數超過3次	
	
```java	
public static Predicate<Customer> purchaseTimesMoreThan3() {
	    return p -> p.getTotalPurchaseCount() > 3;
	}
```


### 建立進行過濾的method

建立 filterCustomers，回傳的型別是List<Customer>，也就過濾後的List，傳入的參數是未過濾的List<Customer>和Predicate。

```java
public static List<Customer> filterCustomers (List<Customer> customers, Predicate<Customer> predicate) {
        return customers.stream().filter( predicate ).collect(Collectors.<Customer>toList());
}
```

在method裡面，使用stream.filter 可把predicate直接帶入其中。

### 建立測試資料

```java
Customer c1 = new Customer(1,23,"M","BB","Bee", 10000.0, 5);
Customer c2 = new Customer(2,13,"F","HH","Horse", 300.0, 2);
Customer c3 = new Customer(3,43,"M","MM","Monkey", 100.0, 1);
Customer c4 = new Customer(4,23,"M","LL","Leopard", 40000.0, 10);
Customer c5 = new Customer(5,19,"F","MM","Moose", 700.0, 7);
Customer c6 = new Customer(6,15,"M","FF","Frog", 2300.0, 5);
Customer c7 = new Customer(7,63,"F","RR","Rat", 9000.0, 2);
Customer c8 = new Customer(8,70,"M","GG","Goose", 4000.0, 8);
Customer c9 = new Customer(9,15,"F","SS","Seal", 670.0, 1);
Customer c10 = new Customer(10,45,"M","JJ","Jaguar", 100000.0, 39);
         
List<Customer> Customers = new ArrayList<Customer>();
Customers.addAll(Arrays.asList(new Customer[]{c1,c2,c3,c4,c5,c6,c7,c8,c9,c10}));
```

### 進行過濾

```java
System.out.println("找出成年男性顧客：[isMale().and(isAdult())]");
List<Customer> adultMaleCustomer = filterCustomers(Customers, isMale().and(isAdult()) );
adultMaleCustomer.stream().forEach( s -> System.out.println(s));

/*
找出成年男性顧客：[isMale().and(isAdult())]
Age(23), Gender(M), lastName:[Bee], lastPurchaseAmount(10000.0), totalPurchaseCount(5)
Age(43), Gender(M), lastName:[Monkey], lastPurchaseAmount(100.0), totalPurchaseCount(1)
Age(23), Gender(M), lastName:[Leopard], lastPurchaseAmount(40000.0), totalPurchaseCount(10)
Age(70), Gender(M), lastName:[Goose], lastPurchaseAmount(4000.0), totalPurchaseCount(8)
Age(45), Gender(M), lastName:[Jaguar], lastPurchaseAmount(100000.0), totalPurchaseCount(39)
*/
```


```java
System.out.println("找出年齡大於25歲的顧客：[isAgeMoreThan(25)]");
List<Customer> ageMoreThanTwentyFiveCustomers = filterCustomers(Customers, isAgeMoreThan(25));
ageMoreThanTwentyFiveCustomers.stream().forEach( s -> System.out.println(s));
/*
找出年齡大於25歲的顧客：[isAgeMoreThan(25)]
Age(43), Gender(M), lastName:[Monkey], lastPurchaseAmount(100.0), totalPurchaseCount(1)
Age(63), Gender(F), lastName:[Rat], lastPurchaseAmount(9000.0), totalPurchaseCount(2)
Age(70), Gender(M), lastName:[Goose], lastPurchaseAmount(4000.0), totalPurchaseCount(8)
Age(45), Gender(M), lastName:[Jaguar], lastPurchaseAmount(100000.0), totalPurchaseCount(39)
*/
```

這邊找出年齡小於25歲的顧客，用的同樣是isAgeMoreThan(25)，但後面接了.negate()，這樣可以把條件反轉。
        
```java        
System.out.println("找出年齡小於25歲的顧客：[isAgeMoreThan(25).negate()]");
List<Customer> ageLessThanTwentyFiveCustomers = filterCustomers(Customers, isAgeMoreThan(25).negate());
ageLessThanTwentyFiveCustomers.stream().forEach( s -> System.out.println(s));
/*
找出年齡小於25歲的顧客：[isAgeMoreThan(25).negate()]
Age(23), Gender(M), lastName:[Bee], lastPurchaseAmount(10000.0), totalPurchaseCount(5)
Age(13), Gender(F), lastName:[Horse], lastPurchaseAmount(300.0), totalPurchaseCount(2)
Age(23), Gender(M), lastName:[Leopard], lastPurchaseAmount(40000.0), totalPurchaseCount(10)
Age(19), Gender(F), lastName:[Moose], lastPurchaseAmount(700.0), totalPurchaseCount(7)
Age(15), Gender(M), lastName:[Frog], lastPurchaseAmount(2300.0), totalPurchaseCount(5)
Age(15), Gender(F), lastName:[Seal], lastPurchaseAmount(670.0), totalPurchaseCount(1)
*/
```

這邊沒有傳入predicate，而是直接傳入s -> s.getLastPurchaseAmount() > 10000。

```java
System.out.println("找出最後一次購金額超過10000的顧客：[s -> s.getLastPurchaseAmount() > 10000]");
List<Customer> lastPurchaseAmountMoreThan10000 = filterCustomers(Customers, s -> s.getLastPurchaseAmount() > 10000);
lastPurchaseAmountMoreThan10000.stream().forEach( s -> System.out.println(s));
/*
找出最後一次購金額超過10000的顧客：[s -> s.getLastPurchaseAmount() > 10000]
Age(23), Gender(M), lastName:[Leopard], lastPurchaseAmount(40000.0), totalPurchaseCount(10)
Age(45), Gender(M), lastName:[Jaguar], lastPurchaseAmount(100000.0), totalPurchaseCount(39)
*/
```




gist：[JavaPredicate.java](https://gist.github.com/hatoto/210ab51747a2f85081ebcf014461c6b8)


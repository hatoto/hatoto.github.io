---
layout: post
title:  "Design Pattern 責任鏈模式"
date:   2017-08-11
categories: design-patterns
tags: java design-patterns
---

* content
{:toc}


## 責任鏈模式

想像一支手機製作完成時要進行產品檢查，手機放在輸送帶上運送，在檢查線上每個檢查站口的責任就是檢查一個項目，有該站口可以處理的情況時，就進行補強，若問題太多就丟出去重新做。

這邊用檢查電話號碼來套用責任鏈模式。題目是從[Exercism.io: Phone Number in Java](http://exercism.io/exercises/java/phone-number/readme)來的。

![螢幕快照 2017-08-11 下午3.12.32.JPG]({{ site.url }}/assets/images/826ECC5A9FCC3ED36568644FFD2BDF93.jpg)

## 替換及檢核規則
* 傳入的內容要把(, ), -, 空白都去掉
* 去掉符號的內容長度只能是10或11位
* 內容只是11位時，第一個字必須是1，不是則拋錯
* 內容只是11位時，回傳時要把第一個字去掉
* 內容含有非數字和合法字符時拋錯

## 檢查器
依據上述的規則，設置了`DigitChecker`、`StartWithOneChecker`、`ValidCharacterChecker`三個檢查器，當作檢查鏈上的三個站口。

上面三個class都繼承了`ResultChecker`

```java
/**
 * 檢查器抽象父類別
 */
abstract class ResultChecker {
	protected ResultChecker nextChecker;

	public ResultChecker(ResultChecker nextChecker) {
		this.nextChecker = nextChecker;
	}
	/**
	 * 繼承此類別的檢查器需實做此method，若該類別中存在ResultChecker，
	 * 則發動該checker的doCheck
	 */
	abstract public String doCheck(String resultString);
}
```



## 替換及檢核步驟


### 用PhoneNumberWithPattern來傳入要檢查的號碼

```java
public class PhoneNumberWithPattern {

	private String regexValidSymbol = "[\\+|\\(|\\)|\\-\\.\\s]";
	private String result = "";
	
	public PhoneNumberWithPattern(String string) {
	
	  Pattern pattern = Pattern.compile(regexValidSymbol);
      Matcher matcher = pattern.matcher(string);
      this.result = matcher.replaceAll("");
        
      ResultChecker startWithOneChecker = new StartWithOneChecker(null);
      ResultChecker digitChecker = new DigitChecker(startWithOneChecker);
      ResultChecker validCharacterChecker = new ValidCharacterChecker(digitChecker);
        
      result = validCharacterChecker.doCheck(result);
	}

	public String getNumber() throws IllegalArgumentException{
		return this.result;
	}

}
```

在`PhoneNumberWithPattern`建構時先把可替換的符號替換掉
```java
Pattern pattern = Pattern.compile(regexValidSymbol);
Matcher matcher = pattern.matcher(string);
this.result = matcher.replaceAll("");
```

### 把替換後的字串放到檢查鏈中檢查

檢查的順序如下：
1. 內容只能有數字、空白、小括號、點 -> ValidCharacterChecker.java
2. 內容長度只能是10或11位 -> DigitChecker.java
3. 內容長度11位時，第一個字必須是1 -> StartWithOneChecker.java

由於上面3個檢查器都有繼承抽象父類別，因此需要各自實作`public String doCheck(String resultString)`這個method。
在這個method中，大家都會檢查是否有`ResultChecker nextChecker`(在父類裡)，有的話會做`nextChecker.doCheck(resultString);`。

所以宣告檢查器的順序，會和執行檢查時的順序相反。
```java
//StartWithOneChecker 後面沒有檢查器了，所以建構時傳入null
ResultChecker startWithOneChecker = new StartWithOneChecker(null);
ResultChecker digitChecker = new DigitChecker(startWithOneChecker);
//ValidCharacterChecker 是第一個檢查器，接下來是DigitChecker，所以建構時傳入DigitChecker
ResultChecker validCharacterChecker = new ValidCharacterChecker(digitChecker);
//執行 validCharacterChecker.doCheck(.....) 開始檢查 
result = validCharacterChecker.doCheck(result);
```

## 測試

### 測試程式
```java
public class PhoneNumberWithPatternTest {
    
    public static void main(String args[]){
    	String returnNumber;
    	
    	System.out.println("A. 單純符號替換");
    	returnNumber = new PhoneNumberWithPattern("(123) 456-7890").getNumber();
    	System.out.println("預期:1234567890\n回傳:"+returnNumber);
    	System.out.println("----------------------------------");    	

    	try {
    		System.out.println("B. 內容長度錯誤");
			new PhoneNumberWithPattern("123456789");
		} catch (Exception e) {
			System.out.println("錯誤訊息:"+e.getMessage());
		}
    	System.out.println("----------------------------------"); 
    	
    	
    	try {
    		System.out.println("C. 內容長度11位，但第一個字不是1");
    		new PhoneNumberWithPattern("21234567890");
		} catch (Exception e) {
			System.out.println("錯誤訊息:"+e.getMessage());
		}
    	System.out.println("----------------------------------");
    	
    	System.out.println("D. 內容長度11位[11234567890]，移除第一位");
    	returnNumber = new PhoneNumberWithPattern("11234567890").getNumber();
    	System.out.println("預期:1234567890\n回傳:"+returnNumber);
    	System.out.println("----------------------------------");   
    	

    	try {
    		System.out.println("E. 內容有不合條件的字元");
    		new PhoneNumberWithPattern("123-abc-7890");
		} catch (Exception e) {
			System.out.println("錯誤訊息:"+e.getMessage());
		}
    	System.out.println("----------------------------------");
    }
}
```
### 測試結果

A. 單純符號替換
預期:1234567890
回傳:1234567890

B. 內容長度錯誤
錯誤訊息:內容只能是10或11位

C. 內容長度11位，但第一個字不是1
錯誤訊息:11位時，第一個字必須是1
   
D. 內容長度11位[11234567890]，移除第一位
預期:1234567890
回傳:1234567890

E. 內容有不合條件的字元
錯誤訊息:內容只能有數字、空白、小括號、點


gist：[PhoneNumberWithPattern.java](https://gist.github.com/hatoto/53d7a006549c2d871425d76cb89317e2)


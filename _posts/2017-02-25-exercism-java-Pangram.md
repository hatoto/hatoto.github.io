---
layout: post
title:  "程式練習 - java - 全字母句Pangram 驗證"
date:   2017-02-25
categories: exercism
tags: exercism java
---

* content
{:toc}


[**全字母句Pangram**](https://zh.wikipedia.org/wiki/%E5%85%A8%E5%AD%97%E6%AF%8D%E5%8F%A5) ： 包含有字母表中所有字母並且言之成義的句子稱為全字母句。

例如：The quick brown fox jumps over the lazy dog

此練習是檢查某一句子是否為Pangram。

對語言的熟悉度和解題邏輯會影響程式寫法和效率。

多看其他人的寫法才會能寫出比較好的解法。

## 解題方式

方式A.

- 1) 建立一個a-z的Map<String, Integer> 預設每個字母數量0。 
- 2) 檢查傳入字串的每個字元，若該字元與Map的字母比對到時，Map中的字母數量加1   
- 3) 依序檢查Map中每個字母的數量，有任一字母記數為零時傳回false

方式B. 
 
- 1) 做一個空Set      
- 2) 檢查傳入字串的每個字元，若該字元為 a-z 或 A-Z 則把該字塞入Set裡
- 3) 查看set的size是26時該輸入字句為 Pangram

方式C. 

- 1) 把輸入的句子用regex 替換掉不是a-zA-Z的字元
- 2) 建立一個alphabet = “abcdefghijklmnopqrstuwvxyz” 的字串   
- 3) 依序查看每個輸入的字串字元是否有在alphabet中，如果對到了就把alphabet中的字換成空白   
- 4) 最後查看alphabet的長度，如果是0表示每個字都被換掉，輸入的句子是Pangram


### 方式A
```java
public static boolean isPangram(String input){
		
		if( null == input || "".equals(input)){
			return false;
		}
		
		char[] alphabet = "abcdefghijklmnopqrstuvwxyz".toCharArray();
		Map<String, Integer> asciiCountMap = new HashMap<String, Integer>();
		for (char c : alphabet) {
			asciiCountMap.put(c+"", 0);
		}
		
		Set<String> keySet = asciiCountMap.keySet();
		boolean isInputPangram = true;
		
		char[] inputAsCharArr = input.toCharArray();
		for (char c : inputAsCharArr) {
			if( c != ' '){
				for (String string : keySet) {
					if( string.equalsIgnoreCase(new Character(c).toString()) ){
						Integer inte = asciiCountMap.get(string);
						inte += 1;
						asciiCountMap.put(string, inte);
					}
				}
			}
		}
		for (String string : keySet) {
			if( asciiCountMap.get(string) == 0 ){
				isInputPangram = false;
				break;
			}
		}
		return isInputPangram;
	}
```


很長的解法，而且效能很差。

### 方式B

~~~java
public static Boolean isPangram(String input) {
        if (input == null || "".equals(input)) {
            return false;
        }
        Set<Character> m = new HashSet<Character>();
        int len = input.length();
        for (int i = 0; i < len; i++) {
            char c = input.charAt(i);
            if (c >= 'a' && c <= 'z') {
                m.add(c);
            }
            if (c >= 'A' && c <= 'Z') {
                m.add(Character.toLowerCase(c));
            }
        }
        return m.size() == 26;
    }
~~~

不常用char，所以沒能直接用字母去比較，
這個解法自己還蠻喜歡。



### 方式C



```java

public static boolean isPangram(String input) {
        if (null == input || input.length() == 0){
        	return false; 
        }
        String lowerCaseInput = input.toLowerCase().replaceAll("[^a-z]","");
        String alphabet = "abcdefghijklmnopqrstuwvxyz";
        for (int i = 0; i < lowerCaseInput.length(); i++) {
            String letter = Character.toString(lowerCaseInput.charAt(i));
            alphabet = alphabet.replaceAll(letter, "");
        }
        if (alphabet.length() == 0) { return true; }
        return false;
 }


```


這個解答方式是把正向統計每個字元次數的方式，改成替換掉字元的方式。
  
  



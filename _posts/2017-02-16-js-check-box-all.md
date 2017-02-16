---
layout: post
title:  "jQuery checkbox 全選"
date:   2017-02-16
categories: javascript
tags: javascript jQuery
---

* content
{:toc}

有時候看到感覺比較進階(沒有辦法一下子看懂)的javascript寫法時，會覺得*哇!原來可以這樣寫*或*這樣好像比較厲害*。

不過寫code還是簡潔易懂會比較好。

### 自己覺得看起來厲害的寫法

- [ ] check all type1
- [ ] 001
- [ ] 002
- [ ] 003
- [ ] 004

```html
<form id="contextForm" class="form" >
<input type="checkbox" id="checkAll" onclick="checkAllDetail();" />check all type1
<ul>
  <li><input type="checkbox" name="id" value="001" />001</li>
  <li><input type="checkbox" name="id" value="002" />002</li>
  <li><input type="checkbox" name="id" value="003" />003</li>
  <li><input type="checkbox" name="id" value="004" />004</li>
</ul>
</form>
```


~~~javascript
function checkAllDetail() {
    loopDetail(function(selector) {
        selector.prop('checked', $('#checkAll').prop('checked'));
    });
}
function loopDetail( aFunction ) {
    if ( aFunction ) {
      $( "input[name='id']", $('#contextForm') ).each(function() {
           aFunction( $(this) );
      });
    }
}
~~~


上面的checkbox 點選 check all type1 時呼叫`checkAllDetail()`這個function，然後在這function中再去呼叫`loopDetail( function parameter )`。

剛開始看覺得程式碼大約10行，還蠻短的，不過卻沒能馬上讀懂，覺得很厲害。

但是其實這樣寫真的很多餘。




### 一行的寫法

- [ ] check all type2
- [ ] 00A
- [ ] 00B
- [ ] 00C
- [ ] 00D


```html
<form id="contextForm2" class="form" >
<input type="checkbox" onclick="checkAll(this);" />check all type2
<ul>
  <li><input type="checkbox" name="id" value="00A" />00A</li>
  <li><input type="checkbox" name="id" value="00B" />00B</li>
  <li><input type="checkbox" name="id" value="00C" />00C</li>
  <li><input type="checkbox" name="id" value="00D" />00D</li>
</ul>
</form>
```

~~~javascript
function checkAll(obj){
  $( "input[name='id']",  $('#contextForm2')).prop('checked',$(obj).prop('checked'));
}
~~~

上面的checkbox 點選 check all type2 時呼叫`checkAll(obj)`這個function，直接用jQuery selector 去拿相關的checkbox並直接用.prop的方式設置每個checkbox的check狀態，根本不需要再多寫個function和跑迴圈。  
  


結論是，感覺厲害的寫法不一定比較好，讓人容易懂的程式會比較OK。

  
  
  
   
    


<p data-height="265" data-theme-id="0" data-slug-hash="QdPjMY" data-default-tab="html,result" data-user="hatoto" data-embed-version="2" data-pen-title="QdPjMY" class="codepen">See the Pen <a href="https://codepen.io/hatoto/pen/QdPjMY/">QdPjMY</a> by martypan (<a href="http://codepen.io/hatoto">@hatoto</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>







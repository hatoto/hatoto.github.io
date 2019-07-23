---
layout: post
title:  "Card Match Game with Vue"
date:   2019-07-23
categories: javascript
tags: javascript vue 
---

* content
{:toc}

### 功能說明

翻牌配對記憶遊戲，使用Vue製作

### 遊戲畫面


![card_match_2019-07-23 16.43.24.JPG]({{ site.url }}/assets/images/card_match_2019-07-23 16.43.24.JPG)

### 遊戲功能

- 可隨時重置遊戲
- 三種難度模式選擇
- 紀錄各模式最快完成時間



### 使用技術

- 卡片翻轉，使用CSS transform 做成卡片翻面效果：[Intro to CSS 3D transforms](https://3dtransforms.desandro.com/card-flip)
- Vue.js，使用Vue渲染卡片，以及卡片點擊事件控制：[Vue.js](https://vuejs.org/)
- 切換開關，難易度的設定開關樣式：[CSS TOGGLE SWITCH](https://ghinda.net/css-toggle-switch/)


### 使用Vue的好處

本來以為用Vue要和Angular一樣建立一些複雜的套件設定，沒想到在CodePen上面加入一個js就可以用了，實在太方便了。

用framework的好處就是不用手動寫很多selector，也省了很多控制事件的步驟。

### CodePen
<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="js,result" data-user="hatoto" data-slug-hash="wemJaa" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="card match game vue">
  <span>See the Pen <a href="https://codepen.io/hatoto/pen/wemJaa/">
  card match game vue</a> by martypan (<a href="https://codepen.io/hatoto">@hatoto</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>





gist：[card-match-game-vue](https://gist.github.com/hatoto/c1d49fcb20a75b8e3e461e3c893c78d9)
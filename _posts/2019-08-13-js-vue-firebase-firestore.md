---
layout: post
title:  "使用Vue和Firebase做一個網站"
date:   2019-08-13
categories: javascript
tags: javascript vue firebase 
---

* content
{:toc}

### 小遊戲

原先在[CodePen](https://codepen.io/)上面用Vue.js練習做了
[翻牌配對記憶遊戲](https://codepen.io/hatoto/pen/wemJaa/)、[撲克牌記憶遊戲](https://codepen.io/hatoto/pen/YbezZK)。

後來再用Vue CLI做一個TypeScript版本的Vue專案，把兩個遊戲都放在這個網站裡。

網站寫好後把整個內容部署到有免錢額度的FireBase，這樣就有一個算是Demo小作品的地方了。

在FireBase上的網站：[Web](https://modontoys.web.app/)

### FireBase的功能

FireBase除了有置放靜態網頁的Hosting功能外，還有Database、Storage、Functions等等等等服務。

目前這個小遊戲網站會用到的就是Database的功能了，用來記錄每次遊戲結束後花費的時間，還有呈現遊戲紀錄。

### Cloud Firestore

Database有分兩種，Cloud Firestore和Realtime Database。

這次用的是Cloud Firestore，Firestore的資料格式就像json一樣

```json
//你的資料庫
{
  "Schema1":{
    "Schema1_Table1":[
      {
       "columnA": "value",
       "columnB": "value"
      }...
    ],
    "Schema1_Table2":[]
  },
  "Schema2":{
    "Schema2_Table1":[],
    "Schema2_Table2":[]
  }
}
```


這比Cloud Datastore好懂很多，以前用Cloud Datastore時，雖然有操作過資料，但其實規則還是很難搞懂。




### 在Vue裡面操作Cloud Firestore

在Vue專案裡面install firebase package，就可以直接操作Firebase Realtime Database, 
Cloud Firestore, Firebase Storage, 
Firebase Cloud Messaging,Firebase Authentication這些功能。

```bash
npm install --save firebase
```

在專案中要使用FireBase API，需要提供自己在FireBase上設定的專案相關資料進行初始設定，
需要的內容就是`FirebaseConfig`裡的欄位內容，這些內容在FireBase的管理頁面中於設定APP時就可取得。


```typescript
import firebase from 'firebase/app';
import 'firebase/firestore'

export interface FirebaseConfig
{
  apiKey: string,
  authDomain: string,
  databaseURL: string,
  projectId: string,
  storageBucket: string,
  messagingSenderId: string,
  appId: string
}

var firebaseConfig : FirebaseConfig = require('./firebaseConfigData.json');
const firebaseApp = firebase.initializeApp(firebaseConfig);
export default firebaseApp.firestore();
```

設定後就可以直接在程式裡查詢和新增遊戲紀錄了。


Code on GitHub：[mini-apps-with-vue](https://github.com/hatoto/mini-apps-with-vue)


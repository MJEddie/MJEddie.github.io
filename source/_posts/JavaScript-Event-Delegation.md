---
title: "[筆記] JavaScript - Event Delegation ( 事件委派 )"
date: 2021-04-17 08:13:45
tags: JavaScript
---
![](/uploads/note.jpg)

## 什麼是 Event Delegation？

Event Delegation ( 事件委派 ) 是一種受惠於 Event Bubbling ( 事件冒泡 ) 而能減少監聽器的方法。
<!-- more -->
## 為什麼要用事件委派

在 JavaScript 中，添加到頁面上的事件處理程序數量將直接關係到頁面的整體運行性能，如果我們想要取得 `ul` 中每個 `li` 的內容，而對每個 `li` 進行監聽，當數量越來越多時，瀏覽器也就建立了越來越多的監聽器，無形中對效能有很大的傷害，此時我們可以透過事件委派直接監聽父層的 `ul`，無論內容有多少，瀏覽器都只需要負擔一組事件監聽器的消耗。

#### 範例一

使用 `jQuery` API

**語法**
```javascript=
// jQuery 1.4.3+
$( elements ).delegate( selector, events, data, handler );
// jQuery 1.7+
$( elements ).on( events, selector, data, handler );
```

* `selector`：選擇器，用來過濾觸發事件的元素。
* `events`：JavsScript 事件類型，如 `click`、`keydown`等。
* `data`：規定傳遞到函數的額外數據，可不填寫。
* `hundle`：觸發事件時要執行的功能。

**HTML**

```html
<ul id="ul">
  <li class="sub-item">000</li>
  <li class="list-item">123</li>
  <li class="list-item">456</li>
  <li class="list-item">789</li>
</ul>
```
**JavaScript**
```javascript=
// Event Delegation
$('#ul').click(function(e){
  console.log($(e.target).text());
  })
  
// jQuery delegate
$('#ul').on('click','li',function(){
  console.log($(this).text());
})
```

#### 範例二

只監聽 `li` 中含有 `list-item` class 名稱的項目，點擊 `sub-item` 這個 class 的 `li` 時就不會有動作。

**JavaScript**
```javascript=
$('#ul').click(function(e){
  console.log($(e.target).text());
  })
  
// jQuery delegate
$('#ul').on('click','.list-item',function(){
  console.log($(this).text());
})
```

#### 範例三

除了可以對加載完成的現有 DOM 節點進行操作外，也可以監聽動態新增的節點。新增一個按鈕，點擊會新增一個 `li` 項目，點擊這個新增的項目時，一樣可以觸發 function。

**HTML**

```html
<ul id="ul">
  <li class="list-item">123</li>
  <li class="list-item">456</li>
  <li class="list-item">789</li>
  <li class="sub-item">000</li>
</ul>

<button id="btn">新增清單</button>
```
**JavaScript**
```javascript=
// Event Delegation
$('#ul').click(function(e){
  console.log($(e.target).text());
  })
  
// jQuery delegate
$('#ul').on('click','li',function(){
  console.log($(this).text());
})

// add list
$('#btn').click(function(){
  $('#ul').append('<li>012</li>');
});
```

## 參考資料
* [jQuery API Document](https://api.jquery.com/delegate/)
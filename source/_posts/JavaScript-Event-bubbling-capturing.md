---
title: "[筆記] JavaScript - Event Bubbling & Capturing ( 事件冒泡與事件捕獲 )"
date: 2021-04-16 07:25:19
tags: JavaScript
---
![](/uploads/note.jpg)

JavaScript 是一個事件驅動 (Event-driven) 的程式語言，當瀏覽器載入網頁開始讀取後，雖然馬上會讀取 JavaScript 事件相關的程式碼，但是必須等到「事件」被觸發(如使用者點擊、按下鍵盤等)後，才會再進行對應程式的執行。
<!-- more -->
## DOM 的事件流程 (Event Flow)

DOM 的事件機制分為三個階段：
* 捕獲階段 (Capture Phase)：在網頁上點擊一個元素時，這個點擊事件會從 `windos` 開始往下尋找，一直到點擊的元素 (target)，這個階段稱作捕獲階段。
* 目標階段 (Target Phase)：在找到目標時，就會是目標階段。**當事件傳到 target 本身，沒有分捕獲跟冒泡**。
* 冒泡階段 (Bubbling Phase)：在事件捕獲到後，會一路傳回去 `windows`，這個時候稱作冒泡階段。

用張大家都很熟悉的 [w3c - event-flow](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow) 圖來說明這三個階段：

![](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)

**捕獲階段**
當點擊 `td` 時，這個點擊事件會從 `windows` 往下一直找到 `td` 為止。

**目標階段**
當找到目標 `td` 時。

**冒泡階段**
當捕獲到目標後，一路傳回去 `windows`。

>也因此會常常聽到所謂的口訣，**「先捕獲，再冒泡」**。

## 事件捕獲 (Event capturing)

接下來我們用 `addEventLinstener` 來接聽事件的階段，並且回傳事件階段 (eventPhase) 看一下實際是不是跟我們想像的一樣。

**語法**
```javascript=
target.addEventListener(type, listener, useCapture);
```
`useCapture`：

* `true`：監聽捕獲階段。
* `false`：監聽冒泡階段，一般預設為 `false`。

`eventPhase` 定義：

* `CAPTURING_PHASE`：捕獲階段，回傳值為 `1`。
* `AT_TARGET`：目標階段，回傳值為`2`。
* `BUBBLING_PHASE`：冒泡階段，回傳值為`3`。

**範例**

**HTML**

```html=
<body>
  <ul id="ul">
    <li id="li">
      <a id ="a" target="_blank" href="http://google.com">
        click me
      </a>
    </li>
  </ul>
</body>
```
**JavaScript**

```javascript=
const ul = document.getElementById('ul');
const li = document.getElementById('li');
const a = document.getElementById('a');

// ul 的捕獲
ul.addEventListener('click', (event) => {
  console.log('ul capturing', event.eventPhase);
}, true)

// li 的捕獲
li.addEventListener('click', (event) => {
  console.log('li capturing', event.eventPhase);
}, true)

// a 的捕獲
a.addEventListener('click', (event) => {
  console.log('a capturing', event.eventPhase);
}, true)
```

結果為：

```javascript=
"ul capturing" 1
"li capturing" 1
"a capturing" 2
```

可以看到點擊 `a` 時，是由最上層 `ul` > `li` > `a` 一直傳遞到 target `a`。

## 事件冒泡 (Event bubbling)

一樣用剛才的範例，把監聽的參數改為 `false`。

```javascript=
// ul 的冒泡
ul.addEventListener('click', (event) => {
  console.log('ul bubbling', event.eventPhase);
}, false)

// li 的冒泡
li.addEventListener('click', (event) => {
  console.log('li bubbling', event.eventPhase);
}, false)

// a 的冒泡
a.addEventListener('click', (event) => {
  console.log('a bubbling', event.eventPhase);
}, false)
```
結果為：

```javascript=
"a bubbling" 2
"li bubbling" 3
"ul bubbling" 3
```

從 targe `a` > `li` > `ul` 一路冒泡回去，其中可以看到 target 的狀態皆為 `AT_TARGET`，因此我們知道 target 本身，沒有分捕獲跟冒泡。

## 取消事件傳遞

當我們想要阻止事件的冒泡時，可以用 `event.stopPropagation()` 來停止傳遞。依照 `event.stopPropagation()` 加在哪一階段，事件傳遞就會終止在該階段，不繼續傳遞下去。

>這邊指的「事件傳遞被終止」，是說不會再把事件傳遞給「下一個節點」，但若是在同一個節點上有不只一個 listener，還是會被執行到。

以冒泡的例子來說，加在 `a` 連結的冒泡階段：

```javascript=
// a 的冒泡
a.addEventListener('click', (event) => {
  event.stopPropagation();
  console.log('a bubbling', event.eventPhase);
}, false)

// a 的冒泡 2
a.addEventListener('click', (event) => {
  console.log('a bubbling 2', event.eventPhase);
}, false)
```

結果就只有：
```javascript=
"a bubbling" 2
"a bubbling 2" 2
```

想要同一層級的監聽不被執行，可以使用 `e.stopImmediatePropagation()`。

```javascript=
// a 的冒泡
a.addEventListener('click', (event) => {
  event.stopImmediatePropagation();
  console.log('a bubbling', event.eventPhase);
}, false)

// a 的冒泡 2
a.addEventListener('click', (event) => {
  console.log('a bubbling 2', event.eventPhase);
}, false)
```
結果就只有：
```javascript=
"a bubbling" 2
```

### 取消預設行為

`event.preventDefault()` 為取消瀏覽器的預設行為，像是超連結、表單中的 `submit` 等，讓其點擊時不會執行預設的動作，因此加上 `event.preventDefault()` 雖然預設動作會取消，但事件還是會往下傳遞。


```javascript=
// ul 的冒泡
ul.addEventListener('click', (event) => {
  console.log('ul bubbling', event.eventPhase);
}, false)

// li 的冒泡
li.addEventListener('click', (event) => {
  console.log('li bubbling', event.eventPhase);
}, false)

// a 的冒泡
a.addEventListener('click', (event) => {
  event.preventDefault();
  console.log('a bubbling', event.eventPhase);
}, false)
```

結果為：

```javascript=
"a bubbling" 2
"li bubbling" 3
"ul bubbling" 3
```

>一旦 call 了 `event.preventDefault()`，在之後傳遞下去的事件裡面也會有效果。

加在 `ul` 的捕獲事件中，讓其之後一直傳遞，也能讓超連結停止預設動作。

```javascript=
ul.addEventListener('click', (event) => { 
  event.preventDefault();
  console.log('ul bubbling', event.eventPhase);
}, true)
```

## 參考資料
* [w3c - UI Events](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow)
* [重新認識 JavaScript: Day 14 事件機制的原理](https://ithelp.ithome.com.tw/articles/10191970)
* [DOM 的事件傳遞機制：捕獲與冒泡](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
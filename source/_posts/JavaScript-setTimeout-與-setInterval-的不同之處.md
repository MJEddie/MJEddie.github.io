---
title: "JavaScript - setTimeout() 與 setInterval() 的不同之處"
date: 2020-12-27 20:45:49
tags: JavaScript
---
![](/uploads/note.jpg)
## `setTimeout()` 的用法

根據 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout) 定義 `setTimeout()` 用法如下
<!-- more -->
```javascript=
var timeoutID = scope.setTimeout(function[, delay, param1, param2, ...]);
var timeoutID = scope.setTimeout(function[, delay]);
var timeoutID = scope.setTimeout(code[, delay]);
```

* timeoutID : `timerID`，為一個正整數，表示定時器的編號，可以傳遞給 `clearTimeout()` 來取消定時器
* `function | code` : 要執行的函數或代碼
* `delay` : 執行前的延遲，默認為 0，單位是 ms
* `param1`、`param2` : 該函數的參數 


**範例**

`myFunction()` function 會在一秒後執行

```javascript=
function myFunction() {
  alert('Hello');
}

setTimeout(myFunction, 1000);
```

**使用 `clearTimeout()` 取消**

我們用 `clearTimeout()` 來取消剛剛範例中的函式，並且把 `timerID` 印出來，會發現還是同個 ID，並沒有因為取消而變成 `null`。

```javascript=
function myFunction() {
  alert('Hello');
}

myVar = setTimeout(myFunction, 1000);
console.log(myVar);    // timer ID
clearTimeout(myVar);
console.log(myVar);    // 同個 timer ID
```


## `setInterval()` 的用法


根據 [MDN](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval) 定義 `setInterval()` 用法如下

```javascript=
var intervalID = scope.setInterval(func, [delay, arg1, arg2, ...]);
var intervalID = scope.setInterval(function[, delay]);
var intervalID = scope.setInterval(code, [delay]);
```

* timeoutID : `timerID`，為一個非零的數字，表示定時器的編號，可以傳遞給 `clearInterval()` 來取消定時器
* `function | code` : 要執行的函數或代碼
* `delay` : 每次執行的間隔，單位是 ms
* `param1`、`param2` : 該函數的參數 

**範例**

每間格半秒會執行 `myCallback` function，`myCallback` function 則會把字串 `Parameter 1`、`Parameter 2` 印出來。

```javascript=
var intervalID = window.setInterval(myCallback, 500, 'Parameter 1', 'Parameter 2');

function myCallback(a, b)
{
 // Your code here
 // Parameters are purely optional.
 console.log(a);
 console.log(b);
}
```

**使用 `clearInterval()` 取消**

將 `timerID` 傳遞給 `clearInterval()` 取消 `setInterval()`。

```javascript=
var intervalID = window.setInterval(myCallback, 500, 'Parameter 1', 'Parameter 2');

function myCallback(a, b)
{
 // Your code here
 // Parameters are purely optional.
 console.log(a);
 console.log(b);
}

clearInterval(intervalID);
```


## 總結

* `setTimeout()` : 是在延遲一段時間後，執行「一次」指定的程式碼
* `setInterval()` : 固定延遲一段時間後，執行對應的程式碼，然後「不斷循環」

兩者都會回傳一個獨立的 `timerID`，該 `timerID` 傳給 `clearTimeout()` 和 `clearInterval()` 用來取消 `setTimeout()` 和 `setInterval()` 的執行。


## 參考資料

* [JavaScript Timing Event](https://www.w3schools.com/js/js_timing.asp)
* [MDN](https://developer.mozilla.org/en-US/search?q=WindowOrWorkerGlobalScope)
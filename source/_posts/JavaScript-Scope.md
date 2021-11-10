---
title: "JavaScript - Scope ( 作用域 )"
date: 2021-04-15 17:24:56
tags: JavaScript
---
![](/uploads/note.jpg)
## 什麼是 Scope？

[w3c](https://www.w3schools.com/js/js_scope.asp) 的定義：**「Scope determines the accessibility (visibility) of variables.」**
<!-- more -->
Scope ( 作用域 ) 決定了變數可以被取用 ( 可視 ) 的範圍，其中又分為 Global Scope ( 全域作用域 ) 跟 Local Scope ( 區域作用域 )。

## Global Scope ( 全域作用域 )

在函式或區塊 `{}` 外宣告的變數，可以在全域使用，又稱**全域變數**。

```javascript=
var carName = "Volvo";
myFunction(); 

function myFunction() {
  console.log(carName);    // Volvo
}

console.log(carName);    // Volvo
```
`myFunction()` 內找不到 `carName` 就會到外面找，直到全域變數為止。
> 若是在自己層級找不到就會一層一層往外找，直到 Global 為止。這種行為，稱之為「範圍鏈」(Scope Chain)。

除了在外層宣告變數外，如果賦值給未宣告的變數會變成全域變數，要避免這種寫法。

```javascript=
function myFunction() {
  carName = "Volvo";
}
myFunction();
console.log(carName);    // Volvo
```
>要先執行 `myFunction()` 才宣告 `carName` 這個變數，因此沒有執行的話就 `console` 會拋出錯誤。

## Local Scope ( 區域作用域 )

在函式或是區塊內宣告的變數，只能在函式或是區塊中使用，又稱**區域變數**。

```javascript=
myFunction();

function myFunction() {
  var carName = "Volvo";
  console.log(carName);    // Volvo
}
console.log(carName);    // not defined
```

## `const` 和 `let`

### `const`

常數宣告，有以下特性：
* 宣告後就不能改變值，因此宣告時一定要設值
* Block-Level Scope ( 塊級作用域 )
* 沒有 Hoising ( 變數提升 )
* Temporal Dead Zone, TDZ ( 暫時死區 )：宣告後才能使用
* 不允許重複宣告

### `let`

變數宣告，和 `var` 類似，但有以下特性
* Block-Level Scope ( 塊級作用域 )
* 沒有 Hoising ( 變數提升 )
* Temporal Dead Zone, TDZ ( 暫時死區 )：宣告後才能使用
* 不允許重複宣告

### Block-Level Scope ( 塊級作用域 )

JavaScript 只有 global 和 function 兩種 scope，意即若使用 var 與 function 做變數宣告，只能將變數的活動範圍限制在全域或函數中，這可能會產生非預期的結果。

```javascript=
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i);
  };
}
a[6]();    // 10
```
使用 `var` 宣告 `i` 當成 for loop 的 counter，當 `i` 離開 for loop 後，仍可被存取，因此得到 `a[6]()` 為 `10`。

ES6 解決了這樣的問題，`let` 、`const` 有 block-level scope 特性，離開大括號 `{…}` 的範圍後便失效。

```javascript=
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i);
  };
}
a[2]();    // 2
```

## 參考資料

* [w3c - JavaScript Scope](https://www.w3schools.com/js/js_scope.asp)
* [重新認識 JavaScript: Day 10 函式 Functions 的基本概念](https://ithelp.ithome.com.tw/articles/10191549)
---
title: "[筆記] JavaScript - Closure ( 閉包 )"
date: 2021-04-20 10:14:17
tags: JavaScript
---
![](/uploads/note.jpg)

## 什麼是 Closure？

[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Closures)的定義：**閉包（Closure）是函式以及該函式被宣告時所在的作用域環境（lexical environment）的組合。**
<!-- more -->
作用域環境簡單地說，就是函式被宣告時所在的 Scope，這個 Scope 裡面包含了能夠被這個函式存取到的變數，詳細可參考 [JavaScript - Scope ( 作用域 )](https://mjeddie.github.io/2021/04/15/JavaScript-Scope/) 中提到的 Scope Chain，因此 Closure 就是一個函式能夠存取自己被宣告時的環境中的變數。

## Closure 的特性

### Closure 可以「保留」環境

**範例一**

```javascript=
function makeFunc() {
  var name = "Nick";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();    // "Nick"
```
在某些程式語言，函式內的局部變數，只會在函式的執行期間存在。當 `makeFunc()` 執行完，`name` 變數再也無法使用。

>在JavaScript中，即使在外層區塊已經回傳的狀況下，只要內層區塊還保留著一份參考，那麽外層區塊的環境不會隨著回傳而消失，我們依然可以存取外層環境中的變數。

即使在外層區塊 `makeFunc()` 已經回傳的狀況下，由於內部作用域 `displayName()` 依然存在，只要內層區塊還保留著一份參考，那麽外層區塊的環境不會隨著回傳而被垃圾回收機制回收，依然可以存取外層環境中的變數 `myFunc`。

**一定要 `return` ?**

即使 `displayName()` 只有被執行沒有 `return`，也可以稱作閉包。但如果不 `return`，就無法使用這個閉包。`return` 目的只是要讓作用域外別的函式可以訪問到這個 `displayName()` 函式，因此 `return` 與否，與是否是閉包無關。

### 每次函式被呼叫時，都會創造一組新的語彙環境 (Lexical Environment)

**範例二**

```javascript=
function makeAdder(x) {
  function add(y) {
    return x + y;
  }
  return add;
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

add5(2)    // 7
add10(2)    // 12
```

`add5` 與 `add10` 都是閉包。他們共享函式的定義，卻保有不同的環境：在 `add5` 的作用域環境，`x` 是 `5`。而在 `add10` 的作用域環境， `x` 則是 `10`。

## Closure 的應用

閉包 (closure) 在比較高層次的概念上，可以想成把函式和一組資料關聯起來。

這對應到物件導向程式設計 (Object-Oriented Programming) 中，物件方法可以存取物件屬性 (property) 的特性。

### 模組模式（Module Pattern）

利用函數的「閉包 (Closure)」特性來避免汙染全域的問題，使用閉包 (Closure) 來提供封裝的功能，將方法和變數限制在一個範圍內存取與使用。

**範例**

我們創造一個 `counter` 物件，並提供三個方法存取物件內部的 `count` 變數。

```javascript=
function makeCounter() {
  let count = 0;
  function changeBy(val) {
    count += val;
  }

  return {
    increment: function() {
      changeBy(1)
    },
    decrement: function() {
      changeBy(-1)
    },
    value: function() {
      return count
    }
  };
};

const counter = makeCounter();

console.log(counter.value());    // 0
counter.increment();
counter.increment();
console.log(counter.value());    // 2
counter.decrement();
console.log(counter.value());    // 1
```

因為 Closure 的特性，`counter` 物件的三個方法 `increment()`、`decrement()` 和 `value()` 能夠存取同一個作用域環境 (Lexical Environment)，所以這三個方法能夠存取 `makeCounter()` 中的同一個 `count` 變數及 `changeBy()` 函式。

透過呼叫這三個方法，我們能夠改變或讀取隱藏起來的 count 變數。

值得注意的是，除非透過 counter 物件上的 increment()、decrement() 或 value() 方法，我們沒辦法直接存取其內部的 count 變數。

### 利用 IIFE (Immediately Invoked Function Expression) 產生獨立的環境

**範例**

來看看以下這段程式碼會印出什麼值

```javascript=
for (var i = 0; i < 5; ++i) {
  setTimeout(function() {
    console.log(i)
  }, 1000 * i)
}
```

結果會是 `5 5 5 5 5`。

`var` 宣告的變數是以函式作為 scope，所以變數 `i` 可以看成是全域變數。上面那樣的寫法實際上等於：

```javascript=
var i;
for (i = 0; i < 5; ++i) {
  setTimeout(function() {
    console.log(i)
  }, 1000 * i)
}
```

這段程式碼總共會產生五個 callback 函式，因為 closure 的特性，它們都會存取到 global scope 中的同一個變數 `i`。

當跑完 for 迴圈後，i 等於 `5`。

接著每隔一秒會有一個 callback 被呼叫到。當 callback 實際被呼叫到時，才會去看 `i` 實際的值，也就是 `5`，所以才會印出 `5 5 5 5 5`。

如果要順利印出 `1 2 3 4 5` 的話有以下幾種方法可以解決：

* 把整段 `setTimeout()` 的程式碼包在 IIFE 中去執行。

```javascript=
for (var i = 0; i < 5; ++i) {
  (function(j) {
    setTimeout(function() {
      console.log(j)
    }, 1000 * j)
  })(i);
}
```

for 迴圈的每個 iteration 中，callback 都會被包在一個新的 IIFE 中，每個 IIFE 都是一組獨立的環境。

呼叫 IIFE 時，會將 `i` 的值複製給 `j`，因此每個 IIFE 都會保存各自的變數 j。

因為 closure 的關係，每個 callback 都可以存取到 IIFE 中的變數 `j`，並且不同 callback 存取到的變數 `j` 都是各自獨立的變數。

* 用 IIFE 產生一個 callback 函數。

```javascript=
for (var i = 0; i < 5; ++i) {
  setTimeout((function(j) {
    return function() {
      console.log(j)
    }
  })(i), 1000 * i)
}
```

這裏的 IIFE 回傳了我們需要讓 `setTimeout()` 執行的 callback。

原理和前一個範例相同，都是利用 IIFE 在 for 迴圈的每個 iteration 產生一組新的環境。

* 用 `let`

```javascript=
for (let i = 0; i < 5; ++i) {
  setTimeout(function() {
    console.log(i)
  }, 1000 * i)
}
```

對 `let` 而言，每個 for 迴圈中的 iteration 都是一個獨立的環境，所以很自然地每個 setTimeout() 的 callback 看到的 `i` 是不同的 `i`。


## 參考資料
* [MDN - Closure](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Closures)
* [重新認識 JavaScript: Day 19 閉包 Closure](https://ithelp.ithome.com.tw/articles/10193009)
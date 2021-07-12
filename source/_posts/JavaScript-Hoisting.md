---
title: "[筆記] JavaScript - Hoisting ( 提升 )"
date: 2021-04-14 07:35:05
tags: JavaScript
---
![](/uploads/note.jpg)

## 什麼是 Hoisting？

在 JavaScript 中變數和函式的宣告會在編譯階段就被先建立一個記憶體空間，等到實際執行時再將值放入記憶體空間內，也就是說變數的宣告被「提升」到最上面去，不過實際執行的位置並沒有改變。
<!-- more -->
[MDN](https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting) 中說到：
>Hoisting 只是用來釐清 JavaScript 在執行階段如何運作的說法，實際位置和程式碼一樣。

## 變數的 Hoisting

接著我們用例子來說明一下：

```javascript=
console.log(a);    // not defined
```
因為沒有宣告變數 `a`，所以會出現錯誤 `a is not defined`。

```javascript=
console.log(a);    // undefined
var a;
```
可以看到宣告變數之後，結果會是 `undefined`，可以發現**只要有宣告變數，就不會出錯**。

```javascript=
console.log(a);    // undefined
var a = 1;
```
接著我們看這個例子為什麼結果不是 `1` 呢？這段程式碼可以解析成以下：

```javascript=
var a;
console.log(a);    // undefined
a = 1;
```
先宣告變數 `var a`，之後才賦予值 `a = 1`，**只有宣告會提升，賦值不會**。

### `let`、`const` 的 Hoisting 

ES6 中導入的新的宣告方式，而 `let` 跟 `const` 的 Hoisting 行為比較不一樣，我們看以下範例：

```javascript=
var a = 10
function test(){
  console.log(a)
  let a
}
test()
```
這段範例的結果為 `Cannot access 'a' before initialization`，這是因為 `var` 宣告的變數會被初始化為 `undefined`，而 `let` 跟 `const` 並不會被初始化為 `undefined`，在還未賦值之前是不能被存取的，如果你在賦值之前存取就會拋出錯誤。
## 函式的 Hoisting

* 函式陳述式 (Function Statement)

先宣告一個函式再呼叫，我們可以預期它的結果為 `"My cat's name is Chloe"`。

```javascript=
function catName(name) {
  console.log("My cat's name is " + name);
}
catName("Chloe");    // "My cat's name is Chloe"
```

如果在宣告之前先呼叫會發生什麼事呢？程式一樣可以運作。

```javascript=
catName("Chloe");    // "My cat's name is Chloe"

function catName(name) {
  console.log("My cat's name is " + name);
}
```

* 函式表達式 ( Function Expression )

接著我們來看看函式表達式的例子：

```javascript=
catName("Chloe");    // catName is not a function
var catName = function(name) {
  console.log("My cat's name is " + name);
}
```
會出現錯誤 `catName is not a function`，我們來解析一下：

```javascript=
var catName;
catName("Chloe");  
var catName = function(name) {
  console.log("My cat's name is " + name);
}
```
在 `catName` 還是為 `undefined` 時，不能當作函式來呼叫，因此會出現錯誤。

## Hoisting 的優先權

我們把變數跟函式放在一起來看看：

```javascript=
console.log(a);    // ƒ a(){}
var a;
function a(){};
```
雖然變數和函式都會被提升，但是**函式的優先權比較高**，結果會是 `function` 而不是 `undefined`。

如果函式有參數的話怎麼辦？

```javascript=
function test(v){
  console.log(v)
  var v = 3
}
test(10)
```
結果是 `10` 而不是 `undefined`。

我們來解析一下：

```javascript=
function test(v){
  var v = 10    // 因為下面呼叫 test(10)
  var v
  console.log(v)
  v = 3
}
test(10)
```
可以發現**參數的優先權是大於變數的**。

**總結**

* 變數和函式的宣告都會提升，優先權為函式 > 參數 > 變數
* 只有宣告會提升，賦值不會


## 參考資料
* [MDN - Hoisting](https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting)
* [我知道你懂 hoisting，可是你了解到多深？](https://blog.techbridge.cc/2018/11/10/javascript-hoisting/)
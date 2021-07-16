---
title: "[筆記] JavaScript - 處理字串的方法"
date: 2020-12-16 12:16:28
tags: JavaScript
---
![](/uploads/note.jpg)
紀錄 JavaScript 中處理字串的方法
<!-- more -->
#### `length` 字串長度

```javascript=
var str = 'Hello World';
console.log(str.length);    // 11
```

#### `charAt()` 從字串中返回指定的字元

**語法**

```javascript=
str.charAt(index)
```

**範例**

```javascript=
var str = 'Hello World';
console.log(str.charAt(0));    // 'H' 第一個字元
console.log(str.charAt(str.length-1));    // 'd' 最後一個字元
console.log(str.charAt(12));    // '' 空字串
```

#### `substring()` 從一段字串中擷取其中的一段

**語法**

```javascript=
str.substring(indexStart[, indexEnd])
```

* `indexStart` 等於 `indexEnd`，`substring` 返回一个空字串。
* 省略 `indexEnd`，`substring` 提取字串一直到字串末。
* 任一参數小於 0 或為 `NaN`，則被當作 0。
* 任一參數大於 `stringName.length`，則被當作 `stringName.length`。
* `indexStart` 大於 `indexEnd`，則 `substring` 的執行結果就像兩個參數調換一樣。


**範例**

```javascript=
var str = 'Hello World';
console.log(str.substring(5,5));    // '' 空字串
console.log(str.substring(0,5));    // 'Hello'
console.log(str.substring(5,0));    // 'Hello'
console.log(str.substring(0,13));    // 'Hello World'
```

#### `slice()` 從一段字串中擷取其中的一段

**語法**

```javascript=
str.slice(beginIndex[, endIndex])
```

* `beginIndex` 是一個數字表示要從哪個位置開始擷取；如果 `beginIndex` 是一個負數，則表示值同 "字串長度 + `beginIndex`"；如果 `beginIndex` 大於字串長度，結果會返回空字串。
* 參數 `endIndex` 表示擷取到這個位置之前為止，預設等於字串長度；如果 `endIndex` 是一個負數，則表示值同 "字串長度 + `endIndex`"。

**範例**

```javascript=
var str = 'Hello World';
console.log(str.slice(0, 5));    // 'Hello'
console.log(str.slice(0,-5));    // 'Hello'
console.log(str.slice(6));    // 'World'
console.log(str.slice(13));    // '' 空字串
```

#### `indexOf` 搜尋在字符串中首次出現的位置

**語法**

```javascript=
str.indexOf(searchValue [, fromIndex])
```
* `searchValue` 如果沒提供字串，會強制設為 `'undefined'`，然後在當前字串中尋找這個值。
>'undefined'.indexOf() 會返回 `0`，因為 undefined 在位置 0 處被找到，但是 'undefine'.indexOf() 將會返回 `-1` ，因為 'undefined' 未被找到。
* `fromIndex` 可以是任意整數，預設為 `0`。如果 `fromIndex` 的值小於 `0`，或者大於 `str.length` ，會分别從 0 和 `str.length` 開始。

**範例**

```javascript=
var str = 'Hello World';
console.log(str.indexOf('Hello'));    // 0
console.log(str.indexOf('World'));    // 6
console.log(str.indexOf('H'));    // 0
console.log(str.indexOf('W'));    // 6
```

#### 將字串中的字取代為另一個字

**語法**

```javascript=
str.replace(regexp|substr, newSubStr|function)
```
* `regexp`：符合正則表達式所匹配的內容會被第二個參數替換掉。
* `substr`：一個將被 `newSubStr` 替換的字符串。其被視為一整個字串，而不是一個正則表達式。僅第一個匹配項會被替換。
* `newSubStr`：用於替換掉第一個參數在原字串中的匹配部分的字串。該字串中可以內插一些特殊的變量名。
* `function`：一個用來創建新子字串的函數，該函數的返回值將替換掉第一個參數匹配到的結果。

**範例**

```javascript=
var re = /o/g;
var str = 'Hello World';
console.log(str.replace('Hello','Hi'));    // 'Hi World'
console.log(str.replace(re,'O'));    // 'HellO WOrld'
console.log(str.replace('o','O'));    // 'HellO World'
```

## 參考資料
* [String - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
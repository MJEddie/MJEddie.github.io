---
title: "[筆記] 用 JavaScript 印出各種圖形"
date: 2021-04-13 22:14:15
tags: JavaScript
---
![](/uploads/note.jpg)
JavaScript 的基礎應用，記錄一下當初解題的過程。
<!-- more -->
* 直角三角形

```
*
**
***
****
*****
```

**思考邏輯**

1. 依照層數輸出 `*`
2. 輸出 `*` 後換行

**實際程式碼**

```javascript=
let n = 5;    // 層數
let output = '';
for (i = 0; i < n; i++) {
    let content = '';
    for (j = 0; j <= i; j++) {
        content += '*';
    }
    output += content + '\n';
}
console.log(output);
```

內層的 for 迴圈負責輸出 `*`，依照層數 `n` 輸出相對應的 `*` 數量，所以控制條件 `j<=i`。
```javascript=
for (j = 0; j <= i; j++) {
    content += '*';
}
```

外層 for 迴圈負責控制層數 `n`，將輸出的 `*` 儲存並換行。
```javascript=
for (i = 0; i < n; i++) {
    let content = '';
    for (j = 0; j <= i; j++) {...}
    output += content + '\n';
}
```

* 直角三角形加強版

```
*
**
***
****
*****
****
***
**
*
```

**思考邏輯**

1. 分成上下兩部份
2. 輸出倒立的直角三角形

**實際程式碼**

```javascript=
let n = 5;    // 層數
let output = '';
for (i = 0; i < n; i++) {
    let content = '';
    for (j = 0; j <= i; j++) {
        content += '*';
    }
    output += content + '\n';
}
for (i = n - 1; i > 0; i--) {
    let content = '';
    for (j = 0; j < i; j++) {
        content += '*';
    }
    output += content + '\n';
}
console.log(output);
```

這個三角形可以拆為兩個直角三角形，我們要額外處理下半部的倒立直角三角形。
內層的 for 迴圈一樣依照層數輸出相對應的 `*`。

```javascript=
for (j = 0; j < i; j++) {
    content += '*';
}
```

外層的 for 迴圈控制層數，從四層依序遞減為一層，所以初始值為 `i = n - 1`，一樣將輸出的 `*` 儲存並換行。

```javascript=
for (i = n - 1; i > 0; i--) {
    let content = '';
    for (j = 0; j < i; j++) {...}
    output += content + '\n';
}
```

**利用陣列儲存**

這邊另一個方法是用陣列存起來，最後用 `arr.join()` 的方法將陣列所有元素用 `\n` 串接合併成一個字串回傳。

```javascript=
let n = 5;    // 層數
let output = [];
for (i = 0; i < n; i++) {
    let content = '';
    for (j = 0; j <= i; j++) {
        content += '*';
    }
    output.push(content);
}
for (i = n - 1; i > 0; i--) {
    let content = '';
    for (j = 0; j < i; j++) {
        content += '*';
    }
    output.push(content);
}
console.log(output.join('\n'))
```

* 等腰三角形

```
    *
   ***
  *****
 *******
*********
```

**思考邏輯**

1. 需要額外輸出空格
2. 輸出的 `*` 每層增加 2 個

**實際程式碼**

```javascript=
let n = 5;    // 層數
let output = '';
for (i = 0; i < n; i++) {
    let content = '';
    for (j = n - 1; j > i; j--) {
        content += '&ensp;';
    }
    for (k = 1; k <= 2 * i + 1; k++) {
        content += '*';
    }
    output += content + '\n';
}
console.log(output);
```

輸出的空格從第一層 4 格依序遞減為第五層的 0 格，初始值為層數 -1 `j = n - 1`，而空格只需要輸出到第四層而已，所以判斷條件為 `j > i`。

```javascript=
for (j = n - 1; j > i; j--) {
    content += '&ensp;';
}
```

輸出的 `*` 從第一層 1 個接著每層多增加 2 個，初始值為 `k = 1`，判斷條件 `k <= 2 * i + 1`。

```javascript=
for (k = 1; k <= 2 * i + 1; k++) {
    content += '*';
}
```

* 菱形 ( 等腰三角形加強版 )

```
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *
```

**思考邏輯**

1. 分成上下兩部份
2. 輸出倒立的等腰三角形

**實際程式碼**

```javascript=
let n = 5;    // 層數
let output = '';
for (i = 0; i < n; i++) {
    let content = '';
    for (j = n - 1; j > i; j--) {
        content += '&ensp;';
    }
    for (k = 1; k <= 2 * i + 1; k++) {
        content += '*';
    }
    output += content + '\n';
}
for (i = n - 1; i > 0; i--) {
    let content = '';
    for (j = n - 1; j >= i; j--) {
        content += '&ensp;';
    }
    for (k = 1; k <= 2 * i - 1; k++) {
        content += '*';
    }
    output += content + '\n';
}
console.log(output);
```

菱形可以拆分為兩個等腰三角形，用正立的等腰三角形的概念來處理下半部的等腰三角形。

外圍層數的 for 迴圈從四層開始，依序遞減為一層，初始值為 `i = n - 1`，判斷條件`i > 0`。

```javascript=
for (i = n - 1; i > 0; i--) {...}
```

輸出的空格反過來從第一層 1 格依序增加為第四層的 4 格，初始值為 `j = n - 1`，判斷條件 `j >= i`。

```javascript=
for (j = n - 1; j >= i; j--) {
    content += '&ensp;';
}
```

輸出的 `*` 反過來，初始值一樣為 `k = 1`，判斷條件則變成 `k <= 2 * i - 1`。

```javascript=
for (k = 1; k <= 2 * i - 1; k++) {
    content += '*';
}
```
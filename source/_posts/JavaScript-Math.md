---
title: "JavaScript - Math() 函數的使用"
date: 2020-12-25 15:28:30
tags: JavaScript
---
![](/uploads/note.jpg)
紀錄內建的 `Math()` 屬性和方法，可以 `console.dir(Math)` 查看 `Math()` 的所有函數方法。
<!-- more -->
## `Math()` 的屬性

![](https://i.imgur.com/WaEIpvd.png)

這邊所列的屬性可以直接呼叫它

```javascript=
Math.LN2    // 0.6931471805599453
Math.SQRR2  // 1.4142135623730951  
```


## `Math()` 的方法

![](https://i.imgur.com/JE883tO.png)

這邊舉幾個常用的方法

* `Math.abs()` 取絕對值

```javascript=
Math.abs(-21)    // 21
```

* `Math.random()` 取 0~1 的隨機小數

```javascript=
Math.random()    // 0.17817644947082445
Math.random()    // 0.1376729897557134
Math.random()    // 0.6986859722517493
```

* `Math.celi()` 取大於這個數的最小整數

```javascript=
Math.ceil(6.6)    // 7
Math.ceil(-6.6)   // -6
```

* `Math.floor()` 取小於這個數的最大整數

```javascript=
Math.floor(3.14)    // 3
Math.floor(6.6)     // 6
Math.floor(-6.6)    // -7
```

* `Math.round()` 四捨五入

```javascript=
Math.round(3.14)    // 3
Math.floor(6.6)     // 7
Math.floor(-6.6)    // -7
```


## 進階應用

* 取陣列中隨機的值

```javascript=
Math.floor(Math.random() * arr.length)
```


## 參考資料

* [JavaScript Math 對象](https://www.w3school.com.cn/jsref/jsref_obj_math.asp)
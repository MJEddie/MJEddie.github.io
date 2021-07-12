---
title: "[實作] JavaScript - New Year Countdown"
date: 2020-12-24 22:00:01
tags: JavaScript
---
## 成品

[New Year Countdown](https://mjeddie.github.io/JavaScript-Projects/New_Year_Countdown/index.html)

![](https://i.imgur.com/qH62bBI.jpg)

## 介紹

參考 JavaScript 30 中 Countdown Timer 的練習，利用 `Date()` 物件做的練習。
<!-- more -->
## 功能

倒數現在離元旦剩餘的時間，主要處理可以分為以下幾個部分

* 設置倒數時間
* 計算剩餘時間
* 顯示時間
* 倒數與停止倒數


### 設置倒數時間

因為是做跨年倒數，所以先用 `getFullYear()` 取得現在年份，再設置倒數時間，如果是做限時的倒數器直接設置倒數時間即可

```javascript=
const currentYear = new Date().getFullYear();               
const countDownDate = new Date(`January 01 ${currentYear + 1} 00:00:00`) 
```


### 計算剩餘時間

再來我們要計算剩餘時間，用 `newDate()` 取得現在時間，然後把倒數時間跟現在時間相減取得剩餘時間，然後把得到的時間 (millisecond) 轉換成天數、小時、分鐘跟秒。

1. 把得到的豪秒數轉換成秒：`result / 1000`
2. 將總秒數除以 60 取餘數，就是計算完分鐘術後剩下的秒數：`(result / 1000) % 60`
3. 用 `Math.floor` 取最大整數：`Math.floor(result / 1000) % 60`

分鐘、小時、天數一樣的邏輯

宣告變數，利用 `getElementById()` 來取得 HTML 內的元素

```javascript=
const seconds = document.getElementById("seconds");
const minutes = document.getElementById("minutes");
const hours = document.getElementById("hours");
const days = document.getElementById("days");
```

計算部分

```javascript=
function remainTime() {
    const timeNow = new Date();
    const result = countDownDate - timeNow;

    const s = Math.floor(result / 1000) % 60;
    const m = Math.floor(result / 1000 / 60) % 60;
    const h = Math.floor(result / 1000 / 60 / 60) % 24;
    const d = Math.floor(result / 1000 / 60 / 60 / 24);

    seconds.innerHTML = formatTime(s);
    minutes.innerHTML = formatTime(m);
    hours.innerHTML = formatTime(h);
    days.innerHTML = d;
}
```

### 顯示時間

在時間不是雙位數時增加前導零顯示，像是 `9` 秒時就會顯示 `09`，在數字小於 `10` 時於開頭加個字串 `0`。

```javascript=
function formatTime(time) {
    return time < 10 ? `0${time}` : time;
}
```

### 倒數與停止倒數

使用 `setInterval` 、 `clearInterval` 方法來實現倒數與停止倒數，`setInterval` 每隔一秒呼叫我們的函式，進而達到倒數的功能，當倒數時間到時我們呼叫 `clearInterval` 來終止倒數的功能。

```javascript=
const intervalId = setInterval(() => {
        const result = countDownDate - (new Date());
        if (result < 0) {
            clearInterval(intervalId);
        } else {
            remainTime();
        }
    },
    1000);
```

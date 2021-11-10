---
title: "JavaScript - Date() 日期與時間"
date: 2020-12-26 21:26:09
tags: JavaScript
---
![](/uploads/note.jpg)

JavaScript 沒有日期資料型態，但是它有提供 `Date()` 物件，使用內件的日期物件與方法、可以取得與操作日期時間。`Date()` 物件是基於世界標準時間（UTC） 1970 年 1 月 1 日開始的毫秒數值來儲存時間。
<!-- more -->
## `Date()` 的創建

`Date()` 物件有四種可在建構式傳入參數的類型如下

* 不帶參數 : 表示當前日期和時間
```javascript=
new Date()    // Tue Jan 19 2021 20:28:00 GMT+0800 (台北標準時間) 
```
* 時間單位 : 
    * 必要參數 : 年、月
        * 年 (四位數)
        * 月 (從 0~11，分別代表一月到十二月)
    * 選擇性參數 : 日、時、分、秒、毫秒
        * 日 (預設為 1)
        * 時、分、秒、毫秒 (預設為 0)

```javascript=
new Date(2021,0)    // Fri Jan 01 2021 00:00:00 GMT+0800 (台北標準時間)
```
* milliseconds : 表示從 `1970-01-01 00:00:00 UTC+0` 開始所經過的毫秒數
```javascript=
new Date(0)            // Thu Jan 01 1970 08:00:00 GMT+0800 (台北標準時間)
// 增加一年
new Date(365 * 24 * 60 * 60 * 1000)    // Fri Jan 01 1971 08:00:00 GMT+0800 (台北標準時間)
```
* date string : `dataString` 為表示時間日期的字串，要符合 [ISO8601](https://zh.wikipedia.org/wiki/ISO_8601) 或 [國際標準 RFC2822](https://tools.ietf.org/html/rfc2822#section-3.3) 的格式

```javascript=
new Date('2021/01/01')    // Fri Jan 01 2021 00:00:00 GMT+0800 (台北標準時間)
```


## `Date()` 的方法

### 取得時間

`new Date()` 不加任何參數就會使用瀏覽器環境來取得目前的日期時間

```javascript=
const timeNow = const new Date();    // Tue Jan 19 2021 20:28:00 GMT+0800 (台北標準時間) 
```

#### 取得日期時間的某個值

我們用 `new Date()`取得時間後，可以只取其中的某個值，像是年、月、日等。

```javascript=
const timeNow = const new Date();    // Tue Jan 19 2021 20:28:00 GMT+0800 (台北標準時間)
timeNow.getFullYear()    // 2021
timeNow.getMonth()       // 0
timeNow.getDate()        // 19
timeNow.getDay()         // 2
```

* `getFullYear()`: 取得西元年，為四位數字
* `getMonth()`: 取得月份的值，範圍 0~11
* `getDate()`: 取得日期的值，範圍 1~31
* `getDay()`: 取得星期的值，範圍 0~6

其中我們會發現 `getMonth()`、`getDay()` 回傳的其實是陣列索引值，其中星期是從禮拜日作為一週的第一天。以下是其他取得時間的方法。


|    方法    |    敘述    | 
| -------- | -------- | 
|    getFullYear()    |  從 Date 物件返回年份 ( 四位數字 )  | 
|    getMonth()    |從 Date 物件返回月份 (0 ~ 11)| 
|    getDate()    |從 Date 物件返回一個月中的某一天 (1 ~ 31)| 
|    getHours()    |返回 Date 物件的小時 (0 ~ 23)| 
|    getMinutes()    |返回 Date 物件的分鐘 (0 ~ 59)| 
|    getSeconds()    |返回 Date 物件的秒數 (0 ~ 59)| 
|    getMilliseconds()    |返回 Date 物件的毫秒(0 ~ 999)| 
|    getTime()    |返回 1970 年 1 月 1 日至今的毫秒數| 
|    getDay()    |從 Date 物件返回一週中的某一天 (0 ~ 6)| 


### 設置時間

從 [MDN文件](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Date) 中我們知道以下幾種設置時間的方式，時間為 1995/12/17 03:24:00，如果沒特別指定的話預設是 0。

```javascript=
var today = new Date();
var birthday = new Date('December 17, 1995 03:24:00');
var birthday = new Date('1995-12-17T03:24:00');
var birthday = new Date(1995, 11, 17); // Sun Dec 17 1995 00:00:00 GMT+0800 (台北標準時間)
var birthday = new Date(1995, 11, 17, 3, 24, 0);
```

下面是其他設定的方法，所以方法都可以使用正負整數或是 0，假如超過設定範圍則會自動加減做進位，用剛剛的例子說明，把月份的參數設定為 `12` ( 範圍 0~11)，那麼時間會變成 1996/1/17 03:24:00。

```javascript=
var birthday = new Date(1995, 12, 17, 3, 24, 0);    //  Wed Jan 17 1996 03:24:00 GMT+0800 (台北標準時間)
```

這次我們把小時換成 `-2` 來看看會發生什麼事，會發現時間變成前一天的 22:24:00，別忘了取得時間時回傳的是陣列索引值，所以我們設定 `-2` 也就是陣列中倒數第二個值，也就是 `22` 啦。

```javascript=
var birthday = new Date(1995, 12, 17, -2, 24, 0);    // Tue Jan 16 1996 22:24:00 GMT+0800 (台北標準時間)
```


|    方法    |    敘述    | 
| -------- | -------- | 
|    setFullYear()    |從 Date 物件返回年份 ( 四位數字 )| 
|    setMonth()    |從 Date 物件返回月份 (0 ~ 11)| 
|    setDate()    |從 Date 物件返回一個月中的某一天 (1 ~ 31)| 
|    setHours()    |返回 Date 物件的小時 (0 ~ 23)| 
|    setMinutes()    |返回 Date 物件的分鐘 (0 ~ 59)| 
|    setSeconds()    |返回 Date 物件的秒數 (0 ~ 59)| 
|    setMilliseconds()    |返回 Date 物件的毫秒(0 ~ 999)| 
|    setTime()    |返回 1970 年 1 月 1 日至今的毫秒數| 


## 時間的比較

`Date()` 物件可以用 >, <, <=, >=, <== 或 >== 運算子來比較兩個 `Date()` 物件，日期越晚的越大

```javascript=
const today = new Date();
const someday = new Date('2022/1/1');

if (someday > today) {
    console.log('Today is before 2022/1/1');
} else {
    console.log('Today is after 2022/1/1');
}

// Today is before 2022/1/1
```

但如果要比較兩個日期是否相等，運算子 `==` `!=` `===` `!==`，要先將 `Date()` 物件用 `date.getTime()` 轉換為數值型態才能比較

```javascript=
const d1 = new Date(2022, 1, 1);
const d2 = new Date(2022, 1, 1);

const same = d1.getTime() === d2.getTime();    // true
const notSame = d1.getTime() !== d2.getTime(); // false
```



## 參考資料

* [JavaScript Date Objects](https://www.w3schools.com/js/js_dates.asp)
* [MDN Date](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Date)
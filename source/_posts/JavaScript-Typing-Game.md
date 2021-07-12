---
title: "[實作] JavaScript - Typing Game"
date: 2021-01-23 00:20:36
tags: [JavaScript, AXAJ]
---
## 成品

[Typing Game](https://mjeddie.github.io/JavaScript-Projects/Typing_Game/index.html)

![](https://i.imgur.com/RBCDALh.png)

## 介紹

串接 [Random Word](https://random-word-api.herokuapp.com/home) 、[臺北市立動物園_動物資料](https://data.taipei/#/dataset/detail?id=5cb73231-b741-48b3-bec3-2ef57bb10029) API 並用 jQuery 語法做的練習。
<!-- more -->
## 功能

畫面會顯示隨機的單字，輸入正確後得分並增加時間，有三種難度可選擇，難度高答對後增加的時間較少，時間到即停止。

* 從 API 中產生隨機單字
* 計時器倒數時間
* 輸入正確後根據難度增加一定的時間
* 將難度儲存至 localStorage

### 從 API 中產生隨機單字

使用 `fetch()` 取得 [Random Word](https://random-word-api.herokuapp.com/home)、[臺北市立動物園_動物資料](https://data.taipei/#/dataset/detail?id=5cb73231-b741-48b3-bec3-2ef57bb10029) 的資料當作我們隨機產生的單字來源，難度簡單的話使用 [Random Word](https://random-word-api.herokuapp.com/home) 的資料，中等跟困難的話則使用 [臺北市立動物園_動物資料](https://data.taipei/#/dataset/detail?id=5cb73231-b741-48b3-bec3-2ef57bb10029) ，取得資料後透過 `json()` 處理檔案，接著傳遞到下一層，動物資料我們擷取 `A_Name_Latin` 拉丁名字作為我們的單字。

函數 `Math.random()` 會隨機產生出 0~1 之間的小數，最大不會超過 1，而 `length` 則回傳陣列中包含的筆數，把兩個相乘可以得到一個最大不會超過該陣列資料筆數的小數，假設我們今天陣列中有 2 筆資料，兩者所相乘的結果是一個不會大於 2 的小數。

`Math.floor` 會回傳小於等於所給數字的最大整數，假設剛相乘完的結果是 39.9，那麼我們就可以抓取陣列中第 39 筆資料中的拉丁名字作為我們的單字。

```javascript=
function simpleWords() {
    fetch('https://random-word-api.herokuapp.com/word?number=1')
        .then(res => res.json())
        .then(data => {
            randomWord = data[0];
            $('h1').text(randomWord);
        })
}

function hardWords() {
    fetch('https://data.taipei/api/v1/dataset/f18de02f-b6c9-47c0-8cda-50efad621c14?scope=resourceAquire')
        .then(res => res.json())
        .then(data => {
            const words = data.result.results;
            randomWord = words[Math.floor(Math.random() * words.length)].F_Name_Latin;
            $('h1').text(randomWord);
        })
}
```


### 計時器倒數時間

`setInterval` 每隔一秒呼叫我們的函式，進而達到倒數的功能，當倒數時間到時我們呼叫 `clearInterval` 來終止倒數的功能。

```javascript=
function countDownTime() {
    $('#time').text(`${time}s`);
    time--;
}

const intervalid = setInterval(() => {
    if (time < 0) {
        clearInterval(intervalid);
    } else {
        countDownTime();
    }
}, 1000)
```


### 輸入正確後根據難度增加一定的時間

對輸入欄位作事件監聽，如果輸入的與顯示的單字相符，那麼計分會 +1，清空輸入欄位並顯示下一組單字，用 `if...else` 迴圈判斷所選擇的難度，並增加相對應的時間。

```javascript=
$('input').keyup(function() {
    const answer = $(this);
    if (answer.val() === randomWord) {
        if (difficulty === 'easy') {
            simpleWords();
        } else {
            hardWords();
        }
        score++;
        $('#score').text(score);
        answer.val('');

        if (difficulty === 'easy') {
            time += 5;
        } else if (difficulty === 'medium') {
            time += 10;
        } else {
            time += 8;
        }
        countDownTime();
    }
})
```


### 將難度儲存至 localStorage

對我們表單中的下拉式選單作監聽，也就是難度選擇的地方，取得值之後用 `localStorage.setItem()` 儲存，並用 `localStorage.getItem()` 讀取，而一開始 `localStorage` 中沒有值時我們預設難度為 easy。

**HTML**

```htmlembedded=
<form id="settings-form">
            <div>
                <label for="difficulty">Difficulty</label>
                <select id="difficulty">
            <option value="easy">Easy</option>
            <option value="medium">Medium</option>
            <option value="hard">Hard</option>
          </select>
            </div>
        </form>
```

**JavaScript**

```javascript=
$('#settings-form').change(function() {
    difficulty = $('#difficulty').val();
    localStorage.setItem('difficulty', difficulty)
})

let difficulty = localStorage.getItem('difficulty') !== null ? localStorage.getItem('difficulty') : 'easy';
$('#difficulty').val(difficulty);
```
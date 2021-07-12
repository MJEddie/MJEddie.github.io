---
title: "[實作] JavaScript - Speak Number Guess"
date: 2021-01-22 01:20:53
tags: JavaScript
---
## 成品

[Speak Number Guess](https://mjeddie.github.io/JavaScript-Projects/Speak_Number_Guess/index.html)

![](https://i.imgur.com/Hgo8uNp.jpg)

## 介紹

使用 [Web SpeechRecognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) 做的猜數字遊戲。
<!-- more -->
## 功能

數字猜謎遊戲，每次會隨機指定一個數字 ( 範圍 1~100 ) 為終極密碼，透過麥克風猜數字縮小範圍，並會顯示目前範圍，猜對後顯示終極密碼並結束遊戲。

* 隨機產生終極密碼 
* 取得語音輸入
* 顯示數字範圍
* 顯示遊戲結束


### 隨機產生終極密碼

用 `Mathrandom()` 和 `Math.floor()` 取得隨機小數後乘上 100，這時候數字的範圍是 0 ~ 99，最後要 +1 才是我們要的範圍。

```javascript=
// Generate random number
function randomNumber() {
    return Math.floor(Math.random() * 100) + 1;
}
```

### 取得語音輸入

這邊先介紹會用到的幾個 [Web SpeechRecognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) 的方法跟屬性 : 

* `SpeechRecognition.start()` : 啟動語音辨識，以偵聽傳入的音頻
* `end` 事件: 會在語音辨識結束時觸發
* `result` 事件 : 會在語音辨識回傳結果時被觸發

目前因為每個瀏覽器的支援不一樣，要確保可以使用要將 `webkit` 標頭的 `webkitSpeechRecognition` 指定給該全域變數，方便我們使用。

之後透過`start()` 啟動語音辨識，並監聽 `result` 來取得我們要的資料作後續處理。觀察可以發現 `results` 中的 `transcript` 是語音輸入所辨識的數字，之後我們會拿這個跟終極密碼去比較。

```javascript=
window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

let recognition = new SpeechRecognition();

recognition.start();
recognition.addEventListener('result', onSpeak);
```

![](https://i.imgur.com/MOPsIjC.png)


### 顯示數字範圍

先看一下我們辨識語音後 HTML 的格式， `<span>` 標籤內放的是我們猜的數字，後面的 `<div>` 則會顯示數字的範圍。

```htmlembedded=
<div id="msg" class="msg">
        <!-- sample HTML
        <div>You said:</div>
        <span class="box">Guessed number</span>
        <div>number range</div> 
        -->
    </div>
```

把剛觀察後我們要的資料拿出來，接著要判斷的可以分為三個部分 : 

1. 判斷是否為數字 : 用 `parseInt()` 剛把得到的資料轉型為數字，若無法轉換為數字會回傳 `NaN`，如果不是數字的話顯示 `That is not a valid number`
2. 判斷數字的範圍，如果超出範圍顯示 `Number must be between 1 and 100`
3. 判斷數字和終極密碼之間的大小關係，進而縮減數字的範圍並顯示出來

最後監聽 `end` 事件，以便一直辨識語音直到我們答對為止。

```javascript=
function onSpeak(e) {
    const msg = e.results[0][0].transcript;

    checkNumber(msg);
}

function checkNumber(msg) {
    const number = parseInt(msg);
    $('#msg').html(`
            <div>You said: </div>
            <span class="box">${msg}</span>
        `)

    if (isNaN(number)) {
        $('#msg').append(`<div>That is not a valid number</div>`);
    } else if (number > 100 || number < 1) {
        $('#msg').append(`<div>Number must be between 1 and 100</div>`)
    } else if (number === finalNumber) {
        $('body').html(`
            <h2>Congrats! You have guessed the number! <br><br>
            It was ${number}</h2>
            <button class="play-again" id="play-again">Play Again</button>
        `)
    } else if (number > finalNumber) {
        max = number;
        $('#msg').append(`<div>The number is between ${min} - ${max}</div>`);
    } else {
        min = number;
        $('#msg').append(`<div>The number is between ${min} - ${max}</div>`);
    }
}
```

### 顯示遊戲結束

遊戲結束後的畫面如下，顯示最後的終極密碼並有個按鈕可以再玩一次，對此我們對按鈕綁定點擊事件已觸發網頁刷新。

![](https://i.imgur.com/65f89u8.jpg)

**HTML**

```htmlembedded=
<button class="play-again" id="play-again">Play Again</button>
```

**JavaScript**

```javascript=
$('body').click(function(e) {
    if (e.target.id == 'play-again') {
        window.location.reload();
    }
});
```


## 參考資料

* [MDN SpeechRecognition](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition)
---
title: "[實作] JavaScript - Music Player"
date: 2021-01-21 00:21:02
tags: JavaScript
---
## 成品

[Music Player](https://mjeddie.github.io/JavaScript-Projects/Music_Player/index.html)

![](https://i.imgur.com/bHfphgj.png)

## 介紹

使用 [Web Audio API](https://developer.mozilla.org/zh-TW/docs/Web/API/Web_Audio_API) 做的播放器練習。
<!-- more -->
## 功能

播放時旋轉封面圖片、顯示歌曲詳細資訊以及播放條，可切換上一首 \ 下一首 或是快轉至點擊處播放，歌曲結束時自動播放下一首。

* 載入歌曲
* 播放和暫停功能
* 切換歌曲功能*
* 顯示播放進度條
* 點擊進度條播放


### 載入歌曲

Web Audio API 可於網頁上操作並播放歌曲，在 HTML 部分使用 `<audio></audio>` tag 就可以播放音樂，播放器的 icon 使用 [Font Awesome](https://fontawesome.com/) 呈現。

在播放前我們先設置歌曲的初始狀態，把要播放的歌曲放入陣列中供我們讀取，並設置初始索引值決定從哪首歌曲開始播放，並載入相對應的音訊檔和封面照片。

補充說明一下使用 `jQuery` 語法抓取元素時會得到一個陣列型態的 `jQuery` 物件，需要用 `get()` 轉換為 `JavaScript` DOM 元素之後再操作。

```javascript=
const audio = $('#audio').get();
const audio = $('#audio').get(0);    // get 後面接的是索引值，索引值由 0 開始

const audio = $('#audio')[0];        // 也可以這樣寫
```

**HTML**

```html
<audio id="audio"></audio>

<div class="navigation">
    <button id="prev" class="action-btn">
        <i class="fas fa-backward"></i>
    </button>
    <button id="play" class="action-btn action-btn-big">
        <i class="fas fa-play"></i>
    </button>
    <button id="next" class="action-btn">
        <i class="fas fa-forward"></i>
    </button>
</div>
```

**JavaScript**

```javascript=
const audio = $('#audio')[0];
const title = $('#title');
const cover = $('#cover')[0];

let songIndex = 2;
const songs = ['acousticbreeze', 'allthat', 'beyondtheline', 'creativeminds', 'onceagain'];

function loadSong(song) {
    title.text(song);
    audio.src = `music/${song}.mp3`;
    cover.src = `images/${song}.jpg`;
}

loadSong(songs[songIndex]);
```


### 播放和暫停功能

播放和暫停其實很相像，當按下播放時增加 `play` 的 class ，然後把播放的按鈕換成暫停的樣式，接著執行音訊檔的播放。暫停則反之。

之後我們要做的就是按鈕的監聽，看是否有 `play` 這個 class 而決定我們要執行哪個函式。

```javascript=
function playSong() {
    $('#music-container').addClass('play');
    playBtn.find('i.fas').removeClass('fa-play');
    playBtn.find('i.fas').addClass('fa-pause');

    audio.play();
}

// Pause song
function pauseSong() {
    $('#music-container').removeClass('play');
    playBtn.find('i.fas').addClass('fa-play');
    playBtn.find('i.fas').removeClass('fa-pause');

    audio.pause();
}

// Event listener
playBtn.click(function() {
    const isPlaying = $('#music-container').hasClass('play');

    if (isPlaying) {
        pauseSong();
    } else {
        playSong();
    }
});
```

### 切換歌曲功能

由於我們的歌曲資料是放在陣列中，切換歌曲時只需要去改變我們要讀取的索引值就好，要注意當我們一直往前一首播放時，要判斷是否已經超過第一首 ( 索引值為 0)，如果是的話就播放最後一首，往下一首播放反之。

接著綁定前一首 \ 下一首 的按鈕監聽，再執行對應的函式。

而當歌曲播放結束時我們也需要呼叫函式來播放下一首，這時需要用到 `audio` 的 `ended `屬性，來看歌曲播放是否結束，監聽歌曲是否結束來播放下一首歌曲。


```javascript=
function prevSong() {
    songIndex--;

    if (songIndex < 0) {
        songIndex = songs.length - 1;
    }

    loadSong(songs[songIndex]);

    playSong();
}

// Next song
function nextSong() {
    songIndex++;

    if (songIndex > songs.length - 1) {
        songIndex = 0;
    }

    loadSong(songs[songIndex]);

    playSong();
}

// Event Linstener
prevBtn.click(function() {
    prevSong();
});
nextBtn.click(function() {
    nextSong();
});

$('#audio').bind('ended', function() {
    nextSong();
});
```



### 顯示播放進度條

這邊需要用到 Web Audio API 中的幾個屬性

* `duration` ( 持續時間 ): 返回當前媒體的長度
* `currentTime` ( 當前時間) : 設置或返回媒體中的當前播放位置
* `timeupdate` ( 時間更新 ) : 當前播放位置發生變化時觸發

分別取得歌曲目前播放長度和歌曲總長度，接著將兩者相除 * 100 就會得到目前長度的百分比數值，之後對歌曲綁定時間更新的事件去觸發，不斷的去改變我們的容器寬度，就達成進度條的功能。

```javascript=
$('#audio').bind('timeupdate', function() {
    const { duration, currentTime } = audio;
    const progressPercent = (currentTime / duration) * 100;
    progress.css('width', `${progressPercent}%`);
});
```

### 點擊進度條播放

這邊的概念跟顯示播放進度很類似，我們取得滑鼠點擊進度條的位置，看他佔整個進度條寬度的百分比，之後乘上歌曲的總長度，得到我們要播放歌曲的位置。

![](https://javascript.info/article/size-and-scroll/metric-all.svg)

這邊用圖輔助說明，首先要取得滑鼠點擊位置，`jQuery` 中沒有 `offsetX` 屬性，所以我們用 `offset()`、`pageX` 來代替

1. `$(e.target).offset().left` 可以抓被點到的元素離頁面左邊的位置
2.  `e.pageX` 可以抓當下被點到位置距離頁面左邊
3. 用 `pageX - clickX` 就可以得到滑鼠點擊的位置
4. 把 `(pageX - clickX)` / `width` ( 進度條的寬度 ) * `duration` 等於我們滑鼠點擊歌曲要播放的位置


```javascript=
$('#progress-container').click(function(e) {
    const width = $('#progress-container').width();
    const clickX = $(e.target).offset().left;
    const pageX = e.pageX;

    const duration = audio.duration;

    audio.currentTime = ((pageX - clickX) / width) * duration;
});
```


## 參考資料

[JS30-Day11-Custom Video Player](https://ithelp.ithome.com.tw/articles/10194816)




---
title: "[實作] AJAX - Twitch Live Streams"
date: 2021-02-25 23:56:03
tags: [JavaScript, AJAX]
---
## 成品

[Twitch Live Streams](https://mjeddie.github.io/JavaScript-Projects/Twitch_Streaming/index.html)

![](https://i.imgur.com/vKr1TTa.jpg)

## 介紹

串接 [Twitch API](https://dev.twitch.tv/docs/v5) 獲取直播中的頻道，頻道依照觀眾人數自動排列，上方有頻道分類可以選擇，點擊後出現該分類中直播的頻道，同時變成紫底白字告訴使用者現在顯示的分類，點擊直播畫面開啟新分頁跳轉至該直播頻道，搭配 infinite scroll 頁面下滑到快底時會自動載入新的頻道。
<!-- more -->
## 功能

* 顯示直播中的頻道
* Infinite Scroll
* 頻道分類功能


### 顯示直播中的頻道

**申請 client ID**

在開始使用 API 前我們需要到[主控台](https://dev.twitch.tv/console/apps/create)申請一組 client ID 作為識別，名稱中不能含有 "twitch"，重新導向網址依照建議填選 "http://localhost" ，分類則依照需求填寫。

![](https://i.imgur.com/xYOVYhT.png)

**Request**

申請好 client ID 就可以開始使用 API 了，官方文件中提供多種的使用方式，而我們要使用到的是 Streams 中的 [Get Live Streams](https://dev.twitch.tv/docs/v5/reference/streams#get-live-streams)，這邊我們使用到的參數為：
* `game`： 回傳的遊戲類型
* `limit`： 每筆回傳的資料數量
* `offset`： 分頁中回傳的資料數量

我們預設要回傳的遊戲類型是 League of Lengends，每次顯示 9 個頻道，`offset` 一開始為 0。

```javascript=
let nowIndex = 0;
let gameName = 'League%20of%20Legends';
const clientId = 'bgo21lskupamuirpz6ra5cn1ri0mta';

function getLiveStreams() {
    const limit = 9;
    $.ajax({
        url: `https://api.twitch.tv/kraken/streams/?game=${gameName}&limit=${limit}&offset=${nowIndex}`,
        headers: {
            'Client-ID': clientId,
            'Accept': 'application/vnd.twitchtv.v5+json'
        },
        method: 'GET',
        dataType: 'json',
        success: function(res) {
            $.each(res.streams, function(i, stream) {
                showChannel(stream);
            });
        },
        error: function(err) {
            alert(err);
        }
    });
}
```

Response 為

```json=
{
      "streams": [
   {
      "_id": 23937446096,
      "average_fps": 60,
      "channel": {
            "_id": 121059319,
            "broadcaster_language": "en",
            "created_at": "2016-04-06T04:12:40Z",
            "display_name": "MOONMOON_OW",
            "followers": 251220,
            "game": "Overwatch",
            "language": "en",
            "logo": "https://static-cdn.jtvnw.net/jtv_user_pictures/moonmoon_ow-profile_image-0fe586039bb28259-300x300.png",
            "mature": true,
            "name": "moonmoon_ow",
            "partner": true,
            "profile_banner": "https://static-cdn.jtvnw.net/jtv_user_pictures/moonmoon_ow-profile_banner-13fbfa1ba07bcd8a-480.png",
            "profile_banner_background_color": null,
            "status": "KKona where my Darryl subs at KKona",
            "updated_at": "2016-12-15T20:04:53Z",
            "url": "https://www.twitch.tv/moonmoon_ow",
            "video_banner": "https://static-cdn.jtvnw.net/jtv_user_pictures/moonmoon_ow-channel_offline_image-2b3302e20384eee8-1920x1080.png",
            "views": 9869754
      },
      "created_at": "2016-12-15T14:55:49Z",
      "delay": 0,
      "game": "Overwatch",
      "is_playlist": false,
      "preview": {
            "large": "https://static-cdn.jtvnw.net/previews-ttv/live_user_moonmoon_ow-640x360.jpg",
            "medium": "https://static-cdn.jtvnw.net/previews-ttv/live_user_moonmoon_ow-320x180.jpg",
            "small": "https://static-cdn.jtvnw.net/previews-ttv/live_user_moonmoon_ow-80x45.jpg",
            "template": "https://static-cdn.jtvnw.net/previews-ttv/live_user_moonmoon_ow-{width}x{height}.jpg"
      },
      "video_height": 720,
      "viewers": 11523
   },
      ...
   ]
}
```

可以看到 `streams`中為我們需要的資料，接著呼叫函式來處理我們的資料

**顯示直播頻道**

呼叫函式，將回傳的資料整理然後顯示在頁面上。
另外我們在 HTML 中寫入 `onclick="window.open('${url}','_blank')"` 就可以達到點擊時開啟新分頁跳轉至該直播頻道。

另外用一個函式來處理觀看人數，超過萬人觀看時改變顯示，例如 11000 觀看會變成 1.1 萬。 
```javascript=
function showChannel(stream) {

    // 抓取資料
    const preImg = stream.preview.medium;
    const avatarImg = stream.channel.logo;
    const title = stream.channel.status;
    const name = stream.channel.display_name;
    const url = stream.channel.url;
    const viewers = stream.viewers;

    // 插入 DOM 中
    const channel = $('<div class="channel"><div>');
    channel.html(`
        <div class="wrap" onclick="window.open('${url}','_blank')">
            <div class="preview">
                <img src=${preImg} alt="preview">
            </div>
            <div class="viewer">觀眾人數 : ${formatViewer(viewers)}</div>
            <div class="info">
                <div class="avatar">
                    <img src=${avatarImg} alt="streamer">
                </div>
                <div class="intro">
                    <h4 class="channel-name">${title} ...</h4>
                    <span class="streamer">${name}</span>
                </div>
            </div>
        </div>
    `);

    $('#channels').append(channel);
}

// format viewer
function formatViewer(viewer) {
    return viewer >= 10000 ? (viewer / 10000).toFixed(1) + '萬' : viewer;
}
```

### Infinite Scroll

加入之前學習到的 [infinite scroll](https://hackmd.io/@s8vWHEjcQQa-XRpr3ILGpw/ByMrFaHAw) 功能，透過 `isLoading` 來限制每次快滑到底時只發出一次請求。

```javascript=
$(window).scroll(function() {
    const { scrollTop, scrollHeight, clientHeight } = document.documentElement;

    if (scrollTop + clientHeight >= scrollHeight - 200) {
        if (!isLoading) {
            getLiveStreams();
        }
    }
});
```

1. 一開始 `isLoading` 為 `false`
2. 獲取 API 資料時為 `true`
3. 資料渲染至頁面後 `nowIndex += 9`，代表我們這次渲染的頻道數量，之後自動載入頻道時會從第 10 筆資料開始載入，並將 `isLoading` 設為 `false` 防止一直發出請求

```javascript=
function getLiveStreams() {
    const limit = 9;
    isLoading = true;
    $.ajax({
        url: `https://api.twitch.tv/kraken/streams/?game=${gameName}&limit=${limit}&offset=${nowIndex}`,
        headers: {
            'Client-ID': clientId,
            'Accept': 'application/vnd.twitchtv.v5+json'
        },
        method: 'GET',
        dataType: 'json',
        success: function(res) {
            $.each(res.streams, function(i, stream) {
                showChannel(stream);
            });
        },
        error: function(err) {
            alert(err);
        }
    });
}

function showChannel(stream) {

    // 抓取資料
    const preImg = stream.preview.medium;
    const avatarImg = stream.channel.logo;
    const title = stream.channel.status;
    const name = stream.channel.display_name;
    const url = stream.channel.url;
    const viewers = stream.viewers;

    // 插入 DOM 中
    const channel = $('<div class="channel"><div>');
    channel.html(`
        <div class="wrap" onclick="window.open('${url}','_blank')">
            <div class="preview">
                <img src=${preImg} alt="preview">
            </div>
            <div class="viewer">觀眾人數 : ${viewers}</div>
            <div class="info">
                <div class="avatar">
                    <img src=${avatarImg} alt="streamer">
                </div>
                <div class="intro">
                    <h4 class="channel-name">${title} ...</h4>
                    <span class="streamer">${name}</span>
                </div>
            </div>
        </div>
    `);

    $('#channels').append(channel);
    nowIndex += 9;
    isLoading = false;
}
```

### 頻道分類功能

監聽按鈕的點擊事件，取得按鈕代表的頻道分類，發出請求並渲染至頁面上，加上 `active` 的 class 名稱改變 CSS 樣式告知使用者現在顯示的直播分類。

要注意的是要將 `offset` 歸零再將資料渲染至頁面上，不然的話會因為已經渲染過的關係(`offset` 的值)，會從當下 `offset` 的值之後開始渲染。

**HTML**

```html=
<div class="category">
    <button name="Just%20Chatting">Just Chatting</button>
    <button name="Hearthstone">Hearthstone</button>
    <button name="Apex%20Legends">Apex Legends</button>
    <button name="League%20of%20Legends" class="active">League of Legends</button>
</div>
```

**JavaScript**

```javascript=
$('button').click(function() {
    gameName = $(this).attr('name');
    $(this).addClass('active').siblings().removeClass('active');
    $('#channels').html('');

    // reset
    nowIndex = 0;
    getLiveStreams();
```
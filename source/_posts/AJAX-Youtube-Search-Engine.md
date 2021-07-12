---
title: "[實作] AJAX - Youtube Search Engine"
date: 2021-02-07 20:15:44
tags: [JavaScript, AJAX]
---
## 成品

[Youtube Search Engine](https://mjeddie.github.io/JavaScript-Projects/Youtube_Search/index.html)

![](https://i.imgur.com/KMqb4if.png)

## 介紹

串接 [Youtube Data API](https://developers.google.com/youtube/v3/getting-started) 將搜尋結果列出來，並引入 fancybox 點擊播放時彈出視窗顯示在頁面最上層，如下圖。
<!-- more -->
![](https://i.imgur.com/zl9MPVZ.png)

## 功能

* 列出搜尋結果
* 上一頁 / 下一頁 功能
* Fancybox

### 列出搜尋結果

**申請 Youtube Data API**

1. 在開始使用 API 前我們需要申請一組金鑰 key 作為授權碼，先到 [Developer Console's](https://console.developers.google.com/apis/dashboard?project=youtube-search-303812) 啟用 API 和服務。

    ![](https://i.imgur.com/casynZb.png)
    
2. 找到並點選「Youtube Data APIv3」

    ![](https://i.imgur.com/xEAW36F.png)

3. 啟用 API

    ![](https://i.imgur.com/IFodLSA.png)

4. 啟用後進入到 Youtube Data API v3 的管理頁面建立憑證，把產生的金鑰 key 記下來
    ![](https://i.imgur.com/C08kv3G.png)
    
**Request**
    
申請好憑證就可以開始使用 API 了，官方文件 [Search list](https://developers.google.com/youtube/v3/docs/search/list) 中說明了可使用的參數，這邊說明幾個待會會使用到的參數：
* `part`：每個搜尋結果中所包含的資訊，頻道名稱、影片敘述、縮圖等，根據手冊將值設為 `snippet`
* `maxResults`：設定每次回傳的數量，範圍 0~50，預設值為 5
* `order`：將搜尋結果排序的方法，分別是
    * `date`：根據創建日期排序
    * `rating`：根據評價排序
    * `relevance`：根據與搜尋的關鍵字相關性排序
    * `title`：根據標題字母順序排序
    * `videoCount`：根據頻道上傳的影片數量排序
    * `viewCount`：根據觀看數排序
* `q`：要搜尋的關鍵字
* `pageToken`：辨認要回傳的結果頁面，`nextPageToken`、`prevPageToken` 可以用來回傳下一頁 / 上一頁的搜尋結果

對搜尋按鈕監聽獲取搜尋的關鍵字，再來將我們需要的參數設定和申請好的金鑰 key 加入 `url` 中並發出請求。

成功之後將回傳的`pageToken` 傳給函式 `generateButtons` 用來產生 下一頁 / 上一頁的按鈕。

```javascript=
let q;

$('#search-btn').click(function(e) {
    e.preventDefault();

    // 清空內容
    $('#results').html('');
    $('#buttons').html('');

    q = $('#search-field').val();
    getVideos(q);
});

function getVideos(q) {
    $.ajax({
        url: `https://youtube.googleapis.com/youtube/v3/search?part=snippet&maxResults=5&order=relevance&q=${q}&key=API_KEY`,
        method: 'GET',
        dataType: 'json',
        success: function(res) {
        
            const nextPageToken = res.nextPageToken;
            const prevPageToken = res.prevPageToken;
            
            $.each(res.items, function(i, item) {
                showViedos(item);
            });
            
            generateButtons(prevPageToken, nextPageToken);
        }
    });
}
```

Response 為

```json=
{
  "kind": "youtube#searchListResponse",
  "etag": etag,
  "nextPageToken": string,
  "prevPageToken": string,
  "regionCode": string,
  "pageInfo": {
    "totalResults": integer,
    "resultsPerPage": integer
  },
  "items": [
    search Resource
  ]
}
```

其中的 `items` 中為我們所需要的資訊

```json=
{
  "kind": "youtube#searchResult",
  "etag": etag,
  "id": {
    "kind": string,
    "videoId": string,
    "channelId": string,
    "playlistId": string
  },
  "snippet": {
    "publishedAt": datetime,
    "channelId": string,
    "title": string,
    "description": string,
    "thumbnails": {
      (key): {
        "url": string,
        "width": unsigned integer,
        "height": unsigned integer
      }
    },
    "channelTitle": string,
    "liveBroadcastContent": string
  }
}
```

**顯示搜尋結果**

呼叫函式，將回傳的每一筆搜尋結果整理然後顯示在頁面上。

```javascript=
function showViedos(item) {

    // 抓取資料
    const videoID = item.id.videoId;
    const title = item.snippet.title;
    const description = item.snippet.description;
    const thumbnail = item.snippet.thumbnails.high.url;
    const channelTitle = item.snippet.channelTitle;
    const videoDate = item.snippet.publishedAt;

    // 插入 DOM 中
    const result = $('<li></li>');
    result.html(`
        <div class="thumbnail">
            <img src="${thumbnail}" alt="">
        </div>
        <div class="info">
            <h3><a data-fancybox href="http://www.youtube.com/embed/${videoID}">${title}</a></h3>
            <small>By <span class="cTitle">${channelTitle}</span> on ${videoDate.substr(0,10)}</small>
            <p>${description}</p>
        </div>
        `);

    $('#results').append(result);
}
```


### 上一頁 / 下一頁 功能

將得到的 `pageToken` 產生相對應的按鈕，並把值賦予屬性儲存起來，點擊按鈕時執行對應的函式。
>第一頁的搜尋結果回傳的只有 `nextPageToken`，而沒有 `prevPageToken`

```javascript=
function generateButtons(prevPageToken, nextPageToken) {
    if (!prevPageToken) {
        const button = $(`
        <div class="button-container">
            <button id="next-button" class="next-button" data-token="${nextPageToken}" onclick="nextPage()"></button>
        </div>
        `);
        $('#buttons').append(button);
    } else {
        const button = $(`
        <div class="button-container">
            <button id="prev-button" class="prev-button" data-token="${prevPageToken}" onclick="prevPage()"></button>
            <button id="next-button" class="next-button" data-token="${nextPageToken}" onclick="nextPage()"></button>
        </div>
        `);
        $('#buttons').append(button);
    }
}
```

從按鈕中的屬性獲取 `pageToken` 的值，把 `pageToken` 的值加入 `url` 中再次發出請求，重新渲染至頁面，這邊用 next page 說明，prev page 也是相同的概念。

```javascript=
function nextPage() {
    const token = $('#next-button').data('token');

    // 清空內容
    $('#results').html('');
    $('#buttons').html('');

    $.ajax({
        url: `https://youtube.googleapis.com/youtube/v3/search?part=snippet&maxResults=5&order=relevance&q=${q}&pageToken=${token}&key=API_KEY`,
        method: 'GET',
        dataType: 'json',
        success: function(res) {

            const nextPageToken = res.nextPageToken;
            const prevPageToken = res.prevPageToken;

            $.each(res.items, function(i, item) {
                showViedos(item);
            });

            generateButtons(prevPageToken, nextPageToken);
        }
    });
}
```

### Fancybox

每一筆搜尋結果使用 fancybox 播放影片，參考文件中 [fancybox Iframe](http://fancyapps.com/fancybox/3/docs/#iframe) 在連結中加入 `data-fancybox`、`data-type="iframe"` 的屬性。

**HTML**

```html=
<!-- 引入 fancybox -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css" />
<script src="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"></script>    

<!-- 加入 fancybox 屬性 -->
<h3><a data-fancybox data-type="iframe" href="http://www.youtube.com/embed/${videoID}">${title}</a></h3>
```

**JavaScript**

```javascript=
$('[data-fancybox]').fancybox({
	toolbar  : false,
	smallBtn : true,
	iframe : {
		preload : false // 自動調整 <iframe> 的寬度/高度
	}
})
```


## 參考資料

* [Youtube Data API](https://developers.google.com/youtube/v3/getting-started)
* [fancybox](https://fancyapps.com/fancybox/3/)
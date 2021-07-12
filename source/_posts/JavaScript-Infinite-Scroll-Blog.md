---
title: "[實作] JavaScript - Infinite Scroll Blog"
date: 2021-01-13 00:12:25
tags: JavaScript
---
## 成品

[Infinite Scroll Blog](https://mjeddie.github.io/JavaScript-Projects/Infinite_Scroll_Blog/index.html)

![](https://i.imgur.com/Rn6PyFe.png)

## 介紹

利用 [JSON Server](https://jsonplaceholder.typicode.com/) 產生假文章做無限滾動的練習。
<!-- more -->
## 功能

* 當捲動到頁尾時，會自動讀取( load ) 更多的文章，讀取時有動畫顯示
* 文章搜尋功能，可以篩選符合的文章內容

### 獲取文章資料

使用 `fetch()` 來取得遠端的文章資料，這次改用 `async/await` 來進行改寫，在結構上較易於閱讀。因為檔案為 json 格式，所以在取得檔案後透過 `json()` 處理檔案。

而我們一開始預設顯示的文章數為一頁五篇文章，以方便我們可以滾動頁面。

```javascript=
let limit = 5;
let page = 1;

async function getPosts() {
    const res = await fetch(
        `https://jsonplaceholder.typicode.com/posts?_limit=${limit}&_page=${page}`
    );

    const data = await res.json();

    return data;
}
```

### 設置初始文章

我們用一個函式來處理剛拿到的遠端資料，讓其依照順序一一呈現在網頁上。

1. 每一篇文章都用一個 `<div>` 包起來
2. 增加 class `post` 以套用 CSS 設定
3. 依照順序把文章的標題跟內容放進去

網頁讀取時先呼叫一次，這時候會顯示我們預設的五篇文章數。

```javascript=
async function showPosts() {
    const posts = await getPosts();

    posts.forEach(post => {
        const postEl = $('<div></div>').appendTo(postsContainer);
        postEl.addClass('post');
        postEl.html(`
            <div class="number">${post.id}</div>
            <div class="post-info">
                <h2 class="post-title">${post.title}</h2>
                <p class="post-body">${post.body}</p>
            </div>
        `);
    });
}

showPosts();
```


### 讀取動畫

當滑到頁尾時，我們增加一個讀取的顯示，用 JS 切換 class 中 `opacity` 的屬性，來操控它的顯示。

![](https://i.imgur.com/eNfMhT8.png)

**HTML**

```htmlembedded=
<div class="loader">
    <div class="circle"></div>
    <div class="circle"></div>
    <div class="circle"></div>
</div>
```

**CSS**

```css=
.loader {
    opacity: 0;    /* 隱藏
}
```

```css=
.loader.show {
    opacity: 1;    /* 顯示
}
```

再來我們增加點動畫效果，讓它做到上下的位移，並且是依序跳動的

```css=
.circle {
 animation: bounce 0.5s ease-in infinite;
}

.circle:nth-of-type(2) {
    animation-delay: 0.1s;
}

.circle:nth-of-type(3) {
    animation-delay: 0.2s;
}

@keyframes bounce {
    0%,
    100% {
        transform: translateY(0);
    }
    50% {
        transform: translateY(-10px);
    }
} 
```


### 顯示文章

對瀏覽器中的視窗監控 scroll 事件，當瀏覽器滾動到頁尾時觸發函式，當動畫顯示 1 秒後會移除，接著間隔 0.3 秒後從遠端讀取文章資料並顯示在頁面上。

```javascript=
function showLoading() {
    loading.addClass('show');

    setTimeout(() => {
        loading.removeClass('show');

        setTimeout(() => {
            page++;
            showPosts();
        }, 300);
    }, 1000);
}

$(window).scroll(function() {
    const { scrollTop, scrollHeight, clientHeight } = document.documentElement;

    if (scrollTop + clientHeight >= scrollHeight - 5) {
        showLoading();
    }
});
```

這邊說明一下 `scrollTop`、`clientHeight`、`scrollHeight`  之間的關係

![](https://javascript.info/article/size-and-scroll/metric-all.svg)

* `scrollTop` : 是「內容」被捲動的距離，也就是內容頂端和捲軸頂端的相對距離，代表在有滾動條時，滾動條向下滾動的距離也就是元素頂部被遮住部分的高度。
* `clientHeight` : 包括 padding 但不包括 border、水平滾動條、margin 的元素的高度。
* `scrollHeight` : 和 `clientHeight` 屬性類似，但包含滾動 ( 隱藏 ) 的部分。


了解之後我們知道完整的內容高度 (`scrollHeight`) = 內容頂端和捲軸頂端的距離 (`scrollTop`) + 內容本身高度(`clientHeight`)，所以當滑到頁尾時也就是 (`scrollTop + clientHeight >= scrollHeight - 5`) 會觸發產生新的文章，而之所以會 `scrollHeight -5` 是希望在接近底時就能夠觸發。


### 搜尋文章

透過監聽搜尋欄位的 `input` 觸發搜尋的功能，`indexOf()` 方法會回傳某個指定的字符串值在字符串中首次出現的位置，如果沒有找到則回傳 `-1`，對此我們將符合的文章顯示，不符合的則隱藏，`toUpperCase()` 則是方便我們搜尋，可以忽略大小寫的輸入。

```javascript=
function filterPosts(e) {
    const term = e.target.value.toUpperCase();
    const posts = document.querySelectorAll('.post');

    posts.forEach(post => {
        const title = post.querySelector('.post-title').innerText.toUpperCase();
        const body = post.querySelector('.post-body').innerText.toUpperCase();

        if (title.indexOf(term) > -1 || body.indexOf(term) > -1) {
            post.style.display = 'flex';
        } else {
            post.style.display = 'none';
        }
    });
}

document.getElementById('filter').addEventListener('input', filterPosts);
```


## 參考資料

* [JavaScript Promise 全介紹](https://wcc723.github.io/development/2020/02/16/all-new-promise/)
* [Async function / Await 深度介紹](https://wcc723.github.io/development/2020/10/16/async-await/)
* [CSS 動畫](https://developer.mozilla.org/zh-TW/docs/Web/CSS/CSS_Animations/Using_CSS_animations)
* [Cheetsheet - Element size and scrolling](https://javascript.info/size-and-scroll)
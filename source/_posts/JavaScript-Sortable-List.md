---
title: "[實作] JavaScript - Sortable List"
date: 2021-01-29 18:12:43
tags: JavaScript
---
## 成品

[台灣必吃小吃 TOP10](https://mjeddie.github.io/JavaScript-Projects/Sortable_List/index.html)

![](https://i.imgur.com/zxgcDIA.jpg)

## 介紹

使用 [jQuery UI - Sortable](https://jqueryui.com/sortable/) 做的可拖曳列表練習。
<!-- more -->
## 功能

透過拖曳排序列表中的內容，可檢查清單的排序是否正確。

* 隨機排序列表中的內容
* 拖曳功能
* 檢查排序

### 隨機排序列表中的內容

首先將我們的資料放入陣列中方便後續處理

```javascript=
const famousFood = [
    '牛肉麵',
    '小籠包',
    '滷肉飯',
    '大腸麵線',
    '蚵仔煎',
    '臭豆腐',
    '雞排',
    '珍珠奶茶',
    '刨冰',
    '鳳梨酥'
];
```

接著將剛剛的陣列用 [展開運算子](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Spread_syntax) `...` 將陣列展開成多個資料，然後將每筆資料都用 `<li>` 包起來，再放到各自的 `<ul>` 裡顯示在頁面上。

```javascript=
function creatList() {
    [...famousFood]
        // 排名的 list
        const orderItem = $('<li></li>').attr('data-index', index);
        orderItem.html(`
        <span class="number">${index + 1}</span>
        `)
        // 食物的 list 
        const foodItem = $('<li></li>').attr('data-index', index);
        foodItem.html(`
        <div class="draggable">
            <p class="food-name">${food}</p>
            <i class="fas fa-grip-lines"></i>
        </div>
        `);
        $('#order-list').append(orderItem);
        $('#draggable-list').append(foodItem);
    });
}
```

我們想要打亂每筆資料的排序位置，好讓之後可以進行拖曳排名的動作，利用 `map()` 將原陣列新增屬性 `sort` 並賦予值 `Math.random()` 以方便我們排序，再用 `sort()` 對剛賦予的 `sort` 值去排列，接著用 `map()` 整理陣列。

```javascript=
function creatList() {
[...famousFood]
.map(data => ({ value: data, sort: Math.random() }))
.sort((a, b) => a.sort - b.sort)
.map(data => data.value)
```

### 拖曳功能

我們使用 [jQuery UI - Sortable](https://jqueryui.com/sortable/) 把對象綁定清單的父元素也就是 `<ul>` ，就能對下層的 `<li></li>` 進行拖曳排序的動作。

```javascript=
$('#draggable-list').sortable();
$('#draggable-list').disableSelection();
```

紀錄一下這次學習到相關的事件用法

```javascript=
$('#draggable-list').sortable({
    // 排序開始時觸發
    start: function( event, ui ) {
    // 想要執行的內容...
    },
    // 停止排序且 DOM 位置更改時觸發
    update: function(e, ui) {
    foodName = ui.item.text(); // 取得被拖曳的元素文字
    }
});
```

想用原生 JavaScript 的話參考 [HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API) 會需要以下幾個事件：

* dragstart : 開始拖曳一個元素時觸發
* dragover : 一個元素被拖曳經過一個有效的放置目標時觸發
* dragenter : 一個元素被拖曳進入一個有效的放置目標時觸發
* dragleave : 一個元素被拖曳離開一個有效的放置目標時觸發
* drop : 一個元素被放置於一個有效的放置目標時觸發


### 檢查排序

這邊我們對清單中的每一個項目 `li` 去比對食物名稱是否和我們原本陣列 `famousFood` 中的相符，相符就加上 `right` 的 class 名稱，文字會變綠色表示正確，不相符就加上 `wrong` 的 class 名稱，文字會變紅色表示錯誤。

```javascript=
function checkOrder() {
    const draggableList = $('#draggable-list li')
    draggableList.each((index, listItem) => {
        const foodName = listItem.querySelector('.draggable').innerText.trim();
        if (foodName !== famousFood[index]) {
            listItem.classList.remove('right');
            listItem.classList.add('wrong');
        } else {
            listItem.classList.remove('wrong');
            listItem.classList.add('right');
        }
    });
}
```

## 參考資料
* [jQuery UI - Sortable](https://jqueryui.com/sortable/)
* [HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)
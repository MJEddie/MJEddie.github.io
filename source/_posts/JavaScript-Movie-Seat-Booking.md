---
title: "[實作] JavaScript - Movie Seat Booking"
date: 2021-01-05 19:30:34
tags: JavaScript
---
## 成品

[Movie Seat Booking](https://mjeddie.github.io/JavaScript-Projects/Movie_Seat_Booking/index.html)

![](https://i.imgur.com/j9Ywtql.jpg)

## 介紹

提供電影和劇院中的座位供選擇以購買門票，並儲存至 `localStorage`，以便在刷新後資料仍存在。
<!-- more -->
## 功能

* 提供電影以及座位選擇
* 選擇座位後，自動計算總金額
* `localStoage` 功能

### 提供電影以及座位選擇

**HTML 架構**

使用 `<select>` `<option>` 標籤做出電影選單，每個選項中 `value` 的值為電影的價格，座位部分用 `<ul>` `<li>` 項目標籤說明座位的狀態，用 `<div>` 去做出每一列的位置 ( 擷取部分程式碼 )。

```html
<div class="movie-container">
    <label>Pick a movie:</label>
    <select id="movie">
         <option value="24">GODZILLA VS KONG ($24)</option>
         <option value="12">DETECTIVE CONAN THE SCARLET ALIBI ($12)</option>
         <option value="18">BATMAN BEGINS ($18)</option>
         <option value="9">SPIDER-MAN ($9)</option>
    </select>
</div>

<ul class="showcase">
    <li>
        <div class="seat"></div>
        <small>N/A</small>
    </li>
    <li>
        <div class="seat selected"></div>
        <small>Selected</small>
    </li>
    <li>
        <div class="seat sold"></div>
        <small>Sold</small>
    </li>
</ul>

<div class="container">
    <div class="screen"></div>
        <div class="row">
        <div class="seat"></div>
        <div class="seat"></div>
        <div class="seat"></div>
        <div class="seat sold"></div>
        <div class="seat sold"></div>
        <div class="seat"></div>
        <div class="seat"></div>
        <div class="seat"></div>
    </div>
</div>
```

**CSS 特效**

這邊記錄一下幾個特別的用法：

* 使用 `nth-of-type()` 選取器來做出兩旁座位的走道

```css=
.seat:nth-of-type(2) {
    margin-right: 18px; /* 左邊兩排座位的走道 */
}

.seat:nth-last-of-type(2) {
    margin-left: 18px; /* 右邊兩排座位的走道 */
}
```

* 使用 `perspective` 搭配螢幕的翻轉、陰影打造 3D 透視效果

```css=
.container {
  perspective: 1000px;    /*3D效果*/
  margin-bottom: 30px;
}

.screen {
  background-color: #fff;
  height: 70px;
  width: 100%;
  margin: 15px 0;
  transform: rotateX(-45deg);    /* 翻轉螢幕 */
  box-shadow: 0 3px 10px rgba(255, 255, 255, 0.7);    /* 陰影效果 */
}
```

### 選擇座位後，自動計算總金額

先對變數進行宣告，其中票價會變動所以用 `let` 宣告，並將值型別轉換為數字

```javascript=
const container = $('.container');
const seats = $('.row .seat:not(.sold)');
const count = $('#count');
const total = $('#total');
const movieSelect = $('#movie');

let ticketPrice = parseInt(movieSelect.val());
```

這邊用函式來處理選擇的座位數和電影價格，將其渲染至頁面上。
透過對電影選單和座位的監聽之後，更新電影的價格以及座位的狀態 (selected)，然後呼叫我們的函式，函式處理的步驟為：

1. 將選擇的座位放入陣列裡：`[...selectedSeats]`
2. 對陣列中的資料進行處理：`map(function(seat) {...})`
3. 得到選擇的位子所代表的索引值：`[...seats].indexOf(seat)`

```javascript=
function updateSelectedCount() {
    const selectedSeats = $('.row .seat.selected');
    const selectedSeatsCount = selectedSeats.length;

    count.text(selectedSeatsCount);
    total.text(selectedSeatsCount * ticketPrice);
}

// Event Listeners
movieSelect.change(function() {
    ticketPrice = parseInt($(this).val());
    updateSelectedCount();
});

container.click(function(e) {
    if ($(e.target).hasClass('seat') && !$(e.target).hasClass('sold')) {
        $(e.target).toggleClass('selected');

        updateSelectedCount();
    }
});
```

### `localStoage` 功能

參考 [MDN locatStorage](https://developer.mozilla.org/zh-TW/docs/Web/API/Window/localStorage) 後得知用 `setItem('key', 'value)'` 將資料儲存至對應的 `key`，之後要讀取的話透過 `setItem()` 輸入 `key` 值可以得到對應的 `value`，我們用一個函式來專門處理資料的儲存。

```javascript=
function setMovieData(movieIndex, moviePrice) {
    localStorage.setItem('selectedMovieIndex', movieIndex);
    localStorage.setItem('selectedMoviePrice', moviePrice);
}
```
將剛剛選擇的電影和位置儲存至 `localStorage`，處理的步驟為：
1. 將選擇的座位放入陣列裡：`[...selectedSeats]`
2. 對陣列中的資料進行處理：`map(function(seat) {...})`
3. 得到選擇的位子所代表的索引值：`[...seats].indexOf(seat)`

其中因為儲存時會被轉成字串格式，如果儲存的資料為物件時，之後讀取資料時會回傳奇怪的字串，我們用 `JSON` 格式來避免這問題，使用 `JSON.stringify()` 將資料轉為 `JSON` 格式的字串。

```javascript=
function updateSelectedCount() {
    const selectedSeats = $('.row .seat.selected');
    const seatsIndex = [...selectedSeats].map(function(seat) {
        return [...seats].indexOf(seat);
    });

    localStorage.setItem('selectedSeats', JSON.stringify(seatsIndex));

    const selectedSeatsCount = selectedSeats.length;

    if (selectedSeatsCount > 1) {
        $('#seat').text('seats');
    } else {
        $('#seat').text('seat');
    }

    count.text(selectedSeatsCount);
    total.text(selectedSeatsCount * ticketPrice);

    setMovieData(movieSelect.get(0).selectedIndex, movieSelect.val());
}
```

**讀取 `locatStorage` 中的資料**

使用 `JSON.parse` 將資料由 `JSON` 格式字串轉回原本的資料內容及型別，並檢查是否有新點選的位置，渲染至頁面上。
1. 判斷 `selectedSeats` 有值且不為空值：`if (selectedSeats !== null && selectedSeats.length > 0)`
2. 對尚未售出的位置中檢查是否包含其索引值：`seats.each((index, seat) => {
            if (selectedSeats.indexOf(index) > -1)
            }`
3. 將 `class` 狀態變更為 `selected`：`seat.classList.add('selected');`

```javascript=
function populateUI() {
    const selectedSeats = JSON.parse(localStorage.getItem('selectedSeats'));

    if (selectedSeats !== null && selectedSeats.length > 0) {
        seats.each((index, seat) => {
            if (selectedSeats.indexOf(index) > -1) {
                seat.classList.add('selected');
            }
        });
    }

    const selectedMovieIndex = localStorage.getItem('selectedMovieIndex');

    if (selectedMovieIndex !== null) {
        movieSelect.get(0).selectedIndex = selectedMovieIndex;
    }
}
```

## 參考資料

* [CSS沒有極限 - CSS transform-3D的透視(perspective)](https://ithelp.ithome.com.tw/articles/10136368)
* [MDN locatStorage](https://developer.mozilla.org/zh-TW/docs/Web/API/Window/localStorage)
* [JS30-Day15-LocalStorage](https://ithelp.ithome.com.tw/articles/10195522)
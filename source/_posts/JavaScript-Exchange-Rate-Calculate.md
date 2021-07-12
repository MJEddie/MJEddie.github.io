---
title: "[實作] JavaScript - Exchange Rate Calculate"
date: 2021-01-06 21:37:53
tags: [JavaScript, AJAX]
---
## 成品

[Exchange Rate Calculate](https://mjeddie.github.io/JavaScript-Projects/Exchange_Rate_Calculator/index.html)

![](https://i.imgur.com/Ik3nKHt.png)

## 介紹

串接 API 並用 jQuery 語法做的匯率轉換器練習。
<!-- more -->
## 功能

* 可選擇貨幣，並顯示匯率
* 選擇貨幣與金額後，轉換出對應的金額
* 可以快速交換兩種貨幣

## HTML 架構

使用 `<select>` `<option>` 標籤做出下拉選單的樣式，每個選項中 `value` 為其代表的貨幣， `currency-one` 為我們選擇的貨幣，`currency-two` 則是我們想要兌換的貨幣，中間有 Swap 按鈕可以快速交換兩個貨幣的匯率。

```htmlembedded=
<div class="container">
    <div class="currency">
        <select id="currency-one">
            <option value="TWD" selected>台幣 (TWD)</option>
            <option value="USD">美金 (USD)</option>
            <option value="HKD">港幣 (HKD)</option>
            <option value="GBP">英鎊 (GBP)</option>
            <option>...</option>
            <option>...</option>
            <option>...</option>
        </select>
        <input type="number" id="amount-one" placeholder="0" value="1" />
    </div>

    <div class="swap-rate-container">
        <button class="btn" id="swap">
            Swap
        </button>
        <div class="rate" id="rate"></div>
    </div>

    <div class="currency">
        <select id="currency-two">
            <option value="TWD">台幣 (TWD)</option>
            <option value="USD" selected>美金 (USD)</option>
            <option value="HKD">港幣 (HKD)</option>
            <option value="GBP">英鎊 (GBP)</option>
            <option>...</option>
            <option>...</option>
            <option>...</option>
        </select>
        <input type="number" id="amount-two" placeholder="0"/>
    </div>
</div>
```


### 匯率計算

使用 `fetch()` 來取得遠端各個貨幣對應其他貨幣的兌換率，`then()` 做為下一步，資料取得後可在 `then()` 裡面接收，因為檔案為 json 格式，所以在取得檔案後透過 `json()` 處理檔案，接著傳遞到下一層，就可以顯示出匯率以及兌換後的金額。

```javascript=
function calculate() {
    const currencyOne = $('#currency-one').val();
    const currencyTwo = $('#currency-two').val();

    fetch(`https://api.exchangerate-api.com/v4/latest/${currencyOne}`)
        .then(res => res.json())
        .then(data => {
            // console.log(data);
            const rate = data.rates[currencyTwo];

            $('#rate').text(`1 ${currencyOne} = ${rate} ${currencyTwo}`);

            $('#amount-two').val(($('#amount-one').val() * rate).toFixed(2));
        });
}
```

### 事件監聽

當我們改變兌換的金額或是貨幣時，可以做到即時的顯示，對此我們需要監聽選擇的貨幣以及輸入的金額，並在其值改變時再次呼叫函式做匯率的計算，已達到即時顯示的功能。

```javascript=
$('#currency-one').change(function() {
    calculate();
});
$('#currency-two').change(function() {
    calculate();
});
$('#amount-one').change(function() {
    calculate();
});
```

### 交換貨幣

點擊 SWAP 按鈕時把要選擇的貨幣 ( 貨幣 1) 與想要兌換的貨幣( 貨幣 2)做交換，交換完再次呼叫函式計算兌換的金額。

1. 把貨幣 1 的匯率存到變數 `temp`
2. 貨幣 2 的匯率改為貨幣 1 的匯率
3. 再把原本 `temp` 中貨幣 1 的匯率存到貨幣 2
4. 再次呼叫函式計算兌換的金額


```javascript=
$('#swap').click(function() {
    const temp = $('#currency-one').val();
    $('#currency-one').val($('#currency-two').val());
    $('#currency-two').val(temp);
    calculate();
})
```


## 參考資料

* [Fetch API](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API/Using_Fetch)
* [Fetch Response](https://developer.mozilla.org/zh-TW/docs/Web/API/Response)
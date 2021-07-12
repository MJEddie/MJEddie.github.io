---
title: "[實作] AJAX - Weather Forecast"
date: 2021-02-03 20:55:18
tags: [JavaScript, AJAX]
---
## 成品

[Weather Forecast](https://mjeddie.github.io/JavaScript-Projects/Weather_Forecast/index.html)

![](https://i.imgur.com/bsGyPBD.jpg)

## 介紹

串接 [中央氣象局 API](https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/%E9%A0%90%E5%A0%B1) 獲取台灣未來一週的天氣預報，並依照不同的天氣情況顯示代表的圖示。
<!-- more -->
左上角顯示今日天氣情況和氣溫，下面則是接下來一週的天氣預報，右邊選單可以選擇其他縣市。

## 功能

* 下拉式選單
* 顯示今日天氣
* 顯示一週天氣預報
* 天氣情況圖示
* 星期顯示

### 下拉式選單

**從 API 獲取天氣資訊**

[中央氣象局 API](https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/%E9%A0%90%E5%A0%B1) 提供許多不同地區以及天數的預測資料給我們使用，根據需求我們選擇[鄉鎮天氣預報-臺灣未來1週天氣預報](https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/%E9%A0%90%E5%A0%B1/get_v1_rest_datastore_F_D0047_091)。

![](https://i.imgur.com/49Mw8mu.png)


成功取得資料後整理並先把台灣各地區的城市資料抓出來，並放到下拉式選單中。

```javascript=
function getWeather() {
    $.ajax({
        url: 'https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-091?Authorization=CWB-373C6328-6BF2-41B3-BB3B-147802B82875&format=JSON&locationName=&elementName=&sort=time',
        method: 'GET',
        dataType: 'json',
        success: function(res) {
            data = res.records.locations[0];
            city = data.location;

            selectCity(city);
        }
    });
}
```

用 `for` 迴圈將台灣各地區城市依照資料排序一筆一筆的放到下拉式選單中，這樣我們的選單即完成了。

之後對選單綁定事件監聽，將選取到的城市索引值和資料丟到函式中即可顯示該城市的天氣預報。

```javascript=
function selectCity(data) {
    let select = document.querySelector('#select');
    for (let i = 0; i < data.length; i++) {
        city = data[i].locationName;
        let value = i;
        select.innerHTML += `
            <option value="${value}">${city}</option>
        `;
    }
}

$('#select').change(function() {
    cityIndex = $('#select :selected').val();
    showWeather(data, cityIndex);
    showWeek(data, cityIndex);
});
```

### 顯示今日天氣

整理得到的資料從中擷取我們要的部分，分別是
* `WeatherDescription ` : 天氣情況描述
* `MinT` : 最低溫度
* `MaxT` : 最高溫度

之後用 `Date()` 取得現在時間並將其轉為字串整理後，和剛剛整理好的氣象資訊一起顯示在頁面上。


```javascript=
function getDate() {
    let timeNow = new Date();
    return timeNow;
}

function showWeather(data, cityIndex) {
    $('.today-description').html('');
    cityName = data.location[cityIndex].locationName;
    const weather = data.location[cityIndex].weatherElement;
    const description = weather[6].time[0].elementValue[0].value;
    const minT = `${weather[8].time[0].elementValue[0].value} °C`;
    const maxT = `${weather[12].time[0].elementValue[0].value} °C`;
    let descriptionImg = checkWeather(description);
    let date = getDate().toGMTString();

    $('.today-description').html(`
    <h1>${cityName}</h1>
    <h2>${date.substr(0,16)}</h2>
    ${descriptionImg}
    <div class="today-description">${description} <br>${minT} / ${maxT}</div>
    `)
}
```

### 顯示一週天氣預報

這邊要處理的基本上和顯示今日天氣一樣，只不過我們額外用 `<div>` 把每一天的資訊包起來，然後依序增加在頁面上。

```javascript=
function showWeek(data, cityIndex) {
    $('#week').html('');
    const weather = data.location[cityIndex].weatherElement;
    for (i = 1; i < 7; i++) {
        let timeIndex = 2 * i;
        const day = $('<div></div>').attr('class', `day-${i}`);
        const description = weather[6].time[timeIndex].elementValue[0].value;
        const minT = `${weather[8].time[timeIndex].elementValue[0].value} °C`;
        const maxT = `${weather[12].time[timeIndex].elementValue[0].value} °C`;
        let descriptionImg = checkWeather(description);
        
        day.html(`
        <h3>${calendar[i].week}</h3>
        <div class="description">
            ${descriptionImg}
        </div>
        <div class="temp">${minT} / ${maxT}</div>
        <div class="week-description">${description}</div>
        `);
        $('#week').append(day);
    }
}
```

### 天氣情況圖示

天氣情況的描述眾多，將其簡短地分成四大類，依據天氣情況則回傳對應的圖示。

* 晴時多雲 : ![](https://i.imgur.com/T3AAv99.png)
* 多雲時晴、多雲 : ![](https://i.imgur.com/aEBN5Cm.png)

* 多雲時陰、陰時多雲、陰天 : ![](https://i.imgur.com/sC57V0c.png)

* 其他有下雨的情況 : ![](https://i.imgur.com/BQJHA3d.png)


```javascript=
function checkWeather(description) {
    if (description === '晴時多雲') {
        return '<img src="images/svg/sunny.svg" alt="weather-img">';
    } else if (description === '多雲時晴' || description === '多雲') {
        return '<img src="images/svg/sun-cloudy.svg" alt="weather-img">';
    } else if (description === '多雲時陰' || description === '陰時多雲' || description === '陰天') {
        return '<img src="images/svg/cloudy.svg" alt="weather-img">';
    } else {
        return '<img src="images/svg/rainy.svg" alt="weather-img">';
    }
}
```

### 星期顯示

這邊我們要獲得從今天往後的星期，使用的原理是這樣 :
1. 先取得今天的時間
2. 取得明天的時間
3. 獲得明天時間的日期、星期
4. 儲存在陣列中使用

一樣的邏輯就可以取得接下來一週的日期跟星期，我們另外存在陣列中方便使用。

```javascript=
const calendar = [];
const weekDay = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    for (i = 1; i < 7; i++) {
        calendar[i] = {}
        // 取得今天的時間
        let loopDate = new Date();
        
        // 取得明天的時間
        let loopDay = loopDate.getDate() + i;
        let loopValue = loopDate.setDate(loopDay)
        
        // 取的明天時間的日期跟星期
        let newDate = new Date(loopValue)
        
        // 儲存起來使用
        calendar[i].day = newDate.getDate()
        calendar[i].week = weekDay[newDate.getDay()]
    }
```

## 參考資料

[預報因子欄位中文說明表](https://opendata.cwb.gov.tw/opendatadoc/MFC/D0047.pdf)
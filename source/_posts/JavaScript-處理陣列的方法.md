---
title: "[筆記] JavaScript - 處理陣列的方法"
date: 2021-05-26 23:38:42
tags: JavaScript
---
![](/uploads/note.jpg)
在學習 JavaScript 陣列時，看到六角學院 [JavaScript 陣列處理必學巧技](https://www.youtube.com/watch?v=_vFuDQ_6Xt8) 直播中的範例講解後，能更簡單的理解 JavaScript 中陣列的處理方法，特別記錄下來。
<!-- more -->
## 幾種常用的陣列 prototype methods

在說明陣列的方法之前，分享一張陣列方法的 cheat sheet，能更快速的理解關於陣列的 prototype methods。
![Photo by Tomek Sułkowski on Twitter](https://pbs.twimg.com/media/Ecj2lLiXgAAIQUr?format=png&name=4096x4096)

### `forEach()`

`forEach()` 會將陣列內的每個元素，接傳入並執行給定的函式一次。

**語法**

```javascript=
arr.forEach(function callback(currentValue[, index[, array]]) {
    //your iterator
}[, thisArg]);
```

`forEach()` 本身可帶兩個參數，第一個是必須的 callback 函式，第二個參數thisArg 是可選擇性的。這個callback 函式會將 Array 中的每一個元素作為參數，帶進這個 callback 函式裡，每個元素各執行一次。

callback 函式中可傳入三個參數：

* `currentValue`：目前被處理的陣列元素值
* `index`：目前被處理的陣列元素索引 ( 選擇性 )
* `array` 呼叫 `forEach()` 陣列本身( 選擇性 )

如果我們只需要陣列的值，也可寫成只有一個參數的函式，其他的會被忽略。

**範例**

```javascript=
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

### `map()` 

`map()` 會建立一個新的陣列，其內容為原陣列的每一個元素經由回呼函式運算後所回傳的結果之集合。

**語法**

```javascript=
let new_array = arr.map(function callback( currentValue[, index[, array]]) {
    // return element for new_array
}[, thisArg])
```

callback 函式中可傳入三個參數：

* `currentValue`：目前被處理的陣列元素值
* `index`：目前被處理的陣列元素索引 ( 選擇性 )
* `array` 呼叫 `map()` 陣列本身( 選擇性 )

**範例**

```javascript=
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]

```

### `filter()` 

`filter()` 會建立一個經指定之函式運算後，由原陣列中通過該函式檢驗之元素所構成的新陣列。

**語法**

```javascript=
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```

callback 函式中可傳入三個參數：

* `element`：目前被處理的陣列元素值
* `index`：目前被處理的陣列元素索引 ( 選擇性 )
* `array` 呼叫 `filter()` 陣列本身( 選擇性 )

**範例**

```javascript=
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```

### `findIndex()`

`findIndex()` 方法將依據提供的測試函式，尋找陣列中符合的元素，並返回其 index（索引）。如果沒有符合的對象，將返回 `-1` 。

**語法**

```javascript=
arr.findIndex(callback[, thisArg])
```

callback 函式中可傳入三個參數：

* `element`：當前元素
* `index`：當前元素的索引 ( 選擇性 )
* `array` 呼叫 `findIndex()` 陣列本身( 選擇性 )

**範例**

```javascript=
const array1 = [5, 12, 8, 130, 44];

const isLargeNumber = (element) => element > 13;

console.log(array1.findIndex(isLargeNumber));
// expected output: 3
```

### `join()`

`join()` 會將所有陣列中的元素轉成字串型態後，連接合併成一個字串。我們可以指定額外的字串，用來分隔每個字串中的元素，如果沒有給指定的分隔符號字串，`join()` 就會用預設的逗號來當分隔符號。任何 `undefined` 或 `null` 的元素都會被視為空字串處理。

**語法**

```javascript=
arr.join([separator])
```

**範例**

```javascript=
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```

### `reduce()`

`reduce()` 會對陣列中的每一個元素，由左至右傳入指定的函數，最後返回一個累加 (accumulator) 的值。

**語法**

```javascript=
arr.reduce(callback[accumulator, currentValue, currentIndex, array], initialValue)
```
callback 函式中傳入的參數分別是：
* `accumulator`：目前的累加值，一開始的預設值是 `initialValue`，接下來的 `accumulator` 值就是每一次函數執行後返回的結果
* `currentValue`：目前執行到的元素
* `currentIndex`：目前執行到的元素索引值 ( 選擇性 )
* `array`：呼叫 `reduce()` 方法的陣列 ( 選擇性 )
* `initialValue`：第一次呼叫時要傳入的累加器初始值 ( 選擇性 )
reduce() 方法會返回所有元素最後累加的結果

**範例**

```javascript=
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

### `sort()`

`sort()` 會對一個陣列的所有元素進行排序，並回傳此陣列。

**語法**

```javascript=
arr.sort([compareFunction])
```
`sort()` 預設會將元素轉型成字串再做比較，比較的方式是從左到右逐一比對元素中的每個字元的 Unicode code point 大小。

如果希望完全依照自己給的條件排序，最好是給 `sort()` 一個帶有條件的匿名函式來當參數，這個匿名函數必須要有兩個參數，然後再依照這兩個參數比較回傳的值，來當排序依據。

`sort()` 會依匿名函式的參數與回傳的值為精確的排序規則：
* 當回傳值為負數時，那麼前面的數放在前面
* 當回傳值為正整數，那麼後面的數在前面
* 當回傳值為零，保持不動

**範例**

```javascript=
// 沒有給參數的預設排序
const arr = [5, 9, 1, 3, 2, 6];
arr.sort();  // [1, 2, 3, 5, 6, 9]

// 以匿名函式回參數做「升序」排序
arr.sort(function(a, b) {
  return a - b; // a - b > 0
});
// [1, 2, 3, 5, 6, 9]

// 如果要反過來做「降序」排序
arr.sort(function(a, b) {
  return b - a;
});
// [9, 6, 5, 3, 2, 1]
```

## 範例說明

情境中總共有七個題目，會先使用 `forEach()` 完成，另外提供較進階精簡的寫法來完成。

### 情境

卡斯伯一行人決定到麵店內用，點了以上餐點以後...
1. 請一一列出每個人的訂單
2. 小明看到今天有打八折！！，請將所有訂單新增一個新價格，金額是 80%
3. 老闆說，今天疫情沒有八折啦，不過 80 元的可以給滷蛋
4. 過一段時間後，老闆發現牛肉沒了，把點牛肉麵的換成牛肉湯麵
5. 老闆說 POS 機壞了，麻煩幫忙出一下 LI 結構，方便列印發票
6. 老闆要收錢了，請問老闆應該收多少錢
7. 今天誰吃最貴！請排序所有的金額

**基本資料**

```javascript=
const people = [ 
  { 
    name: '卡斯伯',
    order: '鍋燒意麵',
    price: 80
  },
  {
    name: '小明',
    order: '牛肉麵',
    price: 120
  },
  {
    name: '漂亮阿姨',
    order: '滷味切盤',
    price: 40
  },
  {
    name: 'Ray',
    order: '大麻醬乾麵',
    price: 60
  },
];
```

### 1. 請一一列出每個人的訂單

```javascript=
people.forEach(function(item, index) {
    console.log(item, index);
});
```

### 2. 小明看到今天有打八折！！，請將所有訂單新增一個新價格，金額是 80%

#### 2.1 `forEach()`

```javascript=
const newOrders = [];
people.forEach(function(item,index){
     newOrders[index] = {
         ...item,
         newPrice: item.price * 0.8,
     }
 });
 
console.log(newOrders);
```

#### 2.2 `map()`

```javascript=
const newOrders = people.map(function(item, index){
    return{
        ...item,
        newPrice: item.price * 0.8,
    }
});

console.log(newOrders);
```

#### 2.3 `map()` + 箭頭函式

```javascript=
const newOrders = people.map((item, index) => ({
    ...item,
    newPrice: item.price * 0.8
}));

console.log(newOrders);
```

### 3. 老闆說，今天疫情沒有八折啦，不過 80 元的可以給滷蛋

#### 3.1 `forEach()`

```javascript=
const newOrders = [];
people.forEach(function(item, index) {
    if (item.price >= 80) {
        newOrders.push(item)
    }
});
 
console.log(newOrders);
```

#### 3.2 `filter()`

```javascript=
const newOrders = people.filter(function(item, index) {
    return item.price >= 80; // 為真的，就會回傳該物件
});
```

#### 3.3 `filter()` + 箭頭函式

```javascript=
const newOrders = people.filter((item) => item.price >= 80);

console.log(newOrders);
```

### 4. 過一段時間後，老闆發現牛肉沒了，把點牛肉麵的換成牛肉湯麵

#### 4.1 `forEach()`

```javascript=
people.forEach(function(obj, key) {
    if (obj.order === '牛肉麵') {
        index = key;
    }
});

people[index].order = '牛肉湯麵';
console.log(people[index]);
```

#### 4.2 `map()`

```javascript=
const index = people.findIndex(function(item){
    return item.order === "牛肉麵";
});

people[index].order = "牛肉湯麵";
console.log(people[index]);
```

#### 4.3 `map()` + 箭頭函式

```javascript=
const index = people.findIndex((item)=>item.order === "牛肉麵");
people[index].order = "牛肉湯麵";
console.log(people[index]);
```

### 5. 老闆說 POS 機壞了，麻煩幫忙出一下 LI 結構，方便列印發票

#### 5.1 `forEach()`

```javascript=
let htmlTemplate = '';
people.forEach(function(item, index) {
    htmlTemplate = htmlTemplate + `<li>
      ${item.order}, ${item.price}
    </li>`;
});

console.log(htmlTemplate);
```

#### 5.2 `map()` 

```javascript=
const htmlTemplate = people.map(function(item, index) {
    return `<li>
    ${item.order}, ${item.price}
  </li>`;
}).join('');

console.log(htmlTemplate);
```

#### 5.3 `map()` + 箭頭函式

```javascript=
const htmlTemplate = people.map((item, index) => `<li>
  ${item.order}, ${item.price}
</li>`).join('');

console.log(htmlTemplate);
```

### 6. 老闆要收錢了，請問老闆應該收多少錢

#### 6.1 `forEach()`

```javascript=
let total = 0;
people.forEach(function(item, index) {
    total += item.price;
});

console.log(total);
```

#### 6.2 `reduce()`

```javascript=
let total = people.reduce(function(acc, cur) { 
    return acc + cur.price;
}, 0);

console.log(total);
```

#### 6.3 `map()` + 箭頭函式

```javascript=
let total = people.reduce((acc,cur)=>(acc += cur.price), 0);

console.log(total);
```

### 7. 今天誰吃最貴！請排序所有的金額

```javascript=
const peopleSort = people.sort((a, b) => {
    return a.price - b.price 
});

console.log(peopleSort);
```

## 參考資料
* [Array - JavaScript | MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array)
* [JavaScript 陣列處理必學巧技 - YouTube](https://www.youtube.com/watch?v=_vFuDQ_6Xt8)
* [JS 將陣列 Array 重新排列的 sort() - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10225733)


###### tags: `JavaScript`

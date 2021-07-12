---
title: "[筆記] JavaScript - Basic"
date: 2020-12-13 21:14:52
tags: JavaScript
---
![](/uploads/note.jpg)
## 什麼是 JavaScript？

* JavaScript是一門基於原型、函式先行的語言，支援物件導向設計
* JavaScript 不是 Java，除了句法上有一些相似之處，JavaScript 和 Java 是完全不相干的
<!-- more -->
    >1995 年時，JavaScript 隨著 Netscape 2.0 首次推出，它原本要被命名為 LiveScript，但因為行銷策略為了強調昇陽的 Java 程式語言的普遍性，改名為 JavaScript，之後就造成了大家的混淆。

* 讓你在網頁中提供動態的功能，像是內容即時更新、影片播放、控制圖片等

* 瀏覽器唯一指定內建程式語言

### 概論

以下引用自 [Wikipedia](https://zh.wikipedia.org/w/index.php?title=JavaScript&variant=zh-tw)

一般來說，完整的JavaScript包括以下幾個部分：

* ECMAScript，描述了該語言的語法和基本物件
* 文件物件模型（DOM），描述處理網頁內容的方法和介面
* 瀏覽器物件模型（BOM），描述與瀏覽器進行互動的方法和介面

JavaScript的基本特點如下：

* 是一種解釋性程式語言（代碼不進行預編譯）
* 主要用來向HTML頁面添加互動行為
* 可以直接嵌入HTML頁面，但寫成單獨的js檔案有利於結構和行為的分離

JavaScript常用來完成以下任務：

* 嵌入動態文字於HTML頁面
* 對瀏覽器事件作出回應
* 讀寫HTML元素
* 在資料被提交到伺服器之前驗證資料
* 檢測訪客的瀏覽器資訊
* 控制cookie，包括建立和修改等


### 語言特性

* 動態型別語言：
* 弱型別語言：不用特別宣告變數的型別，在運作時會自動轉換，比如直接將整數變數與字串變數相加
* 物件導向
    * Primitive Types
    * Object Types


### HTTP 請求方法與 HTTP 動詞

* 用動詞標準化動作 (HTTP Method)
    * GET
    * POST
    * DELETE
    * PUT
    * PATCH

* 用狀態碼標準化結果 (HTTP Status code)
    * 1xx: 稍等
    * 2xx: 成功
    * 3xx: 重新導向 (301 永久導向、302 暫時導向、304 未修改)
    * 4xx: Client 端錯誤 (404 拒絕存取、403 禁止使用、404 找不到)
    * 5xx: Server 端錯誤



## 變數 (Variable)

變數是用來儲存資料和進行運算的基本單位，在 JavaScript 中變數宣告有一定的規則，變數的第一個字母必須為英文字母、底線 `_` 或是錢字號 `$` ，後面可以是英文字母、底線 `_` 或是錢字號 `$` 以及數字。 變數名稱不可以是保留字 (Reserved Words) 與關鍵字 (keyword)

>關鍵字指的是 ECMAScript 所規定具有特定用途的英文單字，不能用來作為變數名稱使用。 而保留字則是雖然目前在 JavaScript 還沒有特殊用途，但在未來有可能會被拿來當關鍵字來使用，所以也不能作為變數名稱。 [ MDN 關鍵字與保留字列表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)


### 資料型別 (Data types)

* 六種基本 (primitives) 資料型別 :
    * Number : 整數或是帶有小數點的浮點數
        * 特殊型 : Infinity、-Infinity、NaN (Not a Number)
    * String : 字串，用一組 `' '` 或 `" "` 包住，不可混用，ES6 可以用 `` 來宣告
    * Boolean : `true` and `false`
    * null : 表示空值的特殊值
    * undefined : 表示值還沒有定義或還未指定
    * Symbol : ES6 新語法，用來建立獨一無二的值

* 物件型別 (Object): 除基本型別外都是物件型別

可以透過 `typeof` 運算子來判斷型別

```javascript=
typeof true;    // 'boolean'
typeof '123';   // 'string'
typeof 123;     // 'number'
typeof [ ];     // 'object'
```

#### 浮點數的陷阱

JavaScript 的 number 是基於「[IEEE 754](https://zh.wikipedia.org/wiki/IEEE_754)」二進位浮點數算術標準，所以當你執行 `0.1 + 0.2 === 0.3` 的時候，你會得到 `false` 的結果，不過執行 `0.2 + 0.3 === 0.5` 會得到 `true`的結果。
>因為十進位的小數無法完美的用二進位的方式表示，只能用無限循環的位數來趨近於十進位的小數，若以 IEEE 754 規定的 24 位數為上限時，在儲存時就會省略一些位數，導致還原時的小數不夠精準。


## 運算式與運算子 (Expression & Operator)

JavaScript 的語法基本上可以分為兩大類，「敘述句 (Statement)」 與 「運算式 (Expression)」

* 敘述句 (Statement)：簡單來說就是執行某個動作。像是變數的宣告、賦值，迴圈和 if 判斷式等等都可以被歸類於此。

```javascript=
var a;
```

* 運算式 (Expression)：而運算式最大的特性，就是它會產生一個「值」。像是我們在呼叫 function 時的參數 (arguments)，或者透過 `=` 賦值時，在 `=` 「右側」的部分都屬於運算式的部分。

```javascript=
var a = 10 * 10;
```
`=` 右側的 10 * 10 就是運算式，而 `10` 則為運算元


### 運算子 (Operators)

* **算術運算子 (Arithmetic Operators)**：以數值 ( 文字或變數也可以 )作為其運算元，並回傳單一數值。最常見的算術運算元是 加法 (+)，減法 (-)， 乘法 (*)，及除法 (/)。

    ![](https://i.imgur.com/W2ikUBn.png)


* **賦值運算子 (Assignment Operators)**：將基於其右方的運算元的值賦予其左方的運算元。
 
  ![](https://i.imgur.com/1woJMOM.png)
  
  利用 `||=` 給預設值
  
  ```javascript
  var a;
  a ||= 10;
  console.log('預設值是:', a);
  ```
  
* **比較運算子 (Comparison Operators)**：會比較運算元並基於比較的結果回傳邏輯值。 運算元可以是數字，字串，邏輯，或物件的值。當兩個運算元不具有相同型態， JavaScript 會嘗試將它們轉換成相同型態。通常是將運算元以數學形式對待。

  ![](https://i.imgur.com/mUDdQt8.png) 

* **邏輯運算子 (Logical Operator)**：使用於布林值時，會回傳布林型態的值。 然而 && 和 || 運算子實際上是回傳兩指定運算元之一，因此用於非布林型態值時，它可能會回傳一個非布林型態的值。 
    * && (AND) : 假如運算式1可以被轉換成 `false` 的話，回傳運算式1; 否則回傳運算式2。 只有在兩個運算元都是`true` 時才會回傳 `true`，否則回傳 `false`
    * || (OR) : 假如運算式1可以被轉換成 `true` 的話，回傳運算式1; 否則回傳運算式2。 || 在兩個運算元有任一個是 `true` 時就會回傳 `true`，否則回傳 `false`
    * ! (NOT) : 假如單一個運算元能被轉換成 `true` 時，回傳 `false` ， 不然回傳 `true`

* **三元運算子 Conditional (ternary) operator**

    >condition ? exprIfTrue : exprIfFalse

    `condition` : 用來作為條件的表達式
    `exprIfTrue` : 如果 `condition` 的值是 `true` ， `exprIfTrue` 會被執行
    `exprIfFalse` : 如果 `condition` 的值是 `false` ， `exprIfFalse` 會被執行
    
    ```javascript=
    var age = 26;
    var beverage = (age >= 21) ? "Beer" : "Juice";
    console.log(beverage); // "Beer"
    ```


## 控制流程

### if 判斷式

**語法**
```javascript=
if (條件式) {
    陳述式1
} else {
    陳述式2
}
```

當條件成立(為 `true` )的時候會執行 if 陳述式裡的程式，而不成立時則執行另外一個陳述式。

以下轉換成布林值時為 `false`

* `false`
* `undefined`
* `null`
* `0`
* `NaN`
* "" ( 完全沒內容的空字串，只有一個空格也是真值)

使用 if...else

```javascript=
let numA = 5;
let numB = 6;
if (numA === numB) {
    console.log('A等於B'); //不成立 不印
} else {
    console.log('A不等於B'); //印出 A不等於B
}
```

使用 else if

```javascript=
let numA = 5;
let numB = 6;

if (numA === numB) {
    console.log('A等於B'); //不成立 不印
} else if (numA > numB) {
    console.log('A大於B'); //不成立 不印
} else {
    console.log('A小於B'); //印出 A小於B
```


### switch 

**語法**

```javascript=
switch (表達式) {
  case value1:
    //當表達式的值符合 value1
    //要執行的陳述句
    break;
  case value2:
    //當表達式的值符合 value2
    //要執行的陳述句
    break;
  ...
  case valueN:
    //當表達式的值符合 valueN
    //要執行的陳述句
    break;
  default:
    //當表達式的值都不符合上述條件
    //要執行的陳述句
    break;
}
```

範例：

```javascript=
var expr = 'Cherries';
switch (expr) {
  case 'Oranges':
    console.log('Oranges are $0.59 a pound.');
    break;
  case 'Apples':
    console.log('Apples are $0.32 a pound.');
    break;
  case 'Bananas':
    console.log('Bananas are $0.48 a pound.');
    break;
  case 'Cherries':
    console.log('Cherries are $3.00 a pound.');
    break;
  case 'Mangoes':
  case 'Papayas':
    console.log('Mangoes and papayas are $2.79 a pound.');
    break;
  default:
    console.log('Sorry, we are out of ' + expr + '.');
}

console.log("Is there anything else you'd like?");
```

**忘記 `break` 時會發生什麼事**
>當 JavaScript 執行到 break 時，會跳出整個 switch 區塊，繼續往下執行。而如果沒有 break 則程式會從符合的 case 區塊開始，一路往下執行到遇到 break 為止。

範例：

```javascript=
var foo = 0;
switch (foo) {
  case -1:
    console.log('negative 1');
    break;
  case 0: // foo is 0 so criteria met here so this block will run
    console.log(0);
    // NOTE: the forgotten break would have been here
  case 1: // no break statement in 'case 0:' so this case will run as well
    console.log(1);
    break; // it encounters this break so will not continue into 'case 2:'
  case 2:
    console.log(2);
    break;
  default:
    console.log('default');
}
```


### for 迴圈

**語法**

```javascript=
for ([初始表達式]; [條件式]; [遞增表達式]) {
  陳述句
}
```

範例：從 1 數到 10

```javascript=
for (var i = 1; i <=10; i++) {
  console.log(i);
}
```


### while 迴圈

**語法**

```javascript=
while (條件式) {
  陳述句
}
```

用 while 改寫 for 迴圈的範例

```javascript=
var i = 1;

while ( i <= 10 ){
  console.log( i );
  i++;
}
```


## do while 迴圈

先執行一次，再判斷要不要繼續執行

**語法**

```javascript=
do {
  陳述句
}
while (條件式)
```

範例：

```javascript=
var i = 1;
do {
  console.log( i );
  i++;
}
while ( i <= 10 )
```

### break 與 continue

`break` 會直接跳離迴圈

範例：從 1 數到 10，數到 4 就不想數了

```javascript=
for (var i = 1; i <=10; i++) {
  console.log(i);
  if(i === 4)
    break;
}
```

`continue` 會跳過一次，然後繼續下一次迴圈

範例：從 1 數到 10，跳過 4 不數

```javascript=
for (var i = 1; i <=10; i++) {
  if(i === 4)
    continue;
  console.log(i);
}
```


## 函式 (Function)
>Don't Repeat Yourself, DRY
* 函式是將一段或多段程式指令包裝起來重複使用，也方便維護。
* Function 是 JavaScript 的一級物件(first class object)，可以當作別的函數的參數或作為一個變數的值。
 
### 定義函式的方式

**語法**

```javascript=
function 名稱 (參數) {
  // 要執行的功能
}
```

一個函式的定義是由一系列的函式關鍵詞組成，分別為:

1. 函式的名稱
2. 包在`()`中的函式參數列表，參數與參數之間用逗號隔開
3. 包在`{}`中用於定義函式功能的區塊


#### 函式宣告 (Function Declaration)

```javascript=
function square (number) {
    return number * number; 
};
```

#### 函式表達式 (Function Expressions)

將一個函式透過 `=` 指定給某個變數

```javascript=
var squre = function (number) {
    return number * number;
};
```

#### new Function 關鍵字建立函式
 
 
#### 作用域 (Scope) 

在 ES6 以前，只有函式能建立範疇，而在 ES6 之後，可用大括號 `{ ... }` 定義區塊範疇，讓 `const` 和 `let` 宣告以區塊為範疇的變數。

範例：區域變數

```javascript=
function exampleFunction() {
    var x = "declared inside function";  // x 只能在 exampleFunction 函数中使用
    console.log("Inside function");
    console.log(x);
}

exampleFunction();
console.log(x);  // 找不到 x 變數


/*
執行結果
"Inside function"
"declared inside function"
x is not defined 
*/
```

範例：全域變數

```javascript=
var x = "declared outside function";

exampleFunction();

function exampleFunction() {
    console.log("Inside function");
    console.log(x); // 內部找不到 x 會往外找
}

console.log("Outside function");
console.log(x);

/*
執行結果
"Inside function"
"declared inside function"
"Outside function"
"declared outside function"
*/
```


#### 提升 (Hoisting)

變數和函式的宣告會在編譯階段就被先建立一個記憶體空間，等到實際執行時再將值放入記憶體空間內。 ES6 透過 `let` 宣告的變數不會有變數提升的效果。

範例：變數的提升

```javascript=
console.log(x);  // undefined
var x = 10;      
console.log(x);  // 10

/* 
實際運行順序
var x;
console.log(x);
x = 10;
console.log(x);
*/
```


範例：函式的提升
 
```javascript=
catName("Chloe");  // "My cat's name is Chloe"

function catName(name) {
  console.log("My cat's name is " + name);
}
```
 
### 回呼函式 (Callback Function)

把函式當作另一個函式的參數，透過另一個函式來呼叫它

範例:

```javascript=
function greeting(name) {
    alert('Hello ' + name);
}

function processUserInput(callback) {
    var name = prompt('輸入你的名字：');
    callback(name);
}

processUserInput(greeting);
```


### IIFE 函式 (Immediately Invoked Function Expression) 

用 function expression 的方式建立函式後並立即執行它

範例:

```javascript=
var sayHi = function(name) {
    console.log('Hi, ' + name);
}('Nick')
```

不用建立變數的寫法:

```javascript=
(function(name) {
    console.log('Hi, ' + name);
})('Nick')
```



## 物件 (Object)

一個物件可以是零至多種屬性的集合，而屬性是鍵 (key) 與值 (value) 之間的關聯。一個屬性可以是基本型別、物件或是函式。

```javascript=
var person = {
  name : ['Bob', 'Smith'],
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
```


### 物件的建立

* 透過 `new` 關鍵字建立，再一一新增其屬性

```javascript=
var person = new Object();

person.name = 'Bob';
person.age = 32;
person.gender = 'male';
person.interests = ['music', 'skiing'];
person.greeting = function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
}
```

* 使用大括號 `{ }` 建立
>使用大括號 `{ }` 建立，並在建立的同時直接指定屬性至物件內，這種建立物件的方式稱為**物件實字 (Object Literal)**

```javascript=
var person = {
  name : 'Bob',
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};
```

### 物件屬性的存取

1. 透過 `.` 進行存取

```javascript=
var person = {
  name : 'Bob',
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};

person.name;    // 'Bob'
person.age;     // 32
```

2. 透過 中括號 `[ ]` 進行存取

```javascript=
var person = {
  name : 'Bob',
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};

person["name"];    // 'Bob'
person["age"];     // 32
```


#### 屬性刪除

透過 `delete` 關鍵字來刪除

```javascript=
var person = {
  name : 'Bob',
  age : 32,
  gender : 'male',
  interests : ['music', 'skiing'],
  greeting: function() {
    alert('Hi! I\'m ' + this.name[0] + '.');
  }
};

delete person.name;

person.name;    // undefined
```


## 陣列 (Array)

陣列是一種有順序的複合式的資料結構，陣列中可以存放任何值，數字、字串、其他陣列、函式等

### 陣列的建立

**語法**

```javascript=
var arrayName = [item1, item2, ...];
```

範例：

```javascript=
var fruits = ['Apple', 'Banana'];
```

### 陣列的操作

#### 取得陣列的長度

```javascript=
var fruits = ['Apple', 'Banana'];

console.log(fruits.length);    // 2
```

#### 取得元素的索引

```javascript=
var fruits = ['Apple', 'Banana'];

console.log(fruits.indexOf('Apple'));    // 0
```

#### 讀取陣列中的元素

```javascript=
var fruits = ['Apple', 'Banana'];

var first = fruits[0];                   // Apple

var last = fruits[fruits.length - 1];    // Banana
```

#### 更改陣列中的值

```javascript=
var fruits = ['Apple', 'Banana'];

fruits[0] = 'Orange';
fruits[1] = 123;

console.log(fruits);    // ["Orange", 123]
```

#### 新增元素

* 用 `push()` 新增元素到陣列最後面
```javascript=
var fruits = ['Apple', 'Banana'];

fruits.push('Orange');

console.log(fruits);    // ["Apple", "Banana", "Orange"]
```

* 用 `unshift()` 新增元素到陣列最前面

```javascript=
var fruits = ['Apple', 'Banana'];

fruits.unshift('Orange');

console.log(fruits);    // ["Orange", "Apple", "Banana"]
```

#### 刪除元素

* 用 `pop()` 移除陣列中最後一個元素
`pop()` 除了移除元素，還會返回移除的元素值

```javascript=
var fruits = ['Apple', 'Banana'];

var last = fruits.pop();    // Banana 

console.log(fruits);        // ["Apple"]
```

* 用 `shift()` 移除陣列中第一個元素

```javascript=
var fruits = ['Apple', 'Banana'];

var first = fruits.shift();    // Apple

console.log(fruits);           // ["Banana"]
```

#### 擷取陣列中的元素

**語法**

```javascript=
ary.slice(begin, end)
```

* `begin` 表示開始擷取的索引位置
* `end` 表示結束擷取的索引位置，擷取的範圍不包含 `end` 元素，如果 `end` 是負數，表示從陣列後面算起，例如 `-1` 表示最後一個元素的位置

範例：

```javascript=
var fruits = [ 'Apple', 'Banana', 'Orange', 'Lemon', 'Mango'];

var foo1 = fruits.slice(1, 3);

console.log(foo1);    // ["Banana", "Orange"]

var foo2 = fruits.slice(2, -1);

console.log(foo2);    // ["Orange", "Lemon"]
```


## 參考資料 

* [MDN JavaScript](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript)

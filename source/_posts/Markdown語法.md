---
title: "Markdown語法"
date: 2020-11-17 18:13:52
tags: Markdown
---
![](/uploads/note.jpg)

## Markdown 語法是什麼？
* Markdown 是一種輕量級標記式語言，為了實現<font color=#4169E1>易讀易寫</font>的特性 Markdown 語法全由標點符號所組成，讓它們看起來就像是所要表達的意思。
* 因為其易讀易寫的特性，目前許多網站都廣泛使用 Markdown 語法。  
<!-- more -->
## 一、常用功能


### Markdown語法：
```
斜體：在兩邊加上 *星號* 或是 _底線_

粗體：在兩邊加上 **兩個星號** 或是 __兩個底線__

也可以用 **星號 與 _底線_**

刪除線：在兩邊加上 ~~兩個波浪~~

引言(註解)：

> 換行：需在最後面+`兩個空白鍵`··  
> 先以··表示空白
```


### 顯示結果：

斜體：在兩邊加上 *星號* 或是 _底線_

粗體：在兩邊加上 **兩個星號** 或是 __兩個底線__

也可以用 **星號 與 _底線_**

刪除線：在兩邊加上 ~~兩個波浪~~

引用文字(註解)：

> 換行：需在最後面+`兩個空白鍵`··  
> 先以··表示空白

## 二、標題



### Markdown語法：
```
Markdown 提供了六種規格的，分別對應 Html 標籤中的`<h1> ~ <h6>`

# H1
## H2
### H3
#### H4
##### H5
###### H6
```


### 顯示結果：

# H1
## H2
### H3
#### H4
##### H5
###### H6



## 三、清單

Markdown 支援無序清單和有序清單，要在清單下加入段落只要 +tab 就好。
無序清單可使用星號、加號或是減號作為標記：



### Markdown語法：
```
*  無序清單
+  這也是清單
-  這還是清單
```


### 顯示結果：

* 無序清單
+ 這也是清單
- 這還是清單

有序清單則使用數字接一個英文句點：

### Markdown語法：
```
1. 有序清單１
2. 有序清單２
```
### 顯示結果：

1. 有序清單１
2. 有序清單２



## 四、連結



### Markdown語法：
```
* 連結兩邊加上`<` `>`就會產生超連結

    <https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80>  
	
* 名稱兩邊加上`[` `]`然後再連結兩邊加上`(` `)`就可以將連結替換成文字連結

	[傳送門](https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80)
	
* 將`[` `]`前+`!`，則可以產生圖片 (把滑鼠指向圖片可以看到註解）

	![圖片參考名稱](https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80 "傳送門")
```


### 顯示結果：

* 連結兩邊加上`<` `>`就會產生超連結

    <https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80>  
	
* 名稱兩邊加上`[` `]`然後再連結兩邊加上`(` `)`就可以將連結替換成文字連結

    [傳送門](https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80)
	
* 將`[` `]`前+`!`，則可以產生圖片 (把滑鼠指向圖片可以看到註解）

    ![圖片參考名稱](https://images.unsplash.com/photo-1584342864307-03dae979b7db?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=80 "傳送門")
    
    
    
## 五、程式碼

```
行內的 `程式碼` 用 `反引號` 包起來
區塊的 `程式碼` 用 ```三個反引號```包起來
記得要加上語言名稱
```


### Markdown語法：

	```javascript
	var s = "JavaScript Syntax Highlighter";
	alert(s);
	```
 
	```php
	$s = "PHP Syntax Highlighter";
	echo $s;
	```


### 顯示結果：

```javascript
var s = "JavaScript Syntax Highlighter";
alert(s);
```

```php
$s = "PHP Syntax Highlighter";
echo $s;
```


## 參考資料

[Markdown 文件](https://markdown.tw/)  
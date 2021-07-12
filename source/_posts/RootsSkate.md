---
title: "[實作] 手刻切版 - RootsSkate"
date: 2020-11-25 22:37:02
tags: [HTML,CSS]
---
## 成品

[RootsSkate](https://mjeddie.github.io/Web-Projects/RootsSkate/index.html) 

![](https://i.imgur.com/JVbmtW0.jpg)

## Intro 網頁介紹

純 HTML&CSS 切版練習，版面配置參考不少網站後自己編排，排版方面使用 float 進行排版。網站主要為販售滑板的網頁。總共有三個頁面：Home、About、Contact，每個頁面都在右下角有快速返回首頁的按鈕設計。
<!-- more -->
Home：基本 logo 圖、navbar，首圖和標語，接著是商品的資訊，最後則是 footer 以及社群 icon。

About：相同的 logo 圖、 navbar 以及 footer，接著是頁面的首圖與標語、品牌的介紹，人物介紹帶有簡易的動畫以及社群的功能。

Contact：相同的 logo 圖、 navbar 以及 footer，首圖部分則是利用 iframe 嵌入 google 地圖，側邊是關於商店的聯絡資訊以及營業時間，form 表單包含四個 input 一個 textarea，搭配一個 submit 的按鈕。


## Layout Notes 

一開始需要先做 reset 的動作，可以在 CSS 設定或是直接引入 `normalize.css` 檔案來幫我們做 reset 的動作。 `box-sizing: border-box;` 這個設定是讓 padding 以及 border 的修改不會改變到 box model 的大小。

首先我們會做一些全域的設定，像是字型、大小、渲染時的平滑效果以及 ul 的設定。

``` css=
* {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}

body {
    font-family: 'Source Sans Pro', sans-serif;
    font-size: 15px;
    font-weight: 400;
    -webkit-font-smoothing: antialiased;
    -moz-font-smoothing: antialiased;
}

ul {
    margin: 0;
    padding-left: 0;
    list-style-type: none;
}
```



### Home

![](https://i.imgur.com/ooU6DtC.png)

這邊要處理的分別是 logo 圖跟我們的 navbar，圖片的高度跟我們的 navbar 高度一樣而不會超出，另一邊用 ul 包住 li 當作 navbar 的選單，並增加改變顏色的效果，因為使用 float 排版，要分別設定圖片跟 navbar 的浮動方向。 

HTML 架構
``` html=
<div id="header">
    <div class="container clearfix">
        <div id="logo">
            <img src="images/ms-icon-144x144.png" width="112" height="100" alt="Skater">
        </div>
        <div id="nav">
            <ul class="clearfix">
                <li><a href="index.html">home</a></li>
                <li><a href="about.html">about</a></li>
                <li><a href="contact.html">contact</a></li>
                <li><a href="#"><i class="icon-basket"></i>cart</a></li>
            </ul>
        </div>
    </div>
</div>
```
CSS 部分

``` css=
#logo {
    float: left;
}

#nav {
    float: right;
    text-transform: uppercase;
    line-height: 100px;
}

#nav li {
    float: left;
}

#nav a {
    padding: 0 12px;
    color: #333;
    text-decoration: none;
    font-size: 14px;
}

#nav a:hover {
    color: #d6b161;
}
```



![](https://i.imgur.com/Bqxitrt.jpg)

首圖是用整個區塊的背景圖片呈現，加上 h1 標題。

HTML 架構

``` html=
<div class="slider">
    <span>
        <h1>explore &emsp; dare &emsp; wonder</h1>
    </span>
</div>
```

CSS 部分

``` css=
slider {
    background-image: url(../images/cover.jpg);
    background-size: cover;
    background-position: 50% 50%;
    background-repeat: no-repeat;
    width: 100%;
    height: 550px;
    position: relative;
}

.slider span {
    position: absolute;
    margin: 0 auto;
    top: 56%;
    left: 28.5%;
    transform: translateY(-50%);
}

```


![](https://i.imgur.com/gQOb2Q2.png)

商品的 showcase 和首圖概念一樣，用區塊的背景圖片去呈現，為了給左邊留白寬度沒有設定 100%，標題文字則用兩種顏色呈現，加入一個 a 連結的 button，並做了改變顏色的動畫。

HTML 架構

``` html=
<div class="new-collection">
            <div class="container clearfix">
                <div class="title"><span class="golden-title">new</span><br>colle-<br>ction</div>
                <a href="shop.html" class="button link">see all</a>
                <div class="background"></div>
            </div>
        </div>
```

CSS 部分

``` css=
.background {
    background-image: url(../images/collection.jpg);
    background-size: cover;
    background-position: 50% 100%;
    width: 89%;
    height: 450px;
    float: right;
    margin-top: -107px;
}

.new-collection .title {
    position: absolute;
    top: 120px;
    font-size: 70px;
    font-weight: bold;
    color: #000;
    text-transform: uppercase;
    letter-spacing: 15px;
    float: left;
}

.new-collection .golden-title {
    color: #d6b161;
}

.button.link {
    position: relative;
    top: 240px;
    padding: 15px 60px;
    background: #000;
    color: #fff;
    -webkit-transition: 0.2s linear all;
    transition: 0.2s linear all;
}

.button.link:hover {
    background: #d6b161;
    color: #fff;
}
```


![](https://i.imgur.com/7AkCQ2a.png)

商品區塊分為三個欄位，排版上設定 `float:left` `width:33.3%`，內容則加入商品圖片、商品標題跟價格。

HTML 架構

``` html=
<div class="products">
    <ul class="clearfix row">
        <li class="col-4 product">
            <div class="thumbnail">
                <img src="images/product-1.jpg" alt="product">
            </div>
            <div class="info">
                <h3 class="title">
                    <a href="#">World Industries Skateboard Graffiti</a>
                </h3>
                <div class="price">$149.99</div>
            </div>
        </li>
    </ul>
</div>
```

CSS 部分

``` css=
.col-4 {
    float: left;
    width: 33.3333333%;
}
```


![](https://i.imgur.com/GQmLSSs.png)

footer 分為左邊的 copyright 以及右邊的社群連結，一樣要分別設定個別的浮動方向。 社群連結是利用在 a 連結裡面放入圖片的方式呈現，在 css 方面設定 `display:inline-blick` 讓我們設定 `width` 跟 `height` 屬性。

HTML 架構

```html=
<div id="footer">
    <div class="container clearfix">
        <div class="copyright">
            <div class="title">Copyright &copy; Just Skate 2020</div>
            <div class="subtitle">Trademarks belong to their respective owners. All rights reserved.</div>
        </div>

        <div class="social-media">
            <ul class="clearfix">
                <li>
                    <a href="https://www.facebook.com/" target="_blank">
                        <img src="images/social-facebook.png" alt="facebook">
                    </a>
                </li>
                <li>
                    <a href="https://twitter.com/?lang=en" target="_blank">
                        <img src="images/social-twitter.png" alt="twitter">
                    </a>
                </li>
                <li>
                    <a href="https://www.instagram.com/" target="_blank">
                            <img src="images/social-instagram.png" alt="instagram">
                    </a>
                </li>
            </ul>
        </div>
    </div>
</div>
```

CSS 部分

```css=
#footer .copyright {
    float: left;
}

#footer .social-media {
    float: right;
}

#footer .social-media li {
    float: left;
}

#footer .social-media a {
    display: inline-block;
    margin: 10px;
    width: auto;
    height: 20px;
    line-height: 20px;
    text-align: center;
    text-decoration: none;
}
```
### About

![](https://i.imgur.com/SKkrtMT.jpg)

About 頁面的首圖跟標語跟首頁一樣，用整個區塊的背景圖片呈現，加上 h1 標題。

HTML 架構

``` html=
<div class="header">
    <div class="container">
        <h1 class="title">our story</h1>
    </div>
</div>
```

CSS 部分

``` css=
#content .header {
    padding: 200px 0;
    background-image: url(../images/about.jpg);
    background-size: cover;
    background-position: 50% 31%;
}
#content .header .title {
    float: left;
    margin: 0;
    color: #fff;
    text-transform: uppercase;
    font-weight: bold;
    letter-spacing: 2px;
    font-size: 24px;
}
```

![](https://i.imgur.com/dr3IyxA.jpg)

成員介紹分為標題 h2、敘述 p 以及下方的成員欄位，當滑鼠移到成員欄位時會有上移以及變色的效果。

HTML 架構 ( 這邊和首頁商品架構類似，只說明動畫部分 )

``` html=
<div class="wrap">
    <div class="avatar">
        <img src="images/member-1.jpg" alt="avatar">
    </div>
    <div class="info">
        <h3 class="name">Benjamin Parker</h3>
        <div class="title">CEO &amp; Co-Founder</div>
    </div>
    <div class="social-media">
        <ul class="clearfix">
            <li><a href="https://www.facebook.com/" target="_blank"><i class="icon-facebook"></i></a></li>
            <li><a href="https://twitter.com/?lang=en" target="_blank"><i class="icon-twitter"></i></a></li>
            <li><a href="https://www.linkedin.com/" target="_blank"><i class="icon-linkedin"></i></a></li>
        </ul>
    </div>
</div>
```

CSS 動畫部分

``` css=
.about #content .member:hover .title {
    color: #fff;
}

.about #content .member:hover a {
    color: #fff;
}
.col-4.member {
    transform: translateY(0px);
    transition: .5s;
}

.col-4.member:hover {
    transform: translateY(-40px);
}

.col-4.member:hover .wrap {
    background-image: linear-gradient(0deg, #d6b161, #fff);
}
```


### Contact

![](https://i.imgur.com/zuYmXuL.png)

Contact 頁面把首圖換成了 google map，主要是用 `iframe` 這個標籤來呈現，詳細使用方式可以參考[網站嵌入google map技術](https://www.webdesigns.com.tw/Embedded-google-map.asp)，並給予高度跟寬度來調整呈現的樣子。

``` htmlembedded=
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3614.9390098163053!2d121.56572361500625!3d25.03614378397135!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3442ab43ab5a86c1%3A0x400f5859b70ce0b7!2zMTEw5Y-w5YyX5biC5L-h576p5Y2A5p2-5aO96LevMTPomZ8!5e0!3m2!1szh-TW!2stw!4v1573633685991!5m2!1szh-TW!2stw"
            width="100%" height="450" frameborder="0" style="border:0" allowfullscreen></iframe>
```

![](https://i.imgur.com/MoA1hb4.png)

下方分為兩個區塊，分別是商店資訊區塊以及表單的區塊，文字部分是 h3 標題搭配 p 敘述，表單每個 `div` 都有 `label` `input`，在 Name 跟 Email 多加 required 要求一定要輸入相關資訊，寬度給予 100% 並加上 border。 

HTML 架構：商店資訊

```html=
<div class="row">
    <div class="col-4 office-info">
        <h3 class="title">our store</h3>
    <div class="contact-info">
        <p class="address">No. 13, Songshou Rd., Xinyi Dist., Taipei City 110, Taiwan (R.O.C.)</p>
        <div class="info">以下省略</div>
    <h3 class="title">opening hour</h3>
    <div class="contact-info">
        <div class="weekday">Monday - Friday: 11:00 - 19:30</div>
        <div class="holiday">Saturday - Sunday: 11:00 - 20:00</div>
    </div>
 </div>
```

HTML 架構：表單部分

```html=
<div class="col-8 question">
    <h3 class="title">have questions?</h3>
    <p>Please fill in the fields below: * = Required<br>We'll get back to you within 48-72 hrs.</p>
    <form action="#" class="contact-form" method="post">
        <div class="row">
            <div class="col-6">
                <label for="name">name *</label>
                <input type="text" name="name" id="name" required>
            </div>
            <div class="col-6">
                <label for="email">email *</label>
                <input type="text" name="email" id="email" required>
            <div class="col-6">以下省略</div>
            </div>
            <div class="col-12">
                <label for="comments">comments</label>
                <textarea name="comments" id="comments"></textarea>
            </div>
    </form>
</div>
```

CSS 表單部分

``` css=
.contact-form input,
.contact-form textarea {
    padding: 6px 12px;
    border: 1px solid #e5e5e5;
    width: 100%;
}
```


### Go top

![](https://i.imgur.com/dAYrOac.png)

Go-top 快速回到首頁按鈕，用 a 連結回到 `#header` 加上 icon 圖示呈現，`position:fixed` 的元素會相對於瀏覽器視窗來定位，即便頁面捲動，還是會固定在相同的位置。

``` html=
<a href="#header" id="go-top"><i class="icon-go-top"></i></a>
```


``` css=
#go-top {
    position: fixed;
    right: 20px;
    bottom: 35px;
    width: 40px;
    height: 40px;
    background: rgba(0, 0, 0, .6);
    color: #fff;
    text-align: center;
    line-height: 40px;
    text-decoration: none;
}
```
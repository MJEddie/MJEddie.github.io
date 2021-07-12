---
title: "[實作] 手刻切版 - Personal Resume"
date: 2020-11-26 20:51:55
tags: [HTML,CSS]
---
## 成品

[Personal Resume](https://mjeddie.github.io/Web-Projects/Resume/index.html)

![](https://i.imgur.com/t86k83h.png)

## Intro 網頁介紹

這是一個個人作品集的網頁，最上方的 navbar 左側是個人的資訊，右側是文字及超連結跳轉頁面。
<!-- more -->
Home：由個人頭像、標語以及敘述組成，搭配一個背景色以及個人的社群。

About me：個人學歷、經歷區塊，

Portfolio：利用交錯漂浮版面來放置個人作品，一邊是作品的圖示，另一邊說明作品內容。

Contact：分成三個區塊，由 icon、標題以及文字敘述組成。

最後則是footer。

## Layout Notes

開始前一樣先進行 reset 的動作，將所有的 margin、padding 歸零，並設定 `box-sizing:border-box` 讓 padding 和 border 的修改不會改變 box model 的大小。

```css=
* {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: 'Montserrat', sans-serif;
}

ul {
    margin: 0;
    padding-left: 0;
    list-style: none;
}
```

![](https://i.imgur.com/qD5N5OS.png)

Navbar 左側是標題搭配敘述，右邊則是 ul 包住 li 當作 navbar 的選單，並增加改變顏色的效果，因為使用 float 排版，要分別設定標題跟 選單的浮動方向。

HTML 架構

```html=
<div id="header">
    <div class="container clearfix">
        <div id="info">
            <h3 class="name"><b>Nick &thinsp; </b></h3>
            <span class="title">&frasl; &thinsp; Becoming a Front-End Developer</span>
        </div>
        <div id="nav">
            <ul>
                <li><a href="#header">home</a></li>
                <li><a href="#about">about me</a></li>
                <li><a href="#portfolios">portfolio</a></li>
                <li><a href="#contact">contact</a></li>
            </ul>
        </div>
    </div>
</div>
```

CSS 部分

```css=
#info {
    float: left;
    line-height: 40px;
}

#nav {
    float: right;
    text-transform: uppercase;
    line-height: 100px;
}

#nav li {
    float: left;
}
```


### Home

![](https://i.imgur.com/6F3y2ya.png)

接著個人頭像這邊練習用 flex 排版，可以分為背景色的橘色區塊、個人頭像的綠色區塊和標題的藍色區塊，然後把個人頭像的綠色區塊向左偏移達到重疊的效果。個人頭像照片的部分，則是用 `border-radius:50%` 去切出圓形來。

HTML 架構

```html=
<div class="container">
    <div class="bg"></div>
    <div class="profile">
        <div class="avatar">
            <img src="images/avatar.jpg" width="206" height="206" alt="avatar">
            <h3 class="name">Nick</h3>
            <div class="title">Becoming a Front-End Developer</div>
            <div class="social-media">
                <ul class="clearfix">
                    <li><a href="https://www.facebook.com/" target="_blank"><i class="fa fa-facebook"></i></a></li>
                    <li><a href="https://github.com/" target="_blank"><i class="fa fa-github"></i></a></li>
                    <li><a href="https://www.linkedin.com/" target="_blank"><i class="fa fa-linkedin"></i></a></li>
                </ul>
            </div>
        </div>
    </div>
    <div class="self-intro">
        <h2>Hello</h2>
        <div class="subtitle">
            Looking for my next opportunity to make a change.
        </div>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Aspernatur nihil quam cupiditate labore totam nostrum dolor, neque voluptatibus cum accusamus.</p>
    </div>
</div>
```
CSS 部分

```css=
#content>.container {
    display: flex;
    width: 1170px;
    margin: auto;
}

.profile {
    width: 365px;
    position: relative;
    top: 33px;
    left: -25%;
}

.avatar {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 350px;
    height: 404px;
    background-color: #F2ECE8;
    padding-top: 30px;
    box-shadow: 5px 5px 50px #F2ECE8;
}

.self-intro {
    width: 45%;
    position: relative;
    left: -17em;
}

.avatar img {
    border: 1px solid transparent;
    border-radius: 50%;
}
```


### About

![](https://i.imgur.com/MtL7exQ.png)

個人學經歷區塊是鏡像對稱的兩個區塊，在區塊裡包含了時間、標題以及敘述，外邊加上陰影的效果。


HTML 架構

```html=
<div class="row">
    <div class="education">
        <h3 class="title">Education</h3>
        <div class="timeline">
            <div class="timeline-item">
                <h6 class="date">
                    <i class="fa fa-calendar" aria-hidden="true"></i> 2013 - 2015
                </h6>
                <h4 class="title">Master In Computer Science</h4>
                <p class="text">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Adipisci vero commodi reiciendis in rerum accusantium fugit vel nesciunt quos reprehenderit?</p>
                <h6 class="date">
                    <i class="fa fa-calendar" aria-hidden="true"></i> 2011 - 2013
                </h6>
                <h4 class="title">Studie At delhi university</h4>
                <p class="text">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Adipisci vero commodi reiciendis in rerum accusantium fugit vel nesciunt quos reprehenderit?</p>
                <h6 class="date">
                    <i class="fa fa-calendar" aria-hidden="true"></i> 2008 - 2011
                </h6>
                <h4 class="title">Bachelor Degree</h4>
                <p class="text">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Adipisci vero commodi reiciendis in rerum accusantium fugit vel nesciunt quos reprehenderit?</p>
            </div>
        </div>
    </div>
```

CSS 部分

```css=
.education {
    display: flex;
    flex-direction: column;
    width: 50%;
    margin-top: 30px;
    padding: 50px 20px;
}

.timeline {
    background: #fff;
    padding: 30px 15px;
    border: 1px solid #0000002c;
    border-radius: 10px;
    width: 100%;
    position: relative;
    box-shadow: 0px 0px 18px 0 #0000002c;
}
```


## Portfolio

![](https://i.imgur.com/4czk6qv.png)

個人作品集使用交錯排版方式，一邊是作品的縮圖，另一邊說明作品，敘述欄加入了交錯的透明背景色。

HTML 架構

```html=
<div class="container">
    <div class="portfolio">
        <div class="thumbnail">
            <img src="https://picsum.photos/600/350?random=10" alt="portfolio">
        </div>
        <div class="portfolio-info">
            <h2 class="title">portfolio</h2>
            <p class="intro">Lorem ipsum dolor sit amet consectetur adipisicing elit. Corporis, odit.</p>
        </div>
    </div>
    <div class="portfolio">
        <div class="portfolio-info">
            <h2 class="title">portfolio</h2>
            <p class="intro">Lorem ipsum dolor sit amet consectetur adipisicing elit. Corporis, odit.</p>
        </div>
        <div class="thumbnail">
            <img src="https://picsum.photos/600/350?random=10" alt="portfolio">
        </div>
    </div>
</div>
```

CSS 部分

```css=
.portfolio {
    display: flex;
    align-items: center;
    margin-bottom: 70px;
}
.portfolio .thumbnail {
    width: 55%;
    flex-shrink: 0;
}

.portfolio .thumbnail img {
    width: 100%;
    vertical-align: middle;
}

.portfolio-info {
    width: 55%;
    flex-shrink: 0;
    padding: 50px 30px;
    position: relative;
    z-index: 1;
}

.portfolio> :first-child {
    margin-right: -10%;
}

.portfolio:nth-child(2n+1) .portfolio-info {
    background-color: rgba(242, 236, 232, .8);
}

.portfolio:nth-child(2n) .portfolio-info {
    background-color: rgba(223, 225, 229, .8);
}
```


## Contact

![](https://i.imgur.com/q0ezZGP.png)

Contact 部分是三個區塊分別代表三種聯絡方式，用 icon 加入標題和文字來呈現。

```html=
<div class="container">
    <h2 class="catagory">contact &thinsp; <em>info</em></h2>
    <div class="contact method">
        <div class="contact-way">
            <div class="icon">
                <img src="https://img.icons8.com/bubbles/100/000000/cell-phone.png " alt="phone">
            </div>
            <div class="contact-info">
                <h1>Phone</h1>
                <p>TEL: +8862 1234-5678</p>
            </div>
        </div>
        <div class="contact-way">..</div>
        <div class="contact-way">..</div>
    </div>
</div>
```

CSS 部分

```css=
.contact.method {
    display: flex;
    width: 100%;
    flex-direction: row;
    justify-content: space-evenly;
    padding: 100px 0;
}

.contact-way {
    display: flex;
    width: 30%;
    margin: 0;
    padding: 30px;
    flex-direction: row;
    border-radius: 10px;
    box-shadow: 0px 0px 18px 0 #0000002c;
    transition: box-shadow .3s ease;
}

.contact-way:hover {
    box-shadow: 0px 0px 5px 0 #0000002c;
}

```
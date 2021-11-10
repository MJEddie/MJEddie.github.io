---
title: "[實作] Final Project User Story ( 需求分析 )"
date: 2021-9-28 23:03:10
tags:
---
# Final Project - La Vie Verte

## 決定好要製作的網站 

實作網站：[盆栽、多肉植物、塊根植物、観葉植物通販/KIDORI -キドリ-](https://ki-do-ri.jp/)

## 使用使用者故事進行需求分析
<!-- more -->
實作功能如下：

1. 商品 & 商品類別
2. 購物車
3. 購物指南
4. 聯絡我們（留言）
5. 前台登入
6. 後台商品（類別）管理
7. ~~植物圖鑑 ( 確定刪除 )~~

---

1. 商品 & 商品分類：

* 使用者可以進到類別列表
    * 盆景
    * 迷你盆景
    * 多肉植物
    * 塊根植物
    * 觀葉植物
* 使用者可以進入商品資訊頁並將商品加入購物車
    * 
* 使用者將商品加入購物車之後會有 Flash Message 回饋 ( 原網站是將商品加入購物車後會跳轉至購物車畫面 )

![](https://i.imgur.com/kyBN14i.jpg)

![](https://i.imgur.com/pySA4jk.png)

![](https://i.imgur.com/maid1cV.png)


2. 購物車
* 使用者可以新增/刪減商品
* 使用者可以結帳
    * 省略成一頁式，使用 [Boostrap checkout form](https://getbootstrap.com/docs/5.0/examples/checkout/)

![](https://i.imgur.com/nh6Dk9F.png)
![](https://i.imgur.com/l6WSaS8.png)


3. 購物指南
* 使用者可以知道購物相關資訊 ( 付款方法、退換貨、運輸等 )

![](https://i.imgur.com/3uNBvIp.png)

4. 聯絡我們（留言）
* 使用者可以留下資料跟訊息並送出
![](https://i.imgur.com/ZgrF0BQ.png)

5. 前台登入
* 使用者可以登入前台
    * 訂單查詢
    * WISH LIST
    * 修改會員資料
* 使用者可以登入後台，使用 Bootstrap 樣式 ( 暫定 )

![](https://i.imgur.com/p2nPeoi.png)


6. 後台商品（類別）管理

* 使用者可以新增商品
* 使用者可以刪除商品
* 使用者可以瀏覽商品細節
* 使用者可以瀏覽所有商品

7. ~~植物圖鑑 ( 刪除 )~~
* 使用者可以進到類別列表
* 使用者可以進入植物資訊頁，並連結至該商品頁面

![](https://i.imgur.com/pAjkbPl.png)
![](https://i.imgur.com/dZdZfne.png)
![](https://i.imgur.com/YcXPVxg.png)


## 分析頁面流程
![](https://i.imgur.com/CIey33G.png)


## 產品資訊

總共五個類別，每個類別有六項商品，

### 盆景


|                              盆景五葉松                               |                              盆景紫杜松                               |                               真柏 5 號                               |
|:---------------------------------------------------------------------:|:---------------------------------------------------------------------:|:---------------------------------------------------------------------:|
|                 ![](https://i.imgur.com/qxwLHXO.jpg)                  | ![](https://ki-do-ri.jp/upload/save_image/01211129_5c452e9d515cc.jpg) | ![](https://ki-do-ri.jp/upload/save_image/10051238_5bb6dcac64293.jpg) |
|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257029)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257028)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256954)  |
|**盆景石化柏樹 3 號**| **盆景真柏**  |**盆景黑松**|
| ![](https://ki-do-ri.jp/upload/save_image/08141430_5b7269094d73d.jpg) |     ![](https://ki-do-ri.jp/upload/save_image/07101856_5b4482b5d7227.jpg)     |     ![](https://ki-do-ri.jp/upload/save_image/10051113_5bb6c8ae72ca0.jpg)     |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256852)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256794)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256952)|


### 迷你盆景


| 迷你盆景火棘 | 迷你盆景楓葉 | 迷你盆景八木小喬木 |
|:--------:|:--------:|:--------:|
|   ![](https://ki-do-ri.jp/upload/save_image/08031233_5b63cd1e2626b.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08031245_5b63cfcb66d7a.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08031304_5b63d46bcf924.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256841)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256842)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256844)|
| **迷你盆景楓樹** | **迷你盆景台灣山莊** | **金錢樹盆景風格** |
|   ![](https://ki-do-ri.jp/upload/save_image/09031333_5b8cb9a4417d6.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/07101956_5b4490f67e854.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/01111336_5c381d42be3e1.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256891)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256797)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257025)|

### 多肉植物


|                              永恆矮人節                               |                        景天 (Sedum mocinianum)                        |                              大戟孔雀舞                               |
|:---------------------------------------------------------------------:|:---------------------------------------------------------------------:|:---------------------------------------------------------------------:|
| ![](https://ki-do-ri.jp/upload/save_image/11191507_5bf2533ca616d.jpg) | ![](https://ki-do-ri.jp/upload/save_image/11191431_5bf24ac904ea8.jpg) | ![](https://ki-do-ri.jp/upload/save_image/11191642_5bf2696b004cc.jpg) |
|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256987)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256986)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256988)  |
|                            **迷你仙人掌**                             |                           **蒙特維爾雪峰**                            |                          **龍舌蘭竹雪**                           |
| ![](https://ki-do-ri.jp/upload/save_image/04171253_5ad56fad00489.jpg) | ![](https://ki-do-ri.jp/upload/save_image/08171521_5b76696ef17e7.jpg) | ![](https://ki-do-ri.jp/upload/save_image/08242003_599eb2938ea20.jpg) |
|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256743)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256874)  |  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256721)  |
### 塊根植物

| 幸運克蘭妮婭 | 白馬城 | Pakipodium |
|:--------:|:--------:|:--------:|
|   ![](https://ki-do-ri.jp/upload/save_image/11221616_5bf657dddbc74.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/06221123_5b2c5d9c4c38d.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08171903_5b769d777d4c3.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256998)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256753)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256881)|
| **鬼棲閣** | **沙漠蘇木** | **鐵甲丸** |
|   ![](https://ki-do-ri.jp/upload/save_image/08242019_599eb64b5e0b8.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08242016_599eb5a81295f.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/09271905_5bacab52e9efa.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256729)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256728)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256935)|

### 觀葉植物

| 鶴望蘭 | 愛心榕 | 鵝掌藤 |
|:--------:|:--------:|:--------:|
|   ![](https://ki-do-ri.jp/upload/save_image/09151748_5f607ff812609.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/09141528_5f5f0d91abb10.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/09141317_5f5eeed6d109d.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257047)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257046)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257045)|
| **喜悅黃金葛** | **花葉絲蘭** | **南洋杉** |
|   ![](https://ki-do-ri.jp/upload/save_image/02051345_5c5914f25352b.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08242116_599ec3a1d668c.jpg)   |   ![](https://ki-do-ri.jp/upload/save_image/08242118_599ec408a9fc7.jpg)   |
|[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=257035)|  [商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256680)  |[商品頁](https://ki-do-ri.jp/products/detail.php?product_id=256735)|
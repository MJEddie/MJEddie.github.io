---
title: "[筆記] 基礎網路概念 - URL"
date: 2021-04-20 12:33:50
tags: 網路概念
---
![](/uploads/note.jpg)

## 什麼是 URL？

統一資源定位符 (Uniform Resource Locator，URL)，簡稱網址，是網際網路上標準資源的位址 （Address） ，如同網路上的門牌。
<!-- more -->
## URL 的格式

完整的 URL 格式如下圖所示：

![](https://user-images.githubusercontent.com/7014009/114458357-add92b80-9c11-11eb-943e-028544a54fc0.png)

* 通訊協定 / 傳輸協定 (Protocol)：網路中互相通訊且受共同認定的協議標準，常見為 HTTP、HTTPS。
* 使用者資訊 (User Information)：用於有支援認證機制的 Origin Server，由於安全性，相對非常少見。
* 主機 (host)：包含網域名稱與 IP 位址，是虛擬主機帳號的主要網域名稱。
    * 子網域 (Sub-domain)：子網域就是依附在某網址底下的網域名稱，主要被用來做一些跟主網站區分的功能，如部落格、購物網站等。
    * 根網域 (Root domain)：通常是指由次級網域 (SLD) 與頂級網域 (TLD) 的組合，用以代表整個網站位址的代稱。
    * 頂級網域 (Top-Level Domains)：依照「型態」將域名分成以下三種：分別是「通用頂級網域」、「國家頂級域名」和「新頂級域名」。
        * 通用頂級網域 (Generic Top-Level Domain，gTLD)：一般常見的 .com / .net / .org / .edu 等結尾，都是屬於通用頂級網名，這類的域名由網際網路名稱與數字地址分配機構 (IANA) 管理。
        * 國家頂級域名 (Country-Code Top-Level Domain，ccTLD)：由 ISO-3166 的二位字母代碼所組成的國家或地區名稱縮寫，主要目的是讓搜尋引擎知道你的服務是提供給哪國人民 ( 許多 ccTLD 都有一些限制 )。常見的有 .tw / .jp / .hk / .uk。 
        * 新頂級域名 (New Generic Top-Level Domain，New gTLD)：能讓企業註冊與產業相符的域名，目的是希望能讓客戶一眼明白網址所代表的含義，例如：.book、.shoes、.net、.online 等。不同類別的網域只是網域類別的不同，在使用上並不會有差異。
* 連接埠 (Port)：預設為 80 port，其他可參考 [常見的連接埠](https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-zh_tw-4/ch-ports.html)。
* 路徑 (Path)：識別主機中 Web 客戶要訪問的特定資源。
* 查詢字串 (Query string)：在網址結尾加上一個問號「？」開始，每一組參數都是用「＆」區隔開來，是一種KEY / Value的組合。
    * 如果 query 字串中，包含 保留字元 (Reserved)，
例如 「@」、「:」、「/」、「?」、「%」、「=」、「 」(空白) … ，必須使用百分比編碼 (Percent-Encoding)「%」。
* 錨點 (Anchor) / 片段 (Fragment)：由 Client 端 (通常為瀏覽器) 處理的部分，並 不會傳遞給 Server，Fragment 最初的唯一目的是錨點 (anchor)，例如 #result 來讓瀏覽器顯示特定的部分。

## IP 與 DNS

### IP

IP位址（IP Address，Internet Protocol Address），又譯為網際協定位址、網際網路協定位址。當裝置連接網路，裝置將被分配一個IP位址，用作標識。

常見的IP位址分為 IPv4 與 IPv6 兩大類，IP位址由一串數字組成。IPv4 由十進位數字組成，並以點分隔，如：`172.16.254.1`，IPv6 由十六進位數字組成，以冒號分割，如：2001:db8:0:1234:0:567:8:1。
>根據 TCP/IP 協議規定，IP 地址是由 32 位的二進制數組成，不過人們為了記憶方便，所以將這32位再分成四段，每段各含 8bits，並轉換成十進位數字且用小數點隔開，這也是為什麼我們的IP每一段只會包含 0 到 255 間的數字。

### DNS

網域名稱系統（Domain Name System，DNS）是網際網路的一項服務。它作為將域名和IP位址相互對映的一個分散式資料庫，能夠使人更方便地存取網際網路。

當您開啟 Web 瀏覽器進入網站時，不需要記住這些冗長的數字進行輸入，而是輸入像 `www.google.com` 這樣的網域名稱就可以連接到正確的位置。

## 參考資料
* [統一資源識別符 (URI)](https://notfalse.net/36/http-uri)
* [主網域、子網域、網域寄放和附加網域是什麼？對SEO有何影響？](https://bettywutalk.com/blog/domains/)
* [IP位址 - 維基百科，自由的百科全書](https://zh.wikipedia.org/wiki/IP%E5%9C%B0%E5%9D%80)
* [域名系統 - 維基百科，自由的百科全書](https://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F)
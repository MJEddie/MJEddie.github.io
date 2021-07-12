---
title: "[筆記] Git & Github 入門"
date: 2020-11-21 18:06:32
tags: [Git,GitHub]
---
![](/uploads/note.jpg)
## Git 是什麼？

一種分散式版本控制系統 (Distributed Version Control Systems)，可藉由它產生一個數據庫 (repository)，並且做到分散式版本控制。由於可在多處放置同一份程式碼、歷史紀錄追蹤與回朔，讓協同開發變得容易。
<!-- more -->
## Git 和 GitHub 的差異

git 是一套軟體系統，而 GitHub 則是運用 git 提供程式原始碼版本控制與代管服務的服務平台。

## Git 安裝

[Git 軟體安裝](https://git-scm.com/)

## Git 版本控制基本觀念

![](https://i.imgur.com/n02KNfM.jpg)

## Git 常用指令

* 初始化數據庫： `git init`
* 查詢當前狀態：`git status`
* 將檔案加入到索引：`git add .`
* 將索引檔案變成一個更新(commit)：`git commit -m "修改內容"`
* 觀察 commit 歷史紀錄： `git log`
* 下載遠端數據庫： `git clone 數據庫網址`
* 更新遠端數據庫： `git push origin master`

## GitHub 專案建立

申請完 Github 帳號後點選右上角來新增 new repository

![](https://i.imgur.com/kzyM9zM.png)

這邊輸入專案的名稱

![](https://i.imgur.com/1fDgt0T.png)

* 專案建立之後如果是全新開始，請依「create a new repository on the command line」的指示進行；如果是要上傳現存專案，則依照「push an existing repository from the command line」指示進行。

![](https://i.imgur.com/a6TWHPN.png)

## GitHub page 實作

進入到專案資料夾，`git status` 確認資料夾內的狀態，可以看到 index.html 跟 css 資料夾裡的 main.css 檔案還未加入索引
>這邊我用舊有專案示範，有先修改內容，所以索引內還沒有修改後的 `index.html` 跟 `main.css`

![](https://i.imgur.com/B30oq0Q.png)

把修改後的 `index.html` 和 `main.css` 用 `git add` 指令加入索引中，再次確認 git 狀態，可以看到檔案顏色變成綠色，代表已經加入

![](https://i.imgur.com/rMD9OOf.png)

接著 `git commit -m "2020style"` 加入到本地數據庫，並註解 "2020style"

![](https://i.imgur.com/K5gOl1N.png)

再來用 `git branch gh-page` 來新增名為 gh-page 的分支，之後用 `git branch` 來確認是否新增成功，成功的話可以看到我們現在有兩個分支

![](https://i.imgur.com/KvP8vbw.png)

輸入` git checkout gh-page` 切換到 gh-page 分支

![](https://i.imgur.com/Z0ZYOXf.png)

輸入 `git push`將本地的檔案 push 到 GitHub 上，但是 GitHub 還沒有這個分支，所以會提示我們要輸入以下指令來完成 `git push --set-upstream origin gh-pages`

![](https://i.imgur.com/DiTayjF.png)

接下來在 GitHub 頁面上可以看到分支建立成功

![](https://i.imgur.com/RSPJ7Ub.png)

剛剛新增的檔案也在分支內

![](https://i.imgur.com/rdrwWo0.png)

之後就可以透過網址來瀏覽我們的靜態網頁
https://mjeddie.github.io/RootsSkate/


## 參考資料

[Git 官方教學文件](https://git-scm.com/book/zh-tw/v2)

[Git 與 Github 是什麼？如何使用 Git？](https://vocus.cc/@raychang/5de3dbb8fd89780001d599fc)


## 延伸閱讀

[5xruby - 為你自己學 Git](https://gitbook.tw/)

[ihower 的 Git 教室](https://ihower.tw/git/index.html)

[保哥 - 30 天精通 Git 版本控管](https://github.com/doggy8088/Learn-Git-in-30-days/blob/master/zh-tw/README.md)

[連猴子都能懂得 Git 入門教學](https://backlog.com/git-tutorial/tw/)
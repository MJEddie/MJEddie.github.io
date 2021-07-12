---
title: "[筆記] Command Line 常用指令"
date: 2020-11-20 20:02:10
tags: [Linux,Command Line]
---
![](/uploads/note.jpg)

## Command Line 是什麼？
Command-Line Interface（命令列介面，簡稱CLI）是在圖形使用者介面得到普及之前使用最為廣泛的使用者介面，它通常不支援滑鼠，用戶通過鍵盤輸入指令，電腦接收到指令後，予以執行。也有人稱之為文字使用者介面（character user interface, CUI）
<!-- more -->
## Command Line 基本指令

* pwd：print working directory，印出現在資料夾位置
* ls：list contents，列出資料夾內所有檔案
    * ls -l：列出詳細資訊
    * ls -a：可以看到隱藏的檔案
    * ls -al:列出詳細資訊，包含隱藏檔案
    * ls -R：可以看到所有的子目錄，較少用

    可以看到 ls -l 中列出的資訊中包含擁有者、權限以及上次修改時間
    
    ![](https://i.imgur.com/vIRh5k8.png)

* cd：change directory，切換資料夾
    * cd ..：上層資料夾
    * cd ../..：上上層資料夾

    指令練習
    
    ![](https://i.imgur.com/kyg3RgV.png)

* mkdir：make directory，建立資料夾

    指令練習，可以看到新增了一個 "practice" 資料夾

    ![](https://i.imgur.com/S1HIeGe.png)


* touch：新增檔案，如果輸入的檔案存在則會修改檔案時間

    指令練習，進入剛新增的資料夾，新增一個 "index.html" 的檔案
    
    ![](https://i.imgur.com/f2gwQwd.png)

* cp：copy，複製檔案

    指令練習，複製 "index.html" 為 "about.html"
    
    ![](https://i.imgur.com/I0Wuj3L.png)

* mv：move，移動檔案，或是重新命名檔案名稱

    指令練習，移動 "about.html" 到上一層 "Music" 資料夾中
    >cd 可使用相對路徑或是絕對路徑，要將檔案移動到上一層時必須使用絕對路徑
    
    ![](https://i.imgur.com/a7CVEoc.png)

    將剛移動過來的 "about.html" 重新命名為 "index.html"
    
    ![](https://i.imgur.com/G3rBzj0.png)


* rm：remove，刪除檔案
    * rmdir，刪除資料夾
    * rm -rf，強制刪除檔案，小心使用

    指令練習，將剛新增的 "practice" 資料夾刪除
    
    ![](https://i.imgur.com/L4mvrLt.png)


* cat：查看檔案內容

    可以看到 "about.html" 裡的內容為 "Hello"
    >原本 about.html 裡是沒有東西，先用 vim 指令編輯 about.html 
    >更多 vim 的操作可以參考 [簡明 Vim 文字編輯器操作入門教學](https://blog.techbridge.cc/2020/04/06/how-to-use-vim-as-an-editor-tutorial/)
    
    ![](https://i.imgur.com/PWqzA7x.png)


## 參考資料

[Linux Command](http://linuxcommand.org/index.php)
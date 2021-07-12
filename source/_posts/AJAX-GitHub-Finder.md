---
title: "[實作] AJAX - GitHub Finder"
date: 2021-02-01 18:50:16
tags: [JavaScript, AJAX]
---
## 成品

[GitHub Finder](https://mjeddie.github.io/JavaScript-Projects/Github_Finder/index.html)

![](https://i.imgur.com/hzdnvFR.png)

## 介紹

串接 [GitHub REST API](https://docs.github.com/en/rest) 搜尋 GitHub 使用者的練習，會顯示使用者的相關資訊 ( 專案數、追蹤人數等 )，以及最近的專案，搜尋不到使用者時有告警顯示提醒。
<!-- more -->
切版方面使用 Bootstrap，頁面在搜尋前只有 Nav bar、search bar 跟 footer 區塊， search bar 在 hover 時才會完整顯示出來，還未 hover 時只顯示 icon。

## 功能

* 顯示使用者資訊
* 顯示專案 (Repo) 資訊
* 告警顯示


### 顯示使用者資訊

**從 API 獲取使用者資訊**

練習用 `ajax` 來串接 API，確認我們的網址、方法與檔案格式，成功後整理我們要的資料，這邊把為 `null` 的值改成空白顯示，用 `substr` 擷取時間字串的前十個字元，讓其格式為 `YYYY-MM-DD` ，最後呼叫函式顯示在頁面上。

```javascript=
function getUserData() {
    $.ajax({
        url: `https://api.github.com/users/${userName}`,
        method: 'GET',
        dataType: 'json',
        success: function(res) {
            let obj = {};
            obj.avatar_url = res.avatar_url !== null ? res.avatar_url : '';
            obj.name = res.name !== null ? res.name : '';
	    obj.account = res.login;
            obj.html_url = res.html_url !== null ? res.html_url : '';
            obj.public_repos = res.public_repos !== null ? res.public_repos : '';
            obj.public_gists = res.public_gists !== null ? res.public_gists : '';
            obj.followers = res.followers !== null ? res.followers : '';
            obj.following = res.following !== null ? res.following : '';
            obj.company = res.company !== null ? res.company : '';
            obj.blog = res.blog !== null ? res.blog : '';
            obj.location = res.location !== null ? res.location : '';
            obj.created_at = res.created_at.substr(0, 10) !== null ? res.created_at.substr(0, 10) : '';
            obj.updated_at = res.updated_at.substr(0, 10) !== null ? res.updated_at.substr(0, 10) : '';
            showProfile(obj);
        }
    });
}
```

**顯示函式**

把從 API 拿到的資料放進整理好的各欄位中，最後顯示在頁面上。

```javascript=
function showProfile(user) {
    $('#profile').html(`
        <div class="card card-body border-0">
            <div class="row">
                <div class="col-md-3">
                    <img class="img-fluid rounded-circle mb-2" src="${user.avatar_url}">
                    <h5 class="mt-2 text-center">${user.name}</h5>
 		    <h6 class="mt-2 text-center text-secondary">${user.account}</h6>
                    <a href="${user.html_url}" target="_blank" class="btn btn-primary btn-block mb-4">View Profile</a>
                </div>
                <div class="col-md-9">
                    <div class="container mt-2 px-0">
                        <span class="badge badge-primary ml-5">Public Repos: ${user.public_repos}</span>
                        <span class="badge badge-secondary">Public Gists: ${user.public_gists}</span>
                        <span class="badge badge-success">Followers: ${user.followers}</span>
                        <span class="badge badge-info">Following: ${user.following}</span>
                    </div>
                    <ul class="list-group list-group-flush mt-2 ml-5">
                        <li class="list-group-item">Company: ${user.company}</li>
                        <li class="list-group-item">Website/Blog: ${user.blog}</li>
                        <li class="list-group-item">Location: ${user.location}</li>
                        <li class="list-group-item">Member Since: ${user.created_at}</li>
                        <li class="list-group-item">Currently Updated: ${user.updated_at}</li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="col-md-12">
            <h3 class="page-heading mb-3">Latest Repos</h3>
            <div id="repos"></div>
        </div>
    `);
}
```

## 顯示專案 (Repo) 資訊

對搜尋按鈕綁定滑鼠點擊的事件，把輸入欄位中的使用者名稱傳入 API 中搜尋。

```javascript=
searchBtn.click(function() {
    userName = searchUser.val();
    getUserData(userName);
    getRepoData(userName);
    searchUser.val('');
})
```

**從 API 獲取專案資訊**

和使用者資訊一樣的方式，確認我們的網址、方法與檔案格式， `per_page=5` 我們設定最多顯示 5 筆專案資料，之後呼叫函式顯示在頁面上。

```javascript=
function getRepoData() {
    $.ajax({
        url: `https://api.github.com/users/${userName}/repos?per_page=5&sort=created: asc`,
        method: 'GET',
        dataType: 'json',
        success: function(res) {
            data = res;
            showRepos(data);
        }
    });
}
```

**顯示函式**

每筆專案資料我們用一個 `<div>` 包起來，把專案相關資訊放在裡面呈現，如果專案敘述為 `null` 則顯示空白，最後把每一筆資料顯示在葉面上。

```javascript=
function showRepos(repos) {
    repos.forEach(function(repo) {
        const repoData = $('<div class="card card-body mb-2  border-0"></div>').appendTo('#repos');
        let repoDes = repo.description !== null ? repo.description : '';
        repoData.html(`
                <div class="row shadow-sm p-3 bg-white rounded">
                    <div class="col-md-6">
                        <a href="${repo.html_url}" target="_blank">
                            <h3>${repo.name}</h3>
                        </a>
                        <p>${repoDes}</p>
                    </div>
                    <div class="col-md-6">
                        <span class="badge badge-warning m-1">Stars ${repo.stargazers_count}</span>
                        <span class="badge badge-success m-1">Watchers ${repo.watchers_count}</span>
                        <span class="badge badge-info m-1">Forks ${repo.forks_count}</span>
                    </div>
                </div>
        `);
    });
}
```

## 告警顯示

當搜尋不到使用者名稱時，會跳出告警提示使用者，我們在獲取使用者資訊的 API 中增加失敗處理函式。

```javascript=
error: function(res) {
            data = res.responseJSON.message;
            if (data === 'Not Found') {
                showAlert(data, 'alert alert-danger');
            }
        }
```

把我們要顯示的訊息整理好準備在頁面上顯示，利用 Bootstrap 提供的 [Alerts](https://getbootstrap.com/docs/4.0/components/alerts/) 方法可以設定我們的告警樣式，我們增加名稱為 `alert alert-danger` 的 class 讓其顯示為紅色，`text-center` 則讓告警的訊息置中，`insertBefore()` 決定我們要在頁面的哪個 DOM 之中插入顯示我們的告警訊息，顯示告警時清空之前搜尋的使用者資訊。

最後我們設置一個定時器，讓告警訊息只顯示 2 秒後便移除。

```javascript=
function showAlert(message, className) {
    const alertMessage = $('<div></div>').attr('class', className + ' text-center');
    alertMessage.text(`User "${userName}" is ${message}`);
    alertMessage.insertBefore($('.search.card.card-body.border-0'));
    $('#profile').html('');
    
    // clear display after 2 sec
    setTimeout(() => {
        alertMessage.remove();
    }, 2000);
}
```
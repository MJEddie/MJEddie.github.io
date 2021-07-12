---
title: "[實作] JavaScript - Form Validator"
date: 2020-12-29 22:31:28
tags: JavaScript
---
## 成品

[From Validator](https://mjeddie.github.io/JavaScript-Projects/Form_Validator/index.html)

![](https://i.imgur.com/jsyUnrt.png)

## 介紹

JavaScript 可用來在資料被送往伺服器前對 HTML 表單中的這些輸入資料進行驗證，算是很常看到的功能，紀錄一下實作的練習。
<!-- more -->
## 功能

* 檢查必填項目，空白則顯示 `xxx is required`
* 檢查字元長度 ( 使用者名稱 3~15 個字元、密碼 8~16 個字元)
* 驗證 email 格式，不符合的話顯示 `Email is not valid`
* 確認密碼是否正確
* 輸入時能立刻判斷是否符合格式

### HTML 架構

`h2` 文字搭配表單格式，`<label>` 元素透過 `for` 屬性與 `<input>` 元素的 `id` 屬性連結，`<small>` 標籤則用來放置我們要呈現的錯誤訊息。

```htmlembedded=
<form id="form" class="form">
    <h2>Register With Us</h2>
    <div class="form-control">
        <label for="username">Username</label>
        <input type="text" id="username" placeholder="Enter username" />
        <small><!-- error message --></small> 
    </div>
    <div class=" form-control ">Email...</div>
    <div class=" form-control ">Password...</div>
    <div class=" form-control ">Confirm Password...</div>
    <button type="submit">Submit</button>
</form>
```
### 檢查必填項目

宣告變數，利用 `getElementById()` 來取得 HTML 內的元素

```javascript=
const form = document.getElementById('form');
const username = document.getElementById('username');
const email = document.getElementById('email');
const password = document.getElementById('password');
const password2 = document.getElementById('password2');
```

我們希望註冊失敗時則顯示紅色，並且把錯誤訊息顯示出來，成功時則顯示綠色，所以分別建立代表失敗跟成功的顯示函式，用 JavaScript 去切換 `class` 的名稱，以達到我們想要的功能。

```javascript=
// Show input error message
function showError(input, message) {
    const formControl = input.parentElement;
    formControl.classList.add('error');
    const small = formControl.querySelector('small');
    small.innerText = message;
}

// Show success outline
function showSuccess(input) {
    const formControl = input.parentElement;
    formControl.classList.add('success');
}
```

檢查必填項目，針對傳入的陣列中每一個元素去檢查，若沒有填寫會顯示 `xxx is required`。
> `.trim()` 函數用於去除字符串兩端的空白字符
```javascript=
// Check required fields
function checkRequired(inputArr) {
    inputArr.forEach(function(input) {
        if (input.value.trim() === '') {
            showError(input, `${getFieldName(input)} is required`);
        } else {
            showSuccess(input);
        }
    });
}
```

其中我們想要讓顯示的第一個英文字母為大寫，所以用另一個函式`getFieldName` 來處理

1. 取得第一個字母：`charAt(0)`
2. 轉換成大寫：`toUpperCase()`
3. 用 `slice(1)` 得到一個去掉字首的字串並和之前轉換的大寫字母相加

```javascript=
// Get fieldname
function getFieldName(input) {
    return input.id.charAt(0).toUpperCase() + input.id.slice(1);
}
```


### 檢查字元長度

用 `.length` 取得輸入的值去判斷長度，若是不符合我們的設定 (使用者名稱 3~15 個字元、密碼 8~16 個字元)呼叫 `showError` 並顯示錯誤訊息，符合則呼叫 `showSucces`。

```javascript=
// Check input length
function checkLength(input, min, max) {
    if (input.value.length < min && input.value.trim() !== '') {
        showError(input, `${getFieldName(input)} must be at least ${min} characters`);
    } else if (input.value.length > max) {
        showError(input, `${getFieldName(input)} must be less than ${max} characters`);
    } else {
        showSuccess(input);
    }
}
```


### 驗證 email 格式

把輸入值用 `test()` 是否與 Email 格式驗證的正規表達式匹配，相符呼叫 `showSucces` 不符合則呼叫 `showError` 並顯示錯誤訊息 `Email is not valid`。

```javascript=
/// Check email is valid
function checkEmail(input) {
    const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    if (re.test(input.value.trim())) {
        showSuccess(input);
    } else if (input.value.trim() === '') {
        showError(input, `${getFieldName(input)} is required`);
    } else {
        showError(input, 'Email is not valid');
    }
}
```


### 確認密碼是否正確

很簡單的對兩個值進行判斷，不相等則呼叫 `showError` 並顯示錯誤訊息 `Passwords do not match`。

```javascript=
// Check passwords match
function checkPasswordMatch(input1, input2) {
    if (input1.value !== input2.value) {
        showError(input2, 'Passwords do not match');
    }
}
```


### 輸入時能立刻判斷是否符合格式

對我們的 `input` 做監聽，接著呼叫我們的判斷函式判斷是否符合格式，使用正則表達式來建立我們的規則，使用 `test` 對我們輸入的字串判斷是否符合正則表達式的規則，然後切換 `class` 名稱顯示符合或不符合，不符合則會額外顯示錯誤訊息。

```javascript=
form.addEventListener('input', validate);

function validate(e) {
    const target = e.target;
    switch (target.id) {
        case 'username':
            const userRegex = /^[a-zA-Z0-9]{3,15}$/;
            if (userRegex.test(target.value.trim())) {
                target.parentElement.classList.add('success');
                target.parentElement.classList.remove('error');
            } else {
                target.parentElement.classList.add('error');
                target.parentElement.classList.remove('success');
                showError(target, 'must be at least 3 characters');
            }
            break;
        case 'email':
            if (target.id == "email") {
                const emailRegex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                if (emailRegex.test(target.value.trim())) {
                    target.parentElement.classList.add('success');
                    target.parentElement.classList.remove('error');
                } else {
                    target.parentElement.classList.add('error');
                    target.parentElement.classList.remove('success');
                    showError(target, 'Email is not valid');
                }
            }
            break;
        case 'password':
            const pwdRegex = /(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$/;
            if (pwdRegex.test(target.value.trim())) {
                target.parentElement.classList.add('success');
                target.parentElement.classList.remove('error');
            } else {
                target.parentElement.classList.add('error');
                target.parentElement.classList.remove('success');
                showError(target, 'must be at least 8 characters');
            }
            break;
        case 'password2':
            const pwd2Regex = /(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$/;
            const pwd = document.querySelector('#password');
            const pwd2 = document.querySelector('#password2');
            if (pwd.value === pwd2.value) {
                target.parentElement.classList.add('success');
                target.parentElement.classList.remove('error');
            } else {
                target.parentElement.classList.add('error');
                target.parentElement.classList.remove('success');
                showError(target, 'Passwords do not match');
            }
            break;

    }
}
```
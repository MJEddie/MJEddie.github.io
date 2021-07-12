---
title: "[實作] Bootstrap - Form Validator"
date: 2021-01-03 22:09:15
tags: Bootstrap
---
## 成品

[From Validator](https://mjeddie.github.io/JavaScript-Projects/Form_Validator/form.html)

![](https://i.imgur.com/R7lU2XM.png)

## 介紹

對之前實作的 From Validator 用 Bootstrap 的 form 改寫。
<!-- more -->
## 功能

* 檢查必填項目，空白則顯示 `xxx is required`
* 檢查字元長度 ( 使用者名稱 3~15 個字元、密碼 8~16 個字元)
* 驗證 email 格式，不符合的話顯示 `Email is not valid`
* 確認密碼是否正確
* 輸入時能立刻判斷是否符合格式

## HTML 架構

每一個表單控制項要增加 class `form-control` 並與其搭配的 `label` 元素用帶有 class `form-group` 的 `div` 包起來形成一個表單群組。class `invalid-feedback` 
可以設定我們的錯誤訊息。

```html=
<form class="needs-validation" novalidate oninput='password2.setCustomValidity(password2.value != password.value ? "Passwords do not match." : "")'>
    <h2>Register With Us</h2>
    <div class="form-group">
        <label for="username">Username</label>
        <input type="text" class="form-control" id="username" placeholder="Enter username" pattern="^[a-zA-Z0-9]{3,15}$" required>
        <small class="invalid-feedback">Username is required</small>
    </div>
    <div class="form-group">
        <label for="email">Email address</label>
        <input type="email" class="form-control" id="email" placeholder="Enter email" pattern='^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$' required>
        <small class="invalid-feedback">Email is not valid</small>
    </div>
    <div class="form-group">
        <label for="password">Password</label>
        <input type="password" name="password" class="form-control" id="password" placeholder="Enter password" pattern="(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$" required>
        <small class="invalid-feedback">Password is required</small>
    </div>
    <div class="form-group">
        <label for="password2">Confirm Password</label>
        <input type="password" name="password2" class="form-control" id="password2" placeholder="Enter password again" data-match="#password" required>
        <small class="invalid-feedback">Passwords do not match</small>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

## 驗證方式

Bootstrap 的表單驗證是透過偽元素 `:invalid` `:valid` 進行的，可以適用在 `<input>`、`<select>`和`<textarea>`。

要使用 Bootstrap 的驗證方式只需要在我們的父元素 `<form>` 加上 `was-validate` clss，而對於自定義的表單驗證訊息，需要將 `novalidate` 屬性添加到 `<form>` 中。接著使用所提供的程式範本，可以完成按下 `submit` 時的表單驗證。

```javascript=
(function () {
  'use strict'

  // Fetch all the forms we want to apply custom Bootstrap validation styles to
  var forms = document.querySelectorAll('.needs-validation')

  // Loop over them and prevent submission
  Array.prototype.slice.call(forms)
    .forEach(function (form) {
      form.addEventListener('submit', function (event) {
        if (!form.checkValidity()) {
          event.preventDefault()
          event.stopPropagation()
        }

        form.classList.add('was-validated')
      }, false)
    })
})()
```

**即時驗證**

剛有提到 Bootstrap 的表單驗證是透過偽元素 `:invalid` `:valid` 進行的，而作為備用 `is-invalid` `is-valid` class 可以用來替代偽元素在伺服器端的驗證，我們只需透過對 `input` 的監聽判斷是否符合我們所設置的規則，添加`is-invalid`或是`is-valid` class 即可達到立即判斷的功能。

```javascript=
form.addEventListener('input', validate);

function validate(e) {
    const target = e.target;
    switch (target.id) {
        case 'username':
            const userRegex = /^[a-zA-Z0-9]{3,15}$/;
            if (userRegex.test(target.value.trim())) {
                target.classList.add('is-valid');
                target.classList.remove('is-invalid');
            } else {
                target.classList.add('is-invalid');
                target.classList.remove('is-valid');
            }
            break;
        case 'email':
            if (target.id == "email") {
                const emailRegex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                if (emailRegex.test(target.value.trim())) {
                    target.classList.add('is-valid');
                    target.classList.remove('is-invalid');
                } else {
                    target.classList.add('is-invalid');
                    target.classList.remove('is-valid');
                }
            }
            break;
        case 'password':
            const pwdRegex = /(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$/;
            if (pwdRegex.test(target.value.trim())) {
                target.classList.add('is-valid');
                target.classList.remove('is-invalid');
            } else {
                target.classList.add('is-invalid');
                target.classList.remove('is-valid');
            }
            break;
        case 'password2':
            const pwd2Regex = /(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,16}$/;
            const pwd = document.querySelector('#password');
            const pwd2 = document.querySelector('#password2');
            if (pwd.value === pwd2.value) {
                target.classList.add('is-valid');
                target.classList.remove('is-invalid');
            } else {
                target.classList.add('is-invalid');
                target.classList.remove('is-valid');
            }
            break;

    }
}
```
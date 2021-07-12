---
title: "[實作] shell環境安裝"
date: 2020-11-19 17:30:43
tags: [Linux,WSL]
---
![](/uploads/implement.jpg)
## WSL 是什麼？
Ｗindows Subsystem for Linux，簡稱 WSL，適用於 Linux 的 Windows 子系統，可在 Windows 上執行 Linux 環境。

## 安裝 WSL
<!-- more -->
### Step 1 - 啟用 WSL

以系統管理員身分開啟 PowerShell

![](https://i.imgur.com/S4Mzic8.png)

執行下面這段

```PowerShell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
確認執行完出現下列畫面

![](https://i.imgur.com/3AWluo4.png)


### Step 2 - 更新至 WSL2

先確認您的版本及組件號碼是否符合以下條件 
>Windows 標誌鍵 + R，輸入 winver 確認版本

* X64 系統： **版本 1903** 或更高版本，含**組建 18362** 或更高組建。
* ARM64 系統： **版本 2004** 或更高版本，含**組建 19041** 或更高組建。
* 低於 18362 的組建不支援 WSL 2。 使用 Windows 更新小幫手來更新您的 Windows 版本。

![](https://i.imgur.com/fAEsomG.png)


### Step 3 - 啟用虛擬機功能

一樣以系統管理員身分開啟 PowerShell 並執行：

```PowerShell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
確認執行完出現下列畫面

![](https://i.imgur.com/DNvUMNa.png)

重新啟動電腦已完成 WSL並更新至 WSL2


### Step 4 - 下載 Linux 核心更新套件

[WSL2 Linx 更新套件](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)


### Step 5 - 將 WSL2 設為預設版本

一樣以系統管理員身分開啟 PowerShell 並執行：

```PowerShell
wsl --set-default-version 2
```
這部分出現以下錯誤，不過不影響之後的安裝
```PowerShell
wsl : 無法辨識 'wsl' 詞彙是否為 Cmdlet、函數、指令檔或可執行程式的名稱。請檢查名稱拼字是否正確，如果包含路徑的話，請確
認路徑是否正確，然後再試一次。
```

### 安裝 Linux 發行版本

從 Microsoft Store 上選取一個 Linux 發行版本，第一次啟動時需要設定**使用者帳戶和密碼**

![](https://i.imgur.com/DTPh2J6.png)

設定完成畫面，之後想要重設密碼或忘記的話可以參考 [這邊](https://docs.microsoft.com/zh-tw/windows/wsl/user-support#forgot-your-password)


## 參考資料

[在 windows10 安裝 bash& oh-my-zsh](https://medium.com/@wens.li/%E5%9C%A8-windows10-%E5%AE%89%E8%A3%9D-oh-my-zsh-916105cf36f7)

[為新的 Linux 發行版本建立使用者帳戶和密碼](https://docs.microsoft.com/zh-tw/windows/wsl/user-support)
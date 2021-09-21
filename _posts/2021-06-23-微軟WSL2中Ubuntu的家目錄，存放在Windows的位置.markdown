---
layout: post
title:  "微軟WSL2中Ubuntu的家目錄，存放在Windows的位置"
subtitle: "實際測試微軟 WSL2 和 Ubuntu 和 Docker Desktop 的整合變的很方便了"
date:   2021-06-23 09:00:00 +0800
categories: Nginx
tags:
- "Nginx"
---

實際測試微軟 WSL2 和 Ubuntu 和 Docker Desktop 的整合變的很方便了

![](/images/medium/1__M7oFwrkZIanu1RBVPgspLw.png)

開始檔案總管，在路徑列輸入

`\\wsl$`

![](/images/medium/1__akAc0hXlONCsafxRpwzmew.png)

一層一層進去後，可以看到 Ubuntu 中 Home 目錄在 Windows 中的路徑

`\\wsl$\Ubuntu-20.04\home\ben\projects`

![](/images/medium/1__XPRe6FSZaeHrKBtKimUjAA.png)

參考

> [https://docs.microsoft.com/en-us/answers/questions/116126/access-wsl2ubuntu-drive-from-file-explorer.html](https://docs.microsoft.com/en-us/answers/questions/116126/access-wsl2ubuntu-drive-from-file-explorer.html)

---
layout: post
title:  "關於透過/etc/hosts直接解析IP主機的動作，使用原本主機的SSL憑證"
subtitle: "有一台主機A有設定SSL，後面增加內網5台主機 B, C, D, E, F (API主機) 沒有SSL"
date:   2021-03-25 09:00:00 +0800
categories: Unclassified
meta_description: "關於透過/etc/hosts直接解析IP主機的動作，使用原本主機的SSL憑證"
tags:
- ""
---

有一台主機 A 有設定 SSL，後面增加內網 5 台主機 B, C, D, E, F ( API 主機 ) 沒有 SSL

當 A 要使用 B, C, D, E, F 主機的 API 時，會出現瀏覽器混合模式的警告阻擋

簡單講：

**_主機 A 使用內網主機 B, C, D, E, F 的 API 時，即使是內網，主機 B, C, D, E, F 也需要具有 SSL_**

![](/images/medium/1__cmeW__n3zNurKLR9DvyTVFQ.jpeg)

原因是有一個檔案沒成功讀取時，或是非同源時，將會進入混合模式，所以不會顯示完整的 https

這個在之前專案時已經遇過相同的狀況

![](/images/medium/1__VXIyw48Pfq4URBQ8iX2fiA.png)

但是又不想要為內網 B, C, D, E, F 分別設定 SSL 解決這個問題

最後聽同事說明，可以直接在代理主機輸入要解析的 IP 和域名

但前提是該單位的 SSL 域名需要 \* 萬用，例如下圖

![](/images/medium/1__O__10KDe__cTKdT91__ljI1Lw.png)

目的是使用原本主機的 SSL 憑證，這一段才是重點

下圖是測試機的跳板機器 Win Server 設定

![](/images/medium/1__QJaMjf2FZnVT2ZipqGJz6A.jpeg)

確定👌可以透過網址 https 執行了，但原本這台 xxx.xxx.xxx.156 是沒有 https 的

![](/images/medium/1__iEa8u4pVrRqMZ3UJ5ABLyA.jpeg)

後續的動作就是在 B, C, D, E, F ( API 主機 ) 每一台主機的 /etc/hosts ( Linux ) 設定相同的設定檔，讓所有機器的解析的一致

當然，將來只要增加機器，之前機器的 hosts 都要再改一次 ( 含 Win Server 和 Linux 都要 )

概念如下圖

![](/images/medium/1__OQnahOLfUV2bxZ0Yn4sqSw.jpeg)

這就像我在公司的 Windows 主機上的 hosts 設定我自己 Mac 裡的專案一樣

這樣 Windows 主機也是可以透過網址連線到我的 Mac 主機中 ( 但專案埠號也是要加 )

這一段不是太大問題，也容易理解

重點是如何使用原本主機的 SSL 憑證，這一段才是重點

另外一個作法是，修改主要擁有 https 的 apache 設定檔

直接做每一台內網主機的代理

> 可以參考以下文章
>
> [https://richarlin.tw/blog/apache\_reverse\_proxy/](https://richarlin.tw/blog/apache_reverse_proxy/)

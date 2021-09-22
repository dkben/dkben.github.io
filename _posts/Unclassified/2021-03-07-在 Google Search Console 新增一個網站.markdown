---
layout: post
title:  "在 Google Search Console 新增一個網站"
subtitle: "Google Search Console 的官方說明"
date:   2021-03-07 09:00:00 +0800
categories: Unclassified
tags:
- ""
---

以下是 Google Search Console 的官方說明

> Google Search Console 簡介
>
> Google Search Console 是 Google 提供的一項免費服務，能夠協助您監控及維持網站在 Google 搜尋結果中的排名，並排解相關問題。即使未申請 Search Console，您的網站仍可能會顯示在 Google 的搜尋結果中，但 Search Console 有助您瞭解並改善 Google 查看您網站的方式。
>
> Search Console 提供各種工具和報告，可用於下列操作：
>
> 確認 Google 能夠找到並[**檢索**](https://support.google.com/webmasters/answer/7643418)您的網站。
>
> 修正索引問題，並要求為新增或更新的內容重新建立索引。
>
> 查看您網站的 Google 搜尋流量資料：網站出現在 Google 搜尋結果中的頻率、哪些搜尋查詢會顯示您的網站、搜尋服務使用者點閱這些查詢結果的頻率等等。
>
> 當 Google 發現您的網站有[**索引**](https://support.google.com/webmasters/answer/7643011)問題、垃圾內容或其他問題時，您會收到快訊。
>
> 顯示哪些網站包含指向您網站的連結。
>
> 排解與 AMP、行動裝置可用性和其他搜尋功能有關的問題。

首先增加一個要監控的網域

![](/images/medium/1____KtotncO07GarAz__YaQY2A.png)

這裡選擇透過 DNS 方式驗證

![](/images/medium/1__ztyhCe89WcIS__N6h__lYW1w.png)

在網域購買單位中增加 TXT 記錄

![](/images/medium/1__ZLigU9f7vtMypU24k2TjQQ.png)

輸入 @ 或網域名稱 ( 不帶子網域名稱 )

![](/images/medium/1__OChh92zfREOtvLKfs45jbQ.png)

回到 Google Search Console 中，按下驗證

![](/images/medium/1__FaI5JzTs5IhvvMJzVCQIqg.png)

起碼要等待一天的時間，才會有資訊可以分析並產生報表

![](/images/medium/1__BPpICmVxu0P6fhnu__bcBTQ.png)
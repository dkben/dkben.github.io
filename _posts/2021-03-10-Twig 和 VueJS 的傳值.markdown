---
layout: post
title:  "Twig 和 VueJS 的傳值"
subtitle: "如果不想要在進入 Vue 頁面後執行 API 取值的話，也可以把第一次的值傳到 Twig 樣版中，然後用 VueJS 讀取在 Twig 樣版中的值"
date:   2021-03-10 09:00:00 +0800
categories: VueJs
tags:
- "VueJs"
---

如果不想要在進入 Vue 頁面後執行 API 取值的話，也可以把第一次的值傳到 Twig 樣版中，然後用 VueJS 讀取在 Twig 樣版中的值。

下圖區分 new.html.twig 新增頁面和 edit.html.twig 修改頁面，使用的是相同的 Form，所以可以引用相同的 \_form.html.twig 頁面

![](/images/medium/1__KjEYLnfuCEJC__GxWKaOPHw.png)

在 Twig 樣版 \_form.html.twig 中使用 HTML5 data attribute 方式賦值

![](/images/medium/1__GGw4unTw__SWfjvXObD4PbA.png)

然後在 Vue JS 中取得 Twig 所設定的值

![](/images/medium/1__mKGzoYzpMoOiec0RpYULJQ.png)

然後就可以在 Vue JS 樣版中使用了

![](/images/medium/1__9__Xvfev__LAyhYOa5K3OG__w.png)

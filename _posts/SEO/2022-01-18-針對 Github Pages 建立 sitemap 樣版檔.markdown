---
sitemap:
    priority: 0.8
layout: post
title:  "針對 Github Pages 建立 sitemap 樣版檔"
subtitle: "針對 Github Pages 建立 sitemap 樣版檔"
date:   2022-01-18 08:00:00 +0800
categories: SEO
meta_description: "針對 Github Pages 建立 sitemap 樣版檔"
tags:
- "SEO"
---

因為 Github Pages 不支援使用 Plugin

參考以下[連結](https://gist.github.com/poychang/fb7be1320565c6cee6cf8255a1ce321a#file-sitemap-xml)手動建立 sitemap.xml 樣版檔 

然後可以在 post 及 page 頁面資料中，加入 sitemap 參數

> sitemap 的設定，可以參考 [sitemap 官方組織](https://www.sitemaps.org/zh_TW/protocol.html)

如果沒有加，就會以 sitemap.xml 預設值為主

![Untitled](/images/2022-01-18/2022-01-18-01.png)

啟動本地模擬

![Untitled](/images/2022-01-18/2022-01-18-02.png)

會在 _site 目錄下自動生成 sitemap.xml

![Untitled](/images/2022-01-18/2022-01-18-03.png)

然後 commit 後 push 到 Github 就可以

![Untitled](/images/2022-01-18/2022-01-18-04.png)

然後就可以提交 Google Search Console

![Untitled](/images/2022-01-18/2022-01-18-05.png)

---

如果使用 Plugin 的話，但注意 Github Pages 不支援

[Jekyll plugin to silently generate a sitemaps.org compliant sitemap for your Jekyll site](https://github.com/jekyll/jekyll-sitemap)

> 參考
>
> [不使用套件直接產生 Jekyll 的 sitemap.xml](https://blog.poychang.net/generating-sitemap-in-jekyll-without-plugin/)
> 
> [Building a Better Sitemap.xml with Jekyll](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)
> 
> [havvg.github.com/sitemap.xml](https://github.com/havvg/havvg.github.com/blob/master/sitemap.xml)
>

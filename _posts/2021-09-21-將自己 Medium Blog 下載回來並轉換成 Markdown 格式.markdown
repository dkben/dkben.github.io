---
layout: post
title:  "將自己Medium Blog下載回來並轉換成Markdown格式"
subtitle: "改用GitHub Pages"
date:   2021-09-21 09:00:00 +0800
categories: Nginx
tags:
- "Nginx"
---

用 npm 安裝 medium-2-md 工具

> https://www.npmjs.com/package/medium-2-md

`npm i -g medium-2-md`

![](/images/2021-09-21/2021-09-21-01.png)

到 medium 下載自己的 blog 回來，文章會在 posts 資料夾裡面

![](/images/2021-09-21/2021-09-21-02.png){:width="800px"}

執行以下轉換指令

`medium-2-md convertLocal 'posts' -dfi`

![](/images/2021-09-21/2021-09-21-03.png){:width="800px"}

轉換後會在原本 posts 資料夾裡面，新增一個 md_時間戳 的資料夾

原本文章標題大多是中文，轉換後會用 - 符號代替，圖片的話會整合在 img 資料夾

![](/images/2021-09-21/2021-09-21-04.png){:width="800px"}

有順利轉成 md 格式了

![](/images/2021-09-21/2021-09-21-05.png){:width="800px"}

> 參考
> 
> medium-2-md: Convert Medium posts to markdown with front matter
> 
> [https://www.gautamdhameja.com/medium-to-markdown-converter/](https://www.gautamdhameja.com/medium-to-markdown-converter/)

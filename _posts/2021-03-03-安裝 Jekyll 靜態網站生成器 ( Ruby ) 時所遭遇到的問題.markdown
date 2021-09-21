---
layout: post
title:  "安裝 Jekyll 靜態網站生成器 ( Ruby ) 時所遭遇到的問題"
subtitle: "按照官網安裝 Jekyll 靜態網站生成器 ( Ruby )"
date:   2021-03-03 09:00:00 +0800
categories: Jekyll
tags:
- "Jekyll"
---

按照官網安裝 Jekyll 靜態網站生成器 ( Ruby )

[https://jekyllrb.com/](https://jekyllrb.com/)

執行

`bundle exec jekyll serve —-trace`

出現錯誤訊息 cannot load such file — webrick (LoadError)

![](/images/medium/1__Zz6FCn2PDxNxl3x6iTOUkg.png)

自己 Mac 的環境如下

![](/images/medium/1__dtrxM__YZ__5NubCFH7PBZxQ.png)

最後有找到解決方式

> jekyll/commands/serve/servlet.rb:3:in \`require’: cannot load such file — webrick
>
> [https://github.com/jekyll/jekyll/issues/8523](https://github.com/jekyll/jekyll/issues/8523)

裡面提到

> Adding gem “webrick” to my Gemfile solves the problem, but Jekyll should include it in its gemspec.

在專案下的 Gemfile 檔案最後面加入 `gem "webrick"`

然後再重新執行一次

`bundle exec jekyll serve --trace`

成功

![](/images/medium/1__kUfLX15ruunAMuS0qMV__mg.png)

出現網站了

![](/images/medium/1__5VJP2pYXZq0fJg2hnNgshQ.png)
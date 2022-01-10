---
layout: post
title:  "手動關閉 PHP Built-in web server 內置伺服器 ( 方法也可以用來關閉其它程式 )"
subtitle: "PHP 基本"
date:   2020-08-02 09:00:00 +0800
categories: PHP
meta_description: "手動關閉 PHP Built-in web server 內置伺服器 ( 方法也可以用來關閉其它程式 )"
tags:
- "PHP"
- "Linux"
---

當使用 PHP CLI 啟動 Built-in web server 內置伺服器時

```shell
php -S 127.0.0.1:8401
```

![Untitled](/images/2020-08-02/2020-08-02-01.png){:width="800px"}

如果不小心離開終端，無法使用 Ctrl-C 關閉

使用 Linux 的 lsof 指令，查詢系統上各行程所開啟的檔案

[https://blog.gtwang.org/linux/linux-lsof-command-list-open-files-tutorial-examples/](https://blog.gtwang.org/linux/linux-lsof-command-list-open-files-tutorial-examples/)

```shell
sudo lsof -i :8401
```

使用靜態的 ps 查詢程序

-u ：有效使用者 (effective user) 相關的 process

```shell
ps u 2935
```

![Untitled](/images/2020-08-02/2020-08-02-02.png){:width="800px"}

刪除程序

```shell
sudo kill -KILL 2935
```

![Untitled](/images/2020-08-02/2020-08-02-03.png){:width="800px"}

![Untitled](/images/2020-08-02/2020-08-02-04.png){:width="800px"}

成功關閉 Built-in web server 內置伺服器

![Untitled](/images/2020-08-02/2020-08-02-05.png){:width="800px"}

> 參考
> 
> [https://stackoverflow.com/questions/34563972/osx-failed-to-listen-on-localhost80-reason-permission-denied](https://stackoverflow.com/questions/34563972/osx-failed-to-listen-on-localhost80-reason-permission-denied)
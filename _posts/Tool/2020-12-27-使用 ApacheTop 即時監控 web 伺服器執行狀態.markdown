---
layout: post
title:  "使用 ApacheTop 即時監控 web 伺服器執行狀態"
subtitle: "監控工具"
date:   2020-12-27 09:00:00 +0800
categories: Tool
tags:
- "Monitor"
---

能夠即時監控 apache 伺服器的執行狀況


在 Ubuntu 18.04 中安裝

```shell
sudo apt-get install apachetop
```

因為有指定網站的 Log 位置，所以顯示 website 設定檔，確認 Log 位置

```shell
cat /etc/apache2/sites-enabled/000-default.conf
```

![Untitled](/images/2020-12-27/2020-12-27-01.png){:width="800px"}

啟動 ApacheTop 並指定使用的 Log

```shell
apachetop -f /var/log/apache2/sinica_tigp.vm_access_log
```

![Untitled](/images/2020-12-27/2020-12-27-02.png){:width="800px"}

按下 control + c 離開

> 參考
> 
> CentOS 安裝
> 
> https://www.liquidweb.com/kb/how-to-install-and-use-apachetop/
>
> 使用 Apachetop 實時監測 web 伺服器運行狀態
> 
> https://kknews.cc/zh-tw/code/pa34k2.html
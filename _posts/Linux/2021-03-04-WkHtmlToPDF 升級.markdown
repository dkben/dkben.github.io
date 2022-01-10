---
layout: post
title:  "WkHtmlToPDF 升級"
subtitle: "客戶正式機為 Windows Server WkHtmlToPdf 版本為 0.12.5，開發機為 Ubuntu 18.04 虛擬機 WkHtmlToPdf
版本為 0.12.4，造成執行時出現異常"
date:   2021-03-04 09:00:00 +0800
categories: Linux
meta_description: "需要將 WkHtmlToPDF 升級。客戶正式機為 Windows Server WkHtmlToPdf 版本為 0.12.5，開發機為 Ubuntu 18.04 虛擬機 WkHtmlToPdf 版本為 0.12.4，造成執行時出現異常"
tags:
- "Linux"
---

客戶正式機為 Windows Server WkHtmlToPdf 版本為 0.12.5，開發機為 Ubuntu 18.04 虛擬機 WkHtmlToPdf 版本為 0.12.4，造成執行時出現異常。

嘗試把指令倒出後進 Linux 中執行

`xvfb-run — ‘/usr/local/bin/wkhtmltopdf’ /tmp/tmp_WkHtmlToPdf_xxw0FW.html /tmp/tmp_WkHtmlToPdf_vKsOkV.html /tmp/tmp_WkHtmlToPdf_dTPDZT.html /tmp/tmp_WkHtmlToPdf_hviuES.html /tmp/tmp_WkHtmlToPdf_jAOljR`

得到錯誤訊息

> QStandardPaths: XDG\_RUNTIME\_DIR not set, defaulting to ‘/tmp/runtime-root’
>
> Error: This version of wkhtmltopdf is build against an unpatched version of QT, and does not support more then one input document.
>
> Exit with code 1, due to unknown error.

![](/images/medium/1__3pxDhAQpElEULwULB1LQrw.png)

有找到安裝 0.12.5 的文章

參考這篇，但失敗，可能資訊已經過時

> [https://gist.github.com/srmds/2507aa3bcdb464085413c650fe42e31d#wkhtmltopdf-0125----ubuntu-1604-x64](https://gist.github.com/srmds/2507aa3bcdb464085413c650fe42e31d#wkhtmltopdf-0125----ubuntu-1604-x64)

參考這篇，但也失敗，而且還移除了 0.12.4 版本

> [https://gist.github.com/ahmadhasankhan/7fd1fcd743fdac8472f04c72289f24cf](https://gist.github.com/ahmadhasankhan/7fd1fcd743fdac8472f04c72289f24cf)

手動安裝 libjpeg62-turbo

`sudo apt-get install ./libjpeg62-turbo_1.5.1–2_amd64.deb`

失敗中斷

![](/images/medium/1__Ya27CFxArJpvZzXjL465ag.png)

安裝遺失的套件

`sudo apt — fix-broken install`

會因為剛剛解除安裝 wkhtmltox 而失敗中斷

![](/images/medium/1__pPxjS70x25w1BdjPtA3DmA.png)

重新裝回 wkhtmltopdf

`sudo apt-get install -y wkhtmltopdf`

成功

![](/images/medium/1__Q1E__yqaeYLLy6CPY1nFtWA.png)

再重新安裝一次 libjpeg62-turbo

`sudo apt-get install ./libjpeg62-turbo_1.5.1–2_amd64.deb`

成功

![](/images/medium/1__q64bkyGJ36g0nANEft8m__Q.png)

參考

> [https://gist.github.com/srmds/2507aa3bcdb464085413c650fe42e31d#wkhtmltopdf-0125----ubuntu-1604-x64](https://gist.github.com/srmds/2507aa3bcdb464085413c650fe42e31d#wkhtmltopdf-0125----ubuntu-1604-x64)

下載的 wkhtmltox_0.12.5 版

wget [https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox\_0.12.5-1.stretch\_amd64.deb](https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb)

安裝剛剛下載的 wkhtmltox_0.12.5 版

`sudo dpkg -i wkhtmltox_0.12.5–1.stretch_amd64.deb`

![](/images/medium/1__3JBhYEDER8scUIW9ufg2zA.png)

檢查版本

`wkhtmltopdf — version`

確定是 0.12.5

![](/images/medium/1__WAcBoxZz5ofYBT2O4y78TA.png)
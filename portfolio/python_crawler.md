---
layout: default
title: 多個Python爬蟲專案
subtitle: "多個Python爬蟲專案"
meta_description: "多個Python爬蟲專案"
---

## 寫了許多 Python 爬蟲

2022 年寫了許多 Python 爬蟲專案，累積了一些經驗

像是多線程、Selenium、SQLite等使用方法

### 大概歸納出的重點如下

先理解要爬的目標是什麼，查看目標網站請求回應資料的方式，和資料的格式

然後先透過 Jupyter Notebook 做測試，確認流程

![](/images/2023-01-20/012.jpeg)

確認後轉成 Python 專案

![](/images/2023-01-20/013.jpeg)

最後佈署到線上主機並排程執行、監控

![](/images/2023-01-20/014.jpeg)

另外就是雖然可以使用多線程執行，但也要看對方伺服器服務是否可以允許我們這麼做

要當有善的爬蟲，不是讓爬蟲變成像是惡意攻擊一樣

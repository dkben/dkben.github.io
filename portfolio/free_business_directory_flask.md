---
layout: default
title: 台灣工商名錄 Flask
subtitle: "提供您每月即時更新的工商資訊"
meta_description: "台灣工商名錄 Flask，使用 Python Flask 框架開發，並透過 Jenkins Pipeline 做建置佈署"
---

<br>

### 這個 Site Project 使用 Python Flask 框架開發  
### 並透過 Jenkins Pipeline 做建置佈署

[https://find-business.hsuweni.info/](https://find-business.hsuweni.info/)

專案目的：  
使用 Python Flask 框架並整合政府公開資訊 API 製作一個查統編的工具網站

---

### Demo 截圖

首頁查詢統編

![](/images/free_business_directory_flask/001.png){:width="800px"}

進入內頁

![](/images/free_business_directory_flask/002.png){:width="800px"}

取得詳細資料

![](/images/free_business_directory_flask/003.png){:width="800px"}

如果不是統編格式的搜尋時，列出符合結果

但這一段受限 API 速度，之後有時間可以用 Redis 做定時的緩存

![](/images/free_business_directory_flask/004.png){:width="800px"}

---

### 實作方式

透過 Blueprint 方式，將多個模組整合到單一專案

![](/images/free_business_directory_flask/005.png){:width="800px"}

Flask 可以透過這種方式擴充額外模組，當然也可以在別的專案重用這些模組

![](/images/free_business_directory_flask/006.png){:width="800px"}

因為實際資料是由政府 API 取得，所以 Jenkins Pipeline 相對簡單

![](/images/free_business_directory_flask/007.png){:width="800px"}

<br>
<br>
<br>
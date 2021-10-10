---
layout: default
title: 台灣工商名錄
subtitle: "提供您每月即時更新的工商資訊"
---

<br>

### 這個 Site Project 使用 SSG - Static Site Generators 靜態網站生成方式建立  
### 並透過 Jenkins Pipeline 做 2 個專案 2 個 Agent 的建置佈署

[https://free-business-directory.hsuweni.info/](https://free-business-directory.hsuweni.info/)

專案目的：  
每月持續給相同格式的 CSV 檔案，然後轉成 Markdown 檔案  
再由 Jekyll 建置成 HTML+CSS+JS 的靜態資源  
然後佈署在 Raspberry Pi 中  
使用者進入內頁後再透過 API 取得詳細資訊

⚠️ 提醒  
這個專案主要是用來練習 Jenkins Pipeline CI/CD 和 Jekyll SSG  
像這種類型的網站，通常會有 10 幾萬筆的資料頁面存在，雖然使用 SSG 也是可以做的到  
但頁面數量那麼大的情況下，搜尋和比對如果沒有使用資料庫，會變得很複雜，所以實務上不推薦

---

### Demo 截圖

首頁列表

![](/images/2021-10-10/2021-10-10-12.png){:width="800px"}

進入內頁

![](/images/2021-10-10/2021-10-10-13.png){:width="800px"}

取得詳細資料

![](/images/2021-10-10/2021-10-10-14.png){:width="800px"}

---

### Jenkins 流程說明

這個建置流程使用 2 個 Agent，主要是因為 Pi ARM V7 無法使用 Jekyll Docker Image
1. Digital Ocean 雲端
2. Raspberry Pi w3

![](/images/2021-10-10/2021-10-10-01.png){:width="800px"}

首先在預設工作區檢查 2 個專案的 Git

![](/images/2021-10-10/2021-10-10-02.png)

然後在 Digital Ocean 指定一個 Python 工作目錄，執行 Python 將 CSV 轉成 md

![](/images/2021-10-10/2021-10-10-03.png)

然後一樣在 Digital Ocean 打包 md 檔案送到 JekyllBuild 工作區執行 SSG，然後在打包  
**就是因為這段無法在 Pi 做，所以才會使用 Digital Ocean**

![](/images/2021-10-10/2021-10-10-04.png)

然後將 Jekyll build 完的結果送到 Pi 裡的指定目錄，啟動 Nginx

![](/images/2021-10-10/2021-10-10-05.png)

最後檢查是否讀的到網站

![](/images/2021-10-10/2021-10-10-06.png)

只要源頭 Python 專案沒有更新 source 裡的 CSV 檔案  
代表產生的 _posts 裡的 md 檔案數量就不會改變  
流程是持續在 source 增加 CSV 檔案，然後用 Python 在 _posts 資料夾中產生新的 md 檔案

![](/images/2021-10-10/2021-10-10-07.png)

實際用 Jenkins 建置測試  
確定在資源來源不變的狀況下  
重複建置都會出現相同的結果，檔案數量不會增加  
確定在 JekyllBuild 階段

![](/images/2021-10-10/2021-10-10-08.png){:width="800px"}

每次結果都一樣

![](/images/2021-10-10/2021-10-10-09.png)

確定在 UnPackage 階段

![](/images/2021-10-10/2021-10-10-10.png){:width="800px"}

每次結果都一樣

![](/images/2021-10-10/2021-10-10-11.png)

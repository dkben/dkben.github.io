---
layout: default
title: OpenCart 2 改造成框架
subtitle: "OpenCart 2 改造成框架"
---

<br>

### 將 OpenCart2 改造成框架，讓工程師可以用比較現代的方式開發專案 

---

### 起因

在電商專案公司工作期間，公司主要使用 OpenCart 2 進行專案開發

估且不論好或壞，總之老闆沒有要換框架的意思

然後 OpenCart <strong>不是框架 ! 不是框架 ! 不是框架 !</strong>

所以用 OpenCart 來做大量的專案開發，而且還是深度客製化的開發，維護成本會很重

---

### 先了解現在的狀況

OpenCart 的原生架構為 MVCL, model, view, controller, language 4 種，前後台各一

因為多國語系的原因，官方版本在 model 中就用了大量的 JOIN 及原生 SQL 等方式

如果一不小心，是真的很容易被 SQL Injection

以下是原本的寫法

![](/images/opencart2_to_framework/001.png){:width="750px"}

然後在 view 中混合了 JS 和 language 中傳過來的語系變數

光找一個變數，就要開很多檔案了

![](/images/opencart2_to_framework/002.png){:width="800px"}

總之狀況如上

以下是針對公司的 OpenCart 2 進行一些架構上調整，讓程式比較方便重用和維護

---

### 開發環境

透過 Docker 啟動開發環境，因為是每個專案獨立，可以讓不同開發者擁有相同環境 (包含DB)

![](/images/opencart2_to_framework/003.png)

啟動時，會自動匯入指定目錄裡的 sql 檔案

![](/images/opencart2_to_framework/004.png)

除了使用 SQL Client 直接操作外，也可以用 phpMyAdmin 操作

![](/images/opencart2_to_framework/005.png)

---

### 系統架構調整

目標是把不怎麼好寫的 OpenCart 盡量改成較方便開發的結構

在 Symfony, Laravel 裡都可以看到類似的結構

然後區分專案和底層 2 個 Git 倉庫

因為大改是不可能的，如果要大改就用新框架就好

所以目標是寫一些方便的小工具，讓開發、維護可以方便

不用一直 DRY - "Don't repeat yourself"

![](/images/opencart2_to_framework/006.png)

---

### 樣版部份

雖然 OpenCart 3 已經改用 Twig Template 了，但 OpenCart 2 還是奇怪的 tpl 檔案 (不是 Smarty 的 tpl)

所以手動更換 Twig Template，及加幾個 Twig 工具進去

不過還是保留了原本的 tpl 樣版

![](/images/opencart2_to_framework/007.png){:width="250px"}

開發時，可以透過 .env 進行選擇

![](/images/opencart2_to_framework/008.png){:width="600px"}

---

### 語系的部份

同一份樣版可顯示不同語系檔案

透過自己寫的 Twig 工具，用來即時檢查及取得不同語系的字串

因為是中文的，所以在開發時也可以清楚知道美術排版位置

![](/images/opencart2_to_framework/009.png)

中文語系檔只要準備一個空檔案就好，首次會自動建立變數

![](/images/opencart2_to_framework/010.png)

英文版再刷新一次，會自動拷貝過來，再進行人工翻譯的動作

![](/images/opencart2_to_framework/011.png)

另外一個 Twig 工具是在樣版中傳入原生路由

自動轉成 SEO URL 或寫死的短網址工具

![](/images/opencart2_to_framework/012.png)

---

### Menu, Breadcrumb, Rule 整合

這是最花時間的部份，因為公司的 OpenCart 有 3 個路由

原生路由，寫死的短網址、客戶可以自訂的 SEO 短網址

其中一個可以讓客戶在後台進行修改，所以 Menu 和 Breadcrumb 也要可以動態判斷

![](/images/opencart2_to_framework/013.png){:width="800px"}

最後是多加了一個轉換的方法，用原生路由判斷是否擁有第 2, 3 路由

![](/images/opencart2_to_framework/014.png)

Breadcrumb 也是相同的判斷

![](/images/opencart2_to_framework/015.png)

專案中會有一個 Menu 設定檔，用來設定靜態寫死的資料

然後再透過 tag 來組合動態的 Menu 資料，例如商品、文章等等...

![](/images/opencart2_to_framework/016.png){:width="700px"}

---

### 增加效率的開發工具

因為 OpenCart 2 有點舊，沒有框架指令可以用

所以用 composer 方式建立自己的 CLI 指令

像是

1. PHPUnit
2. PHPStan
3. 掃描 DB Table 建立 Entity 檔案
4. 清空指定的 Table
5. 假資料匯入
6. Code Generator 模組拷貝生成器

![](/images/opencart2_to_framework/017.png){:width="600px"}

主要指令為快速拷貝更名後台的模組，所以可以快速把後台的程式碼、資料表準備好

![](/images/opencart2_to_framework/018.png){:width="800px"}

當然，我是透過一個模組設定檔來判別模組

所以也可以自己把寫過的模組拷貝到底層中，之後可以重用，效率會提昇不少

![](/images/opencart2_to_framework/019.png){:width="800px"}

指令操作記錄

![](/images/opencart2_to_framework/020.png){:width="800px"}

也有移除模組指令

在移除模組後，為了安全起見，也會在備份資料夾中備份移除的模組程式及資料表

![](/images/opencart2_to_framework/021.png){:width="500px"}

---

### 後記

之前就發現公司工程師每個人的環境都不太相同，所以首先做的是整合開發環境。

針對前台的變動比較多，主要是讓工程師可以用比現代的語法進行開發，且每個專案前台都長的不太一樣，所以才這麼選擇。

後台基本上沒什麼修改，只是增加一些CLI指令，用來拷貝包裝好的模組，填充資料等等，用來快速建立後台功能。

但一些 ORM Entity 等 Class，前後台都可以通用，在開發維護上會比較一致。

<br>
<br>
<br>
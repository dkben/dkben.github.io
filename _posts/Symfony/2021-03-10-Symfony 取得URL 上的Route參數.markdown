---
layout: post
title:  "Symfony 取得URL 上的Route參數"
subtitle: "可以使用以下方式取得 Route 參數"
date:   2021-03-10 09:00:00 +0800
categories: Symfony
meta_description: "Symfony 取得 URL 上的 Route 參數"
tags:
- "Symfony"
---

可以使用以下方式取得 Route 參數

![](/images/medium/1__JCLIWfJAqok3yfntffMsmA.png)

程式碼如下

```php
$routeParams = $request->attributes->get('_route_params');
dd($routeParams['id']);
```

確定有抓到 route 的 id 值

![](/images/medium/1__1x0T25gWL9o62LDnUCWUfw.png)

在渲染 edit 頁面時，又把 id 傳了出去

主要是因為 edit 是使用 Vue JS 前後分離開發，需要這個 id 再 call 一次 API 取得資料

當然，修改頁面所需要的資料也可以在這裡傳出，只是選擇的實作方法不同而以

![](/images/medium/1__C5xgHIMVRhwu7UCrJYJG7Q.png)
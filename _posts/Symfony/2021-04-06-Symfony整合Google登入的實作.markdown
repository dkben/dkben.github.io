---
layout: post
title:  "Symfony整合Google登入的實作"
subtitle: "Symfony整合Google登入的實作"
date:   2021-04-06 09:00:00 +0800
categories: Symfony
tags:
- "Symfony"
---

跟整合 Facebook 的方式一樣，參考以下方式

> Easily talk to an OAuth2 server for social functionality in Symfony
>
> [https://github.com/knpuniversity/oauth2-client-bundle](https://github.com/knpuniversity/oauth2-client-bundle)
>
> Easily implement Google login with Symfony 4
>
> [https://hugo-soltys.com/blog/easily-implement-google-login-with-symfony-4](https://hugo-soltys.com/blog/easily-implement-google-login-with-symfony-4)

Google 開發者介面

[**Google Cloud Platform**  
_Google Cloud Platform lets you build, deploy, and scale applications, websites, and services on the same infrastructure…_console.cloud.google.com](https://console.cloud.google.com/apis/credentials "https://console.cloud.google.com/apis/credentials")[](https://console.cloud.google.com/apis/credentials)

不過使用 Google 時出現了授權錯誤

![](/images/medium/1__FbFqC4wjNi5pfwv8b0uWOg.png)

後來發現每一個 Google Cloud Platform 專案只能設定一個網站的 OAuth，原本的已經有別的專案使用了，所以設定的 callback 位置會造成授權錯誤

再建立一個新的專案

![](/images/medium/1__49__keYBxR__ZzFU__lGWbaAg.png)

切換到新的專案

![](/images/medium/1__5wnw0tKa__ncmfxfPy2X4Ug.png)

設定憑證和 OAuth 資訊

![](/images/medium/1__RSC9UqU__vpNJeNMNJ40fHg.png)

測試 Google 登入

![](/images/medium/1__db0r1li5FsGApb76zG3kRw.png)

成功了，可以選擇 Google 帳戶了

![](/images/medium/1__a2Iw4aPYo6IzaFVAGoN7jA.png)

登入後也可以取得資訊

![](/images/medium/1__LT7T0g8aQzfsPhZk2rXlqQ.png)

透過 Google 註冊新建帳號也 OK

![](/images/medium/1__168iw5ypgvNhmfsxu9Ob7A.png)

但登出後，因為 Symfnoy 5 的router 設定可能有問題，自動被導向 en 語系了

![](/images/medium/1__eElm1xNYmJbxLdj0t5426Q.png)

此時如果再次使用 Google 登入，就會造成授權錯誤，因為 callback 位置多了一個 en 字綴

![](/images/medium/1__gQnmkpGvgZWUVfGlD0HTPA.png)

把所有會用到的語系 URI 都加到授權清單中

![](/images/medium/1__P4Eo__Yeavf0aS1upVX__lXQ.png)

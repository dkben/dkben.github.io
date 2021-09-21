---
layout: post
title:  "Symfony整合Facebook登入的實作"
subtitle: "Symfony整合Facebook登入的實作"
date:   2021-04-06 09:00:00 +0800
categories: Nginx
tags:
- "Nginx"
---

大致上是都是參考以下教學設定

> Easily talk to an OAuth2 server for social functionality in Symfony
>
> [https://github.com/knpuniversity/oauth2-client-bundle](https://github.com/knpuniversity/oauth2-client-bundle)
>
> Easily implement Facebook login with Symfony 4
>
> [https://hugo-soltys.com/blog/easily-implement-facebook-login-with-symfony-4](https://hugo-soltys.com/blog/easily-implement-facebook-login-with-symfony-4)

Facebook 開發者介面

[**Facebook for Developers**  
_Code to connect people with Facebook for Developers. Explore AI, business tools, gaming, open source, publishing…_developers.facebook.com](https://developers.facebook.com/ "https://developers.facebook.com/")[](https://developers.facebook.com/)

資料表如下，要先增加一個 facebook_id 欄位 ( ORM 屬性是 facebookId )

![](/images/medium/1__iaOi4fCpFDafeT7AjAFnJw.png)

教學上有提到，認證有 2 種方式

1.  Authenticating with Guard (用的是這個)
2.  Using the new Symfony Authenticator

設定檔 security 設定如下

config/packages/security.yaml

開啟 authenticator

```php
security:  
enable_authenticator_manager: true
```

設定帳號提供者

```php
providers:    
    app_user_provider:  
    entity:  
        class: App\\Entity\\User  
        property: email  
    oauth:  
        id: knpu.oauth2.user_provider
```

防火牆設定

```php
firewalls:  
    dev:  
        pattern: ^/(_(profiler|wdt)|css|images|js)/  
        security: false  
    main:        
        entry_point: App\\Security\\LoginFormAuthenticator  
        pattern: ^/  
        user_checker: App\\Security\\UserChecker  
        anonymous: false  
        lazy: true  
        provider: app_user_provider  
        guard:  
        authenticators:  
        - App\\Security\\LoginFormAuthenticator  
        - App\\Security\\MyFacebookAuthenticator
```

要建立 Controller 類別

src/Controller/FacebookController.php

要建立 Facebook 自己的 Authenticator 類別

src/Security/MyFacebookAuthenticator.php

⚠️ 注意！此時是使用 Facebook 官網提供的按鈕語法

![](/images/medium/1__p70wIfoxizVKMWDkirPKjQ.png)

會跳出一個彈跳視窗登入

![](/images/medium/1__0q1JsH8pKi26wBz8dhOjXQ.png)

登入驗證後，透過 Facebook Authenticator 檔案取得資訊

![](/images/medium/1__ZICXSmVeawgxk4tVAmJLVw.png)

![](/images/medium/1__XbP__k55SIL9pi55mpzw54A.png)

確定可以取得資訊

![](/images/medium/1__EIo7Vh__3jRvo6__lkqFTI2A.png)

測試一下，要如何從 Facebook 中取得各個屬性

src/Security/MyFacebookAuthenticator.php

![](/images/medium/1____KDLRHG3ZTWIq8__mLFB6WA.png)

確定可以取得各項資訊

![](/images/medium/1__zd____tIWgZiXngJ8A8ikGqw.png)

可以透過這種方式註冊和登入了

![](/images/medium/1__R4zJqTCHQsQj5nho__5iknA.png)

### **不過此時登入後，並不會自動導向系統後台… 卡很久…**

回到一開始的 Facebook 設定，上面的 JS 設定仍是要設定在 Twig 樣版中，但如果使用 Facebook 提供的按鈕，反而會造成登入後無法自動導向

![](/images/medium/1__x7DjvtG9eem1Gq9NFVJqhw.png)

紅色框框是 Facebook 提供的

![](/images/medium/1__aSH1sEoXF8zxGwAePwx12g.png)

按鈕效果如下

![](/images/medium/1__gH6MD3gSGEKDAdBY__G4e1g.png)

有效的 OAuth URI 並沒有特別設定

![](/images/medium/1__7KPrbjoZkEI9vHJpSfnN0w.png)

解決方法是，不要用 Facebook 提供的 button，使用自定的 a 連結語法

![](/images/medium/1____ikScr38m__2if1965geUhw.png)

測試 Facebook 登入

![](/images/medium/1__J63ZLnZhwF0fnF__MIttXAQ.png)

此時不會另外開新視窗登入，而是頁面直接跳轉，登入後就會跳回系統後台首頁了

![](/images/medium/1__KCPQ__95h7xJ9muuTnhL02g.png)

原因可能是使用 Facebook 提供的按鈕會另開新視窗登入，登入後無法導回原本的系統 URI 位置

但如果不另外開新視窗，就不會有這個問題

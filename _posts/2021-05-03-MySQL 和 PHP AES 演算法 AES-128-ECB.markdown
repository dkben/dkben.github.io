---
layout: post
title:  "MySQL 和 PHP AES 演算法 AES-128-ECB"
subtitle: "注意 MySQL 5.7 的 AES 加密演算法預設值是 AES-128-ECB"
date:   2021-05-03 09:00:00 +0800
categories: Nginx
tags:
- "Nginx"
---

注意 MySQL 5.7 的 AES 加密演算法預設值是 AES-128-ECB

![](/images/medium/1__hoLbq8__JA0TdxuZhKonWUQ.png)

AES-128-ECB 不需要 Initialization Vector 值

key 的長度會依不同演算法有不同規則，使用 AES-128-ECB 演算法時，最多 16 字元

![](/images/medium/1__q0bV2j4hPzPw39ufLLo8Yw.png)

```php
// $key = hash("sha256", $AES_key);  // 不要這樣用，字串太長的話 MySQL 會另外做處理
$key = substr(hash("sha256", $AES_key), 0, 16);  // key 的長度會依不同演算法有不同規則，使用 AES-128-ECB 演算法時，最多 16 字元
```

使用 PHP 和 MySQL 進行編碼，得到一致的結果

![](/images/medium/1__0sfz9WOLSQG2zNgjx3RE__w.png)

做 Equal 比對

![](/images/medium/1__L8AnrwO1o__Y3H__anK17Zug.png)
![](/images/medium/1__eVwENC__R5ABuRpApwJHW5A.png)

做 LIKE 搜尋

![](/images/medium/1__UCYOUcSQS15I0fbsR2__J4g.png)
![](/images/medium/1__plgsvxhGhOpVnAIIMS2UcA.png)



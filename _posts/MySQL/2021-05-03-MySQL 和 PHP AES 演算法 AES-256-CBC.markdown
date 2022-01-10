---
layout: post
title:  "MySQL 和 PHP AES 演算法 AES-256-CBC"
subtitle: "注意 MySQL 5.7 的 AES 加密演算法預設值是 AES-128-ECB"
date:   2021-05-03 09:00:00 +0800
categories: MySQL
meta_description: "MySQL 和 PHP AES 演算法 AES-256-CBC，注意 MySQL 5.7 的 AES 加密演算法預設值是 AES-128-ECB"
tags:
- "MySQL"
---

注意 MySQL 5.7 的 AES 加密演算法預設值是 AES-128-ECB

![](/images/medium/1__hoLbq8__JA0TdxuZhKonWUQ.png)

要先修改成 AES-256-CBC

![](/images/medium/1__2PnaO4__ZWuYP0wuYV__AHZw.png)

單次修改

`SET block_encryption_mode = 'aes-256-cbc';`

全域修改

`SET global block_encryption_mode = 'AES-256-CBC';`

查看

`SHOW VARIABLES LIKE '%block%';`

不過正式時，要直接修改 my.cnf，確定編碼規則在每一次資料載重啟時都一致，不然會有問題

> [https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-block-encryption-mode.html](https://dev.mysql.com/doc/mysql-secure-deployment-guide/5.7/en/secure-deployment-block-encryption-mode.html)

第 1 點：原本主 key 有使用 hash 256 取得一個 64 字元

> [https://stackoverflow.com/questions/2240973/how-long-is-the-sha256-hash/2241014](https://stackoverflow.com/questions/2240973/how-long-is-the-sha256-hash/2241014)

第 2 點：然後再用這個加密後的 key 再做一次 hash 但只取 16 個字元 ( Initialization Vector ) ( iv ) 向量初始化

第 3 點：是告訴 PHP 使用 openssl_encrypt 時，要返回 Raw 格式資料

第 4 點：自己做 Base 64 編碼

⚠️ 如果透過 openssl_encrypt 處理，會得到預期外的結果，所以自己用 PHP 或 MySQL 的 Base 64 處理

![](/images/medium/1__vOQ__D__81G__7YRNXjY1cL1A.png)

透過加密後的 key 和 iv 加密儲存的值，但發現雙方不一致

![](/images/medium/1__Pm8OKauNvPYt__VMJySCXCA.png)

主 Key 不做 hash 試試

![](/images/medium/1__XEugfgK1bN6wScpcO8wxVg.png)

雙方加密後的值一致了

![](/images/medium/1__l__q9wQiinfwIYBP__W9daeQ.png)

⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️⚠️ 重要注意事項

所以目前知道一個狀況

就是 key 太長，會造成 PHP 和 MySQL 做 AES 的結果不一致

這是 Google 到的資訊

密鑰長度：AES-128-ECB 密鑰長度為 128bits (As of MySQL 5.6.17, key lengths of 196 or 256 bits can be used)，但使用 AES\_ENCRYPT()、AES\_DECRYPT() 時，如果輸入太短或太長的密鑰，MySQL 會自動處理成 128bits

> [https://security.stackexchange.com/questions/190611/mysql-aes-encrypt-string-key-length](https://security.stackexchange.com/questions/190611/mysql-aes-encrypt-string-key-length)

解決方法就是減少主 key 的字串長度

然後 Initialization Vector 也是一樣，要注意是否符合演算法的規範

> [https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html](https://dev.mysql.com/doc/refman/8.0/en/encryption-functions.html)

For modes that require the optional init\_vector argument, it must be 16 bytes or longer (bytes in excess of 16 are ignored). An error occurs if init\_vector is missing.

![](/images/medium/1__f3UnQ3pPDiiDR3S6jox3hA.png)

做 Equal 比對

![](/images/medium/1__aVGm8S1jx7BN5tCu1yDYiQ.png)

![](/images/medium/1__4QpGUd7gicqUXXp6EmkXhQ.png)

做 LIKE 搜尋

![](/images/medium/1__xBIFBYAG6__Mi6L6a3Yg5Gg.png)

![](/images/medium/1____dMER8WwVZjUEBW05XduCw.png)

⚠️ 注意事項

因為用 PHP 程式或 MySQL 的 AES 解編碼後會得到二進位資料

如果資料欄位不是用二位進檔案形式儲存的話 ( BINARY, VARBINARY, TINYBLOB, BLOB, MEDIUMBLOB, LONGBLOG )

就要另外做一次 BASE 64 的解編碼

像下圖就是欄位是一般的 VARCHAR，不過儲存時送進去是二進位，所以造成亂碼，此外也不一定能再取出解碼 AES 復原成原本的字串

![](/images/medium/1__IqFFYWWjh6cTLt__Osq50MQ.png)

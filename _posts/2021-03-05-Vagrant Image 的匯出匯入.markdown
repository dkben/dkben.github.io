---
layout: post
title:  "Vagrant Image 的匯出匯入"
subtitle: "某些環境不容易重現時，可以把現有的虛擬機整個匯出，然後使用在別的環境"
date:   2021-03-05 09:00:00 +0800
categories: Jekyll
tags:
- "Jekyll"
---

某些環境不容易重現時，可以把現有的虛擬機整個匯出，然後使用在別的環境

進入專案資料夾中

匯出 ( ⚠️ 匯出時，會自動關閉啟動的虛擬機，之後要再手動開啟 )

`vagrant package --output php71oci8.box`

![](/images/medium/1__gcLKxw__PbLYJgZw2ylzErQ.png)

匯入並更名，在匯出的資料夾做就好了，匯入後就可以把 Image 印象檔刪除

`vagrant box add --name php71oci8 ./php71oci8.box`

![](/images/medium/1__g6JNgfHgbOSib9htwI2emQ.png)

將原本外部的 Image 名稱，然後 Vagrantfile 的內容要修改一下，因為不需要重複執行安裝套件的指令了，原本匯入的 Image 印象檔已經有了

![](/images/medium/1__aIM9Qnqa6JN4Bfqdo56Obw.png)

修改成剛剛匯入的 Image 名稱

![](/images/medium/1__VmpY__tjF6zlaRzyZhafUDA.png)

啟動

`vagrant up`

![](/images/medium/1__dObmwIhXk1E__Ia__Ckc4hPA.png)

管理

`vagrant box list`

![](/images/medium/1__AnvYlkYjl0kUz__rghAd__ng.png)

移除一個印象檔

`vagrant box remove php71oci8`

![](/images/medium/1__bTCgT86O2rbzTOu__H2nn3A.png)
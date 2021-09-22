---
layout: post
title:  "PhpStorm+Docker+PHP8+Xdebug3的設定方式"
subtitle: "Configure Xdebug"
date:   2021-07-11 09:00:00 +0800
categories:
tags:
- "2021"
---

Configure Xdebug

> [https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html](https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html)

在本機查看 Container 裡的 PHP 資訊，確定有裝 Xdebug 3

`docker exec -ti opencart3_web_1 sh -c "php -v"`

![](/images/medium/1____oVP0D4WPV7hbZsi9TdLAw.png)

設定 Debug

⚠️ 下圖的 2 要打勾

![](/images/medium/1__2dwILtt2yA1hyubfuvT6__w.png)

設定 Server，Docker Container 所使用的本機埠號和資料夾對應要設定好

![](/images/medium/1__2iUQUXqoGVqBUOOBoxfWQg.png)

設定執行設定，建立一個 PHP Web Page，選擇上面建立的 Server

![](/images/medium/1__3NVBix5eH0NsA7gtnoloGg.png)

啟動監聽，按下綠色蟲蟲的按鈕，選擇入口點

![](/images/medium/1__j22DQYF__HKbNX9J9In4Xzw.png)

如果出現以下警告，代表 mapping 的資料夾不正確，重新設定

![](/images/medium/1__yhMC0H7ibbve5qudi__iZkg.png)

設定本機和 Container 裡的資料夾對應

兩邊的相對位置可以不同，所以才會讓使用者視需要一個一個資料夾設定對應關係

![](/images/medium/1__J33ApfTuQMYnPE5LtLRsAQ.png)

前台會一直轉圈圈

![](/images/medium/1__v6NLPuYk93Jc__dC8w2w25Q.png)

透過以下按鈕進入下一步

![](/images/medium/1__73fLto3FsY0Psiz1hE8swA.png)

選擇 Debugger 標籤查看

![](/images/medium/1____9HL5hHZZvt3wNtttZVTRA.png)

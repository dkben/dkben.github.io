---
layout: post
title:  "Hubot 聊天機器人串 Slack，並使用 GitLab CICD 自動佈署及啟動"
subtitle: "Hubot, Slack, GitLab CICD"
date:   2021-03-16 09:00:00 +0800
categories: integrations
tags:
- "Robot"
---

先到自己的 Slack 安裝 Hubot App

[https://hsuweni.slack.com/apps](https://hsuweni.slack.com/apps)

![](/images/medium/1__9bHpKKti56H9osZAo7bKig.png)

給它一個名字

![](/images/medium/1__cR8UkhG0__vhxUKO7Qpy0EA.png)

會得到一個 API Key

![](/images/medium/1__oaYW3oKw97__n1jYcNRoZXw.png)

yeoman 是一個整合工具包 SCAFFOLDING TOOL 腳手架工具

[https://yeoman.io/](https://yeoman.io/)

全域安裝

npm install -g yo generator-hubot

![](/images/medium/1__7qPYpHr20smVxs7LRKV5Hg.png)

一直無法成功…

算了，睡覺去….

結果一覺醒來，重新嘗試就可以安裝了

不過我習慣電腦不關機，但會把程式關閉，所以有可能是 terminal 重啟的關係

![](/images/medium/1__ix9jgYF4WNB__2CT8TjpSyw.png)

建立一個 catbot 專案

進專案資料夾

yo hubot --adapter=slack

![](/images/medium/1__PhN6ANbQhfQRBnH6yhnAJw.png)

使用在 Slack 網站申請的 API Key 執行以下指令

HUBOT\_SLACK\_TOKEN=xoxb-1299715080640-1559432386100-xxxxxxxxxx ./bin/hubot --adapter slack

![](/images/medium/1__7vEUvoo9C9dxZ__vcpxFXDQ.png)

要輸入關鍵字 help 才有效

![](/images/medium/1__pt__abw7JWPgLtYFwcw__bWQ.png)

簡單測試 ping，有回應 PONG 就是成功

![](/images/medium/1__CQvZpTGuGEt1GX2ZRjlmlw.png)

提示有說明 image me 是請 catbot 送一張圖片過來，但也提示要你自己設定自訂的搜尋引擎，預設的 Google 不一定能一直提供

![](/images/medium/1__VfbzeUYzYsVOBPpDFoQsIg.png)

也可以叫聊天機器人給我地圖

![](/images/medium/1__joJTW8YcDX0OJlmtNjQmrg.png)
![](/images/medium/1__8gctdPHOoWzHT1y8CCNTkw.png)
![](/images/medium/1__ZQLxBpP5A4DJdywYcAT0JQ.png)
![](/images/medium/1__v8jiQXVfhPu0NNsS5J3flA.png)

另外一個預設指令 pug me

![](/images/medium/1__QPNLaTW5928hHl6KBd__Ruw.png)

因為 Cabot 預設是使用 coffee script

照著搜尋到的教學加入一個 configcat.coffee 檔案

![](/images/medium/1__QrFA4MuZfQJei5Tsinx7uA.png)

輸入以下

![](/images/medium/1__TxFDoCgdjt7wlq3n71oEUw.png)

無效….

![](/images/medium/1__bXGUMEj517xE6__1Q8JvQVg.png)

有可能是它引用的 API 位置已經掛掉了

![](/images/medium/1__OO9t5CuM__Sd3jX38p32cjg.png)

不要用 Coffee Script，用 ES6 語法來寫

[https://keestalkstech.com/2018/01/hubot-es6-promises/](https://keestalkstech.com/2018/01/hubot-es6-promises/)

![](/images/medium/1__iPk8KLDf__MQM3gArSO6QNw.png)

一樣失敗…

![](/images/medium/1__E60P95kyD4Z8xKLh4LxPJw.png)
![](/images/medium/1__Pk4mVQEIcE0zxIbhz5si1w.png)

有提示是說找不到 decode-html 這個模組

![](/images/medium/1__SPQ3kNTSaRcZp50uaAS1VQ.png)

在專案中使用 npm 安裝

npm install decode-html

輸入關鍵字「norris」，有回應了，隨機返回一個笑話

![](/images/medium/1__glVFFTHVYJ8THACruqnz0A.png)

把它搬到自己的 Raspberry Pi 4 主機上執行

HUBOT\_SLACK\_TOKEN=xoxb-1299715080640-1559432386100-xxxxxxxxxx ./bin/hubot --adapter slack

![](/images/medium/1__1S5VpmcilcH1n1Olse77Zg.png)

除了在專案中建立佈署主機的 SSH Key 外

![](/images/medium/1__RfR1yZVP01fUoWFyW6rpag.png)

也要建立一開始的 Slack Hubot Key

![](/images/medium/1__VuvcMbF2NG3dfMHfJIMofA.png)

注意⚠️

因為佈署是透過 GitLab 的 CICD，所以要注意啟動機器人的語法

before\_script:

\- apt-get update -qq

\- apt-get install -qq git

\- 'which ssh-agent || ( apt-get install -qq openssh-client )'

\- eval $(ssh-agent -s)

\- ssh-add <(echo "$PI4\_SSH\_PRIVATE\_KEY")

\- mkdir -p ~/.ssh

\- '\[\[ -f /.dockerenv \]\] && echo -e "Host \*\\n\\tStrictHostKeyChecking no\\n\\n" > ~/.ssh/config'

deploy\_staging:

type: deploy

environment:

name: staging

url: http://220.132.xxx.xxx

script:

\- ssh ben@220.132.xxx.xxx -p10022 "cd /home/ben/projects/web && \\rm -R catbot/ && mkdir catbot && exit"

\- ssh ben@220.132.xxx.xxx -p10022 "cd /home/ben/projects/web/catbot && git clone git@gitlab.com:dkben/catbot.git . && npm install && exit"

\- ssh ben@220.132.xxx.xxx -p10022 "cd /home/ben/projects/web/catbot && eval nohup HUBOT\_SLACK\_TOKEN=$HUBOT\_SLACK\_TOKEN ./bin/hubot --adapter slack &"

only:

\- master

CICD 中有 passed 不代表機器人一定啟動成功

![](/images/medium/1__K__sN__LuupWrPbu6ayGYTgQ.png)

如果有要修改 CICD 的佈署腳本，最好還是本機先執行確認再 push

![](/images/medium/1__kCelmG39kJk1aqYBv__sMag.png)

使用 GitLab CICD 佈署後，確定機器人有啟動

![](/images/medium/1__kY53nIB1__YZNrQwnhBKN__g.png)

> 參考

> [https://dev.to/configcat/complete-guide-to-build-a-slack-chatbot-in-7-minutes-and-host-it-for-free-1ef8](https://dev.to/configcat/complete-guide-to-build-a-slack-chatbot-in-7-minutes-and-host-it-for-free-1ef8)

> [https://chatbotslife.com/building-a-slackbot-with-hubot-85016dd1477e](https://chatbotslife.com/building-a-slackbot-with-hubot-85016dd1477e)

> [http://springest.io/hubot-part-1-get-it-running-locally-in-slack](http://springest.io/hubot-part-1-get-it-running-locally-in-slack)

> [https://hubot.github.com/](https://hubot.github.com/)

> [https://github.com/hubotio/hubot/blob/master/docs/scripting.md](https://github.com/hubotio/hubot/blob/master/docs/scripting.md)

> [https://www.loggly.com/blog/building-chatops-bot-slack-loggly-part-1/](https://www.loggly.com/blog/building-chatops-bot-slack-loggly-part-1/)

> [https://chatbotslife.com/building-a-slackbot-with-hubot-85016dd1477e](https://chatbotslife.com/building-a-slackbot-with-hubot-85016dd1477e)

> [http://springest.io/hubot-part-1-get-it-running-locally-in-slack](http://springest.io/hubot-part-1-get-it-running-locally-in-slack)

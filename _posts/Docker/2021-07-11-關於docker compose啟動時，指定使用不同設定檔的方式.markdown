---
layout: post
title:  "關於docker compose啟動時，指定使用不同設定檔的方式"
subtitle: "原本一直想使用 .env 的方式載入，但一直無法成功"
date:   2021-07-11 09:00:00 +0800
categories: Docker
meta_description: "無法在 docker-compose.yml 中針對不同環境啟動使用不同的 .env，但可以使用不同 docker-compose.yml 設定檔啟動"
tags:
- "Docker"
---

原本一直想使用 .env 的方式載入，但一直無法成功

> — env-file option #6170
>
> [https://github.com/docker/compose/issues/6170](https://github.com/docker/compose/issues/6170)
>
> Environment variables in Compose
>
> [https://docs.docker.com/compose/environment-variables/](https://docs.docker.com/compose/environment-variables/)

類似這樣

![](/images/medium/1__CtJsdEzRDzevj04hgz2Y8Q.png)

不管是用這樣

`docker compose --env-file ./.env.dev up -d`

還是這樣

`docker compose --env-file=./.env.dev up -d`

啟動後 env 的值都帶不進去

查看環境變數也是看不到

`docker compose --env-file ./.env.dev config`

這樣也看不到

`docker compose --env-file=./.env.dev config`

經過一個下午的測試 ( 在我的 Mac Pro 中 )

只有這種方式可以有效區分不同的環境變數

![](/images/medium/1__oVEErF28371fYP3NYTQfvw.png)

啟動後

`docker compose up -d`

查看啟動後的環境變數

`docker compose config`

![](/images/medium/1__yV__vg283SFl6__yAKde9vkg.png)

⚠️ 發現如果啟動時，使用不同的設定檔

`docker compose -f docker-compose.dev.yml up -d`

當下達以下指令時，仍會返回 docker-compose.yml 的值

`docker compose config`

但啟動的 Container 確實是使用不同的設定檔沒錯

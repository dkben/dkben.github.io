---
layout: post
title:  "Found orphan containers 發現孤立容器"
subtitle: "這是發生在移除環境時的時候"
date:   2021-08-22 09:00:00 +0800
categories: Docker
tags:
- "Docker"
---

只要 docker-compose.yml 裡面有 compose, nodejs 容器就會有 orphan containers 孤立容器，我是指下圖我的專案開發環境

每手動執行一次指令也會產生額外的容器

這是發生在移除環境時的時候

`docker-compose down`

![](/images/2021-08-22/2021-08-22-01.png){:width="800px"}

出現 Warning 警告

> WARNING: Found orphan containers (psi_nodejs_1, psi_composer_1) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up.

發現孤立容器

`docker-compose down --remove-orphans`

![](/images/2021-08-22/2021-08-22-02.png)

看來在 docker-compose 中如果沒有 always 啟動的話，就會變孤立容器
> Removing orphan container “psi_composer_1"
> 
> Removing orphan container “psi_nodejs_1"
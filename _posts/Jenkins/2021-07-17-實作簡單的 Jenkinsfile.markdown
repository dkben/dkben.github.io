---
layout: post
title:  "實作簡單的 Jenkinsfile"
subtitle: "使用 Pipeline Declarative Style 聲明式"
date:   2021-07-17 09:00:00 +0800
categories: Jenkins
meta_description: "實作簡單的 Jenkinsfile，使用 Pipeline Declarative Style 聲明式"
tags:
- "Jenkins"
---

建立一個專案

![Untitled](/images/2021-07-17/2021-07-17-01.png){:width="800px"}

建立 Jenkinsfile 檔案，內容如下，其中 label w3 代表使用標籤為 w3 的節點 Server

```yaml
pipeline {
    agent {
        label 'w3'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```

在 Jenkins 中建立一個 Pipeline 專案，選擇 SCM 方式

![Untitled](/images/2021-07-17/2021-07-17-02.png){:width="800px"}

執行建置

![Untitled](/images/2021-07-17/2021-07-17-03.png){:width="800px"}

> 官方文件
> 
> [使用 Jenkinsfile](https://www.jenkins.io/zh/doc/book/pipeline/jenkinsfile/)
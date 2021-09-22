---
layout: post
title:  "Raspberry 4 Jenkins—樹莓派 4 安裝執行 Jenkins (全記錄)"
subtitle: "記錄使用 Raspberry 4 來架設 Jenkins 服務"
date:   2021-03-16 09:00:00 +0800
categories: RaspberryPi
tags:
- "RaspberryPi"
---

記錄使用 Raspberry 4 來架設 Jenkins 服務

事先需要設定光世代埠號轉接

![](/images/medium/1__Fk0IpcxkMZt2CmgmPpLVsA.png)

**Host 安裝 Arm 版本的 Jenkins**

在網路上找到一個可以支援 ARM CPU 的 jenkins Image

> [https://hub.docker.com/r/jenkins4eval/jenkins](https://hub.docker.com/r/jenkins4eval/jenkins)

注意要先建相關目錄掛載

`sudo docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v /home/pi/jenkins\_home:/var/jenkins\_home jenkins4eval/jenkins`

**使用 root 身份登入 jenkins Docker Container 中**

不能使用一般方式登入容器，這樣登入的話，要切到 root 時會需要密碼，但我們不會知道 root 密碼 ( 因為那 image 不是我們自己建的 )

`sudo docker exec -ti jenkins bash`

要使用以下語法

`sudo docker exec -it -u root jenkins /bin/bash`

登入容器後就是 root 身份了

![](/images/medium/1__j__JQw6ptGR__KM5E2p5EMkw.png)

> 參考
>
> [https://stackoverflow.com/questions/33272054/how-can-i-get-docker-container-roots-password](https://stackoverflow.com/questions/33272054/how-can-i-get-docker-container-roots-password)

**安裝 Composer ( 要先裝 PHP )**

登入 jenkins 容器後，先升級系統

`apt-get update`

安裝 PHP 7

`apt-get install -y php7.0 php7.0-xdebug php7.0-xsl php7.0-dom php7.0-zip php7.0-mbstring`

![](/images/medium/1__N__Fs2Cx95vNnk9yKqyACYA.png)

安裝 composer

`php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`

![](/images/medium/1__FUeU822__9bxuMAREVYO0fA.png)

`php composer-setup.php --install-dir=/usr/local/bin --filename=composer`

![](/images/medium/1__LXf__kcwBkkqRv__Ux0ENWMA.png)

`php -r "unlink('composer-setup.php');"`

![](/images/medium/1__KpRNwUDV5FaUVp12Cy2HIQ.png)

使用 root 身份執行

`composer`

![](/images/medium/1__hQUNFfzqci6PVPc0B712EQ.png)

退出容器，用 jenkins 身份登入

`sudo docker exec -ti jenkins bash`

執行

`composer`

![](/images/medium/1__HZasvWaKVDLoNYfO0dQ5lw.png)

> 參考
>
> [https://modess.io/jenkins-php/](https://modess.io/jenkins-php/)

**安裝 Node, NPM**

如果指令是 nodejs 代表裝錯，移除重裝

> [https://stackoverflow.com/questions/48943416/bash-npm-command-not-found-in-debian-9-3](https://stackoverflow.com/questions/48943416/bash-npm-command-not-found-in-debian-9-3)
>
> [https://linuxize.com/post/how-to-install-node-js-on-debian-10/](https://linuxize.com/post/how-to-install-node-js-on-debian-10/)

指令中有 sudo 的話要去除，因為用上述方法登入已經是 root

`curl -sL https://deb.nodesource.com/setup\_12.x | bash -`

裝完後 node 和 npm 應該都會存在

![](/images/medium/1__VzyAPEbbGHZ2LFb7bC4TUQ.png)

![](/images/medium/1__igTUbQ8KJz__m4o0Ujdj7qQ.png)

**執行 Jenkins 建置**

這裡會執行很久，視專案而定，目前跑了 15 分鐘以上

![](/images/medium/1__NXT__mPrCmnZDixhlOjo63w.png)

不過就算 PHP composer 和 NPM 可以跑完，最後仍會出錯

![](/images/medium/1__cAUI8QU90XVlGIh12xtNGA.png)

出錯的原因是 jenkins 無法調用 host 的 docker 指令

`sudo docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /home/pi/jenkins_home:/var/jenkins_home jenkins4eval/jenkins`

不過一開始建立 jenkins 的指令確實有使用 -v 把 host 的 docker 掛到 jenkins 容器中

進入 jenkins 容器中查看，確實有 docker.sock

![](/images/medium/1__ntoSzX9rtcBDMTr0XzLFnw.png)

所以要注意一開始建立 jenkins 容器時，是否有加上 /usr/bin/docker 位置

`sudo docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v /home/pi/jenkins_home:/var/jenkins_home new-jenkins`

不過此時在 jenkins 中可以使用 host 端的 docker 指令，但會報權限不足的錯誤

( 記得要用一般身份進 jenkins 容器中，使用 docker ps 指令測試，不能只下 docker 指令 )

> Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get [http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json:](http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/json:) dial unix /var/run/docker.sock: connect: permission denied

經過一連串的失敗…

最後在 host 中，使用 pi 身份執行

`sudo chmod 666 /var/run/docker.sock`

![](/images/medium/1__XLlDW2Cqi5jU2j__teLcKYQ.png)

終於可以在 jenkins 容器中，使用一般身份 jenkins 執行 docker 指令了

![](/images/medium/1__Vde5gZKrFjDPzGbUlPXsow.png)

> 參考
>
> [https://github.com/palantir/gradle-docker/issues/188](https://github.com/palantir/gradle-docker/issues/188)
>
> [https://blog.csdn.net/kikajack/article/details/79806520](https://blog.csdn.net/kikajack/article/details/79806520)

接下來執行 Jenkins 建置，起碼可以動作了，剩下就是一些專案設定調整了

![](/images/medium/1__YdaU3a__ZMTHtH0L6c8iGbQ.png)

![](/images/medium/1__wBiPoPSkLqt__RB__y4Iz7lg.png)

雖然建置成功，但由於 ARM CPU 的架構，仍然是出現無法執行容器服務的狀況

![](/images/medium/1__n8QG6OWS90Ljdsk__7ABV8w.png)

> That would explain it. The gitlab/gitlab-ce container is built for x86_64 architecture. You will need to run the container on system that has the x86_64 architecture.
>
> gitlab/gitlab-ce 容器是針對x86_64體系結構構建的。 您將需要在具有x86_64體系結構的系統上運行容器

![](/images/medium/1__NHtfr9FINTWyGOSShO5c1Q.png)

**結論，不要使用 Raspberry 來架設 Jenkins**

1. ARM CPU ( 目前 ) 與很多程式並不能搭配，容易遭遇狀況
2. 速度太慢，建置一次要 15 分鐘

會不會想打人 XD
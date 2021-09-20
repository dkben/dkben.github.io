---
layout: default
title: Raspberry Pi 家用 Server 架設
---
## Raspberry Pi 家用 Server 架設

### 超省錢的 Server 架設方法

---

目前是把親朋好友的網站搬回家裡的 Raspberry Pi 中存放，可以省下一些錢。

遇到比較大的麻煩是，因為CPU是ARM架構，所以不是每一個Docker Image都可以用，要花時間找一下並測試。

另外有一篇文章 [Raspberry 4 Jenkins — 樹莓派 4 安裝執行 Jenkins (全記錄)](https://hsu-wen-i.medium.com/raspberry-4-jenkins-%E6%A8%B9%E8%8E%93%E6%B4%BE-4-%E5%AE%89%E8%A3%9D%E5%9F%B7%E8%A1%8C-jenkins-%E5%85%A8%E8%A8%98%E9%8C%84-b4899c7219b3)

---

2020 年8月16日收到

一次買 3 台，其中有一台是同事的

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-01.png){:width="800px"}

有送袋子

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-02.png){:width="800px"}

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-03.png){:width="800px"}

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-04.png){:width="800px"}

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-05.png){:width="800px"}

變壓器比想像中還大，所以只是這樣接，將來可能會換成 USB 電源 HUB，可以節省空間

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-06.png){:width="800px"}

選擇無桌面版本，直接 CLI 介面操作

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-07.png){:width="800px"}

Raspberry Pi 4 的接線方式，螢幕要接在 HDMI-1 的位置

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-08.png){:width="800px"}

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-09.png){:width="800px"}

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-10.png){:width="800px"}

---

## 安裝 Docker

下載安裝腳本

``curl -fsSL https://get.docker.com -o get-docker.sh``

執行安裝腳本

``sudo sh get-docker.sh``

裝好要重啟系統才會正常

![Raspberry Pi4](images/raspberry-pi4-server/raspberry-pi4-server-11.png){:width="800px"}

另外，也在樹莓派上掛載免費的Google Drive 15GB空間 (使用 rclone 工具)
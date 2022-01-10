---
layout: post
title:  "使用 USB 透過 SSH 連線 Raspberry Pi Zero"
subtitle: "按照以下文章重灌設定 Raspberry Pi 後，使用 USB 再透過 SSH 連線，但在 Windows 10 中一直失敗，無法連線"
date:   2021-03-25 09:00:00 +0800
categories: RaspberryPi
meta_description: "使用 USB 透過 SSH 連線 Raspberry Pi Zero，按照以下文章重灌設定 Raspberry Pi 後，使用 USB 再透過 SSH 連線，但在 Windows 10 中一直失敗，無法連線"
tags:
- "RaspberryPi"
---

按照以下文章重灌設定 Raspberry Pi 後，使用 USB 再透過 SSH 連線，但在 Windows 10 中一直失敗，無法連線

> 參考
>
> HEADLESS PI ZERO SSH ACCESS OVER USB (WINDOWS)
>
> [https://desertbot.io/blog/headless-pi-zero-ssh-access-over-usb-windows](https://desertbot.io/blog/headless-pi-zero-ssh-access-over-usb-windows)
>
> 树莓派 zero w raspberrypi ,unable to open connection to raspberrypi.local host does not exist
>
> [https://blog.csdn.net/u013608300/article/details/85225298](https://blog.csdn.net/u013608300/article/details/85225298)
>
> RASPBERRY PI ZERO USB/ETHERNET GADGET TUTORIAL
>
> [https://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/](https://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/)

不過當我用改用 Mac 時，就可以連線

第 1 孔只有電源，第 2 孔才是訊號+電源

![](/images/medium/1__PLaqRj8uQkfLELLiyS1__JQ.png)

可以順利使用 USB 連線了

![](/images/medium/1__Yt__BiI2CvBP4am0qqvudqA.png)

有 usb, wlan 裝置，但此時 wlan 並沒有取得 IP

![](/images/medium/1__arN6ZbGddZr2nyR24i4I0Q.png)

修改網路設定檔

`sudo vi /etc/dhcpcd.conf`

加入以下

```apacheconf
interface wlan0
static ip_address=192.168.1.152/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```

但此時就算重新啟動也無法真的用 Wi-Fi 登入

啟用 Raspberry OS 專用的設定模式

> 參考
>
> [https://www.raspberrypi.org/documentation/configuration/raspi-config.md](https://www.raspberrypi.org/documentation/configuration/raspi-config.md)

`sudo raspi-config`

要啟動 SSH 連線

![](/images/medium/1__vlSZ9w1H1xQnbQ1F27k3ZQ.png)

注意 ⚠️ 記得一定要設定 Wireless LAN 無線網路，這樣才能取得 Wi-Fi 內網 IP

![](/images/medium/1__AM7uhl0MLbWXylOndel68g.png)

在重新啟動前 wlan0 沒有取得 IP

![](/images/medium/1__ReyrCsuVAkXu__evrGVW3OA.png)

重新啟動後，wlan0 有取得 IP 了

![](/images/medium/1__J13QY5k3cU__CKv3JoLhXBA.png)

把 Raspberry Pi 接到別的地方去

![](/images/medium/1__Sc9Hj9XYXPBVFdhqbvU9nA.png)

真的可以透過 Wi-Fi 連線了

![](/images/medium/1__eHuiZ0sI92c__hNkMyVG__Gg.png)

另外，這裡也有提到，直接在 SD 卡中設定，但我沒有實作測試過

> How do I set up networking/WiFi/static IP address on Raspbian/Raspberry Pi OS?
>
> [https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address-on-raspbian-raspberry-pi-os](https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address-on-raspbian-raspberry-pi-os)

---
title: "Configuring wireless network on Gentoo Linux"
description: ""
lead: ""
date: 2022-07-16T21:49:15+09:00
lastmod: 2022-07-16T21:49:15+09:00
draft: false
weight: 50
images: ["gentoo-configure-wireless-network.jpg"]
contributors: []
---

WhaleOS 프로젝트를 진행하면서 ChromiumOS의 기반이 되는 젠투 리눅스를 너무 몰라도 안 되겠다는 생각이 들었다. 젠투 리눅스의 튜토리얼은 OS 설치부터 시작되기 때문에 이를 따라해보며 기초 지식을 쌓으면 좋겠다는 생각이 들었다.

설치 과정은 USB 굽기와 USB 부팅부터 시작하긴 하지만 이 문서는 부팅이 완료된 후 네트워크를 설정하여 인터넷이 가능하도록 만드는 시점부터 시작한다. 보통 유선 네트워크는 이더넷 케이블만 연결해도 바로 동작한다. 하지만 필자는 집에서 노트북에 연결할 어댑터가 없어 무선 네트워크 연결이 필요했다.

여기에서는 무선 네트워크의 다양한 배경지식을 언급하지는 않을 것이다. 또한 젠투 공식 문서를 보면 Open 혹은 WEP 보안 방식에 한해 기본 설치된 iw 명령어를 활용하면 쉽게 연결이 가능하다고 나와 있다. 그러나 요즈음 대부분의 AP는 WPA 보안 방식을 사용하는데, 리눅스 배포판들은 공통적으로 이 방식의 연결을 지원하기 위해 wpa_supplicant를 사용한다.

이제 설명할 내용은 젠투 리눅스의 USB 부팅이 완료된 시점에서 WPA 방식의 무선 네트워크 연결을 위해 wpa_supplicant의 기본 사용법을 설명할 것이다. 젠투 리눅스가 아니더라도 도움이 될 수 있을 것이다.

### Find your wireless interface and access point name

iwconfig 명령을 이용해 연결된 AP 가 없음을 확인한다.

```shellscript
$ iwconfig
lo        no wireless extensions.

enp0s31f6  no wireless extensions.

wlp0s20f3  IEEE 802.11  ESSID:off/any
          Mode:Managed  Access Point: Not-Associated   Tx-Power=-2147483648 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:on
```

iwlist 명령을 이용해 AP를 스캔한다.

```bash
$ iwlist wlp0s20f3 scan | grep ESSID
                    ESSID:"Jarvis"
                    ...
```

### Connect to Wi-Fi network using wpa_supplicant

최근 젠투 minimal installation media 에는 wpa_supplicant 가 이미 설치되어 있다. 그러므로 추가 설치없이 관련 유틸리티를 바로 사용할 수 있다. 아래와 같이 wpa_passphrase 명령으로 wpa_supplicant.conf 파일을 생성한다.

```bash
$ wpa_passphrase "Jarvis" ******** | tee /etc/wpa_supplicant/wpa_supplicant.conf
network={
        ssid="Jarvis"
        #psk="********"
        psk=6b3a55e0261b0304143f805a24924d0c1c44524821305f31d9277843b8a10f4e
}
```

생성된 설정파일을 이용해 AP에 연결해보자. `-B` 옵션을 주면 백그라운드로 실행된다. ESSID를 확인해보면 "Jarvis" 로 잘 연결되어 있으며 ping도 잘 동작함을 볼 수 있다.

```bash
$ wpa_supplicant -B -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlp0s20f3
Successfully initialized wpa_supplicant

$ iwconfig
lo        no wireless extensions.

enp0s31f6  no wireless extensions.

wlp0s20f3  IEEE 802.11  ESSID:"Jarvis"
          Mode:Managed  Frequency:5.745 GHz  Access Point: 88:C3:97:FA:69:AE
          Bit Rate=245 Mb/s   Tx-Power=22 dBm
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:on
          Link Quality=70/70   Signal level=-35 dBm
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excess retries:0  Invalid misc:19   Missed beacon:0
```

젠투 리눅스에서도 무선 네트워크 설정을 어렵지 않게 할 수 있었다. 이제 ssh를 활용해 원격으로 접속해 이후 설치를 진행하는 것도 가능하다.

### References

- [Configuring the network - Gentoo Wiki][1]
- [Using WPA_Supplicant to Connect to WPA2 Wi-fi from Terminal on Ubuntu 16.04 Server][2]

[1]: https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Networking
[2]: https://www.linuxbabe.com/command-line/ubuntu-server-16-04-wifi-wpa-supplicant

---
title: "Kernel Loading"
description: ""
lead: ""
date: 2022-04-02T23:00:41+09:00
lastmod: 2022-04-02T23:00:41+09:00
draft: true
weight: 50
images: ["kernel-loading.jpg"]
contributors: []
---

## Kernel Loading

### GRUB

[GRUB(Grand Unified Bootloader)]()은 GNU 프로젝트의 부트로더이다. GRUB은 일련의 커널 로딩 과정을 간편하게 도와주는 역할을 한다. 다양한 운영체제를 하드 디스크, 플로피 디스크, USB 등 장치의 종류에 구애받지 않고 부팅할 수 있게 해준다. 그래서 특히 운영체제 멀티부팅을 위해서도 널리 사용된다.

이것이 가능한 이유는 GRUB이 운영체제를 로드하기 위한 정해진 규격을 제공하기 때문이다. GRUB은 커널 파일의 유효성을 검증하고 커널을 메모리에 적재한다. 그리고 하드웨어 제반사항을 초기화한 후 초기화 정보를 커널에 넘기면서 커널 엔트리를 호출해 제어권을 커널로 옮겨주는 역할을 한다.

GRUB을 통한 부팅 과정은 아래와 같다.


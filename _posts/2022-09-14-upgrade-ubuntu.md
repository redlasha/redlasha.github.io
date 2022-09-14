---
title: "우분투 리눅스 업그레이드 하기"
date: 2022-09-14
categories: software
---

# 우분투 리눅스 업그레이드 하기

리눅스를 사용하다 보면 업그레이드 하라고 한다. 귀찮지만 업그레이드를 해본다.

## 리눅스 업그레이드 알림

리눅스 접속을 하다보면 `New release 'xx.xx.1 LTS' available.` 이란 문구가 표시될 때가 있다.

```
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.13.0-1025-oracle x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Sep 14 22:03:06 KST 2022

  System load:  0.26               Users logged in:                  0
  Usage of /:   78.7% of 44.97GB   IPv4 address for br-7935db88dad4: ***.18.0.1
  Memory usage: 66%                IPv4 address for docker0:         ***.17.0.1
  Swap usage:   29%                IPv4 address for ens3:            10.0.0.2
  Processes:    145

  => There are 2 zombie processes.

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

135 updates can be installed immediately.
3 of these updates are security updates.
To see these additional updates run: apt list --upgradable

New release '22.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

```

## 리눅스 업그레이드 명령들

나중에 보기 위해 명령어만 메모한다.

```
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade

sudo apt autoremove

sudo apt install update-manager-core
sudo do-release-upgrade

```

## 리눅스 재시작
```
sudo reboot
```


참고: [https://jjudrgn.tistory.com/6](https://jjudrgn.tistory.com/6)  
참고: [https://gentlesark.tistory.com/80](https://gentlesark.tistory.com/80)    
참고: [https://jhnyang.tistory.com/50](https://jhnyang.tistory.com/50)  

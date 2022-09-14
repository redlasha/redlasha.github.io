---
title: "우분투 리눅스 방화벽 설정"
date: 2022-09-14
categories: software
---

## 리눅스 방화벽 설정하기

나중에 다시 방화벽 설정하기 위해서 남겨 놓음

### iptables에 80 port 오픈하기

```
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### iptables 저장하기

그러나 저장되지 않았음. ufw로 설정되어 있기 때문인듯
iptables 설정으로 80 port는 오픈되는데...

```
~$ sudo service iptables save 
iptables: unrecognized service

```


## ufw

iptables 대신 기본으로 ufw가 설정되어 있음.
기억이 나지 않는다면 참고 블로그를 읽어보자

참고: [https://dokeeo.tistory.com/40](https://dokeeo.tistory.com/40)
참고: [https://lindarex.github.io/ubuntu/ubuntu-ufw-setting/](https://lindarex.github.io/ubuntu/ubuntu-ufw-setting/)

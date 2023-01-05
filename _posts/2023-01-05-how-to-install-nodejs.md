---
title: node 설치하기 with nvm
date: 2023-01-05
categories: software
keywords:
  - node
  - nvm
  - npm
---
# node 설치하기

한동안 node를 사용하지 않다가 github 프로젝트를 내려받으면 node를 업데이트 해줘야 하는 경우가 종종 있다.  
이참에 nvm을 설치해서 node 재설치 하지 않도록 한다.  

## nvm
   
 nvm: node version manager   
   
 [nvm windows github](https://github.com/coreybutler/nvm-windows)  
 
 release 페이지에서 설치파일을 다운로드 한다.   
 2023-01-05 기준 1.1.10 버전이 최신이다.  
 https://github.com/coreybutler/nvm-windows/releases  
   
 `nvm-setup.exe` 를 다운로드하고 실행한다.    
   
 중간에 설치 경로를 설정하는데, nvm 설치 경로를 설정하고   
 그 다음엔 node의 경로를 설정(심볼릭 링크)까지 해서 설치 완료된다.  
 
 nvm으로 node 버전을 변경해서 사용할 수 있다.  
 
## node 설치

### 다운로드 가능한 버전 조회
```
> nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    19.3.0    |   18.12.1    |   0.12.18    |   0.11.16    |
|    19.2.0    |   18.12.0    |   0.12.17    |   0.11.15    |
...
```  
  
### node 버전 선택해서 install (아직은 사용할 수 없다)  
```
nvm install v18.12.1
```

### 다운로드한 node를 사용하기 위한 명령어  
```
nvm use 18.12.1
```
 
여기까지 하면 node & npm 을 사용할 수 있게 된다.
 
 
 
 

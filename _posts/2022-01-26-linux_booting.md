---
title: 리눅스 부팅 설정
author: SeungEun Baek
date: 2022-01-26
category: Study
layout: post
---
   
## Boot Script 등록
```
1. /etc/inid.d 에 script 파일 등록
2. /etc/rc.d/rc{n}.d 심볼릭링크 추가 
3. 추가된 심볼릭링크 확인 명령어 : find /etc/rc.d |egrep "name"
```
   
## Run Level

리눅스는 부팅이 되면서 Run Level에 맞는 작업을 수행하며, 각 Level의 작업은 아래와 같이 분리된다.<br>
현재 시스템이 사용중인 기본 Target을 확인하는 명령어 : systemctl get-default

> ex) 서버가 Level5로 부팅 된다면,  rc5.d에 등록된 심볼릭링크에 해당하는 서비스가 자동 실행된다

    
| <center>File</center> | Target Unit<br>(Run Level) | Target Unit<br>(Systemd Target) | 설명 |
|:--------|:--------|:--------|:--------|
| /etc/rc.d/rc0.d | runlevel0.target | poweroff.target | 시스템종료를 의미<br>init 0을 입력하여 런레벨을 0으로 변경하여 리눅스 시스템을 종료 |
| /etc/rc.d/rc1.d | runlevel1.target | rescue.target | single-user 모드<br>시스템 복구 모드를 위해 사용(일명 안전모드)<br>단일 사용자 모드로서 관리자 쉘을 얻게 된다 |
| /etc/rc.d/rc2.d | runlevel2.target | multi-user.target<br>(without NFS) | NFS(Network File System)을 사용하지 않는 multi-user모드<br>네트워크를 사용하지 않는 상태의 텍스트 유저 모드를 뜻하지만 CentOS7부터는 사용되지 않는 레벨<br>호환성을 위해 런레벨3과 동일한 것으로 취급 |
| /etc/rc.d/rc3.d | runlevel3.target | (full) multi-user.target | 텍스트 모드의 multi-user모드<br>일반적인 쉘 스크립트 기반의 인터페이스를 작동<br>일반적으로 텍스트 유저 모드라고 부른다 |
| /etc/rc.d/rc4.d | runlevel4.target | multi-user.target<br>(unused) | 기본적으로 사용하지 않는 모드<br>런레벨2와 호환성을 위해 런레벨3과 같은 취급<br>런레벨3을 임의의 정의로 설정하여 별도로 사용 가능 |
| /etc/rc.d/rc5.d | runlevel5.target | graphical.target | 그래픽 보드의 다중 사용자 모드<br>기본적으로 런레벨3과 동일하면서 그래픽이 첨가됨<br>GUI를 제공하는 그래픽 유저 모드 |
| /etc/rc.d/rc6.d | runlevel6.target | reboot.target | 시스템 재부팅을 나타내는 모드<br>init 6을 입력하여 런레벨을 6으로 변경하면 시스템 재부팅<br>*런레벨 6을 init default로 설정한다면 시스템은 무한 재부팅 상태가 되기 때문에 주의해야한다* |
| /etc/rc.d/rc.local | | | 모든 부팅작업이 완료된 다음 마지막에 수행된다 |


## 심볼릭링크(ex:S50contents) 파일의 제일 앞 알파벳 의미 
```
S : 부팅할 때 시작(Start) 되는 Script File<br>
K : 부팅할 때 시작되지 않는(Kill) Script File<br>
숫자 : 실행되는 우선순위

보통 Run Level  3,4,5 : Start / 0,1,2,6 : Kill
```

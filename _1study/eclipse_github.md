---
title: Eclipse Github 연동
author: SeungEun Baek
date: 2022-02-03 23:18 
category: bse
layout: post
sitemap :
  changefreq : daily
  priority : 1.0
---
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fdev-seungeun.github.io%2F1study%2Feclipse_github%2F&count_bg=%23FEC8E6&title_bg=%23B2ADAD&icon=&icon_color=%23515050&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)            
## 1. Github상에서 생성한 Repository 연동 후, Project Commit,Push 하기
> 1. Git 환경 설정
    Window > Perspective > Open Perspective > Others..<br>   
   ![image](https://user-images.githubusercontent.com/80504390/152358573-0dbc6f50-56db-4258-8158-c09aff0758b2.png)
> 2. 여기서 Git 아이콘 선택 후 Open
> 3. Git Repository 창에서 Clone a Git Repository 선택
> 4. ex) uri : https://github.com/dev-seungeun/java_projects.git
> 5. branch, master 등 선택하여 clone
> 6. 'Git Respository' 창에서 가져올 프로젝트 선택하여 import

<br><br>

## 2. 갑자기 Github 연동 로그인 실패할 경우
    "can't connect to any repository(깃헙 주소):not authorized"

### 원인
> Github에서 아이디, 패스워드 인증을 없앰(2021.08.13) > ID/Personal Access Token 으로 인증방식이 바뀌었다

<br>

### 해결방법 - 토큰생성

> 1. Github 홈페이지 > 프로필 클릭 > Setting 메뉴 선택<br>   
         <img src="https://user-images.githubusercontent.com/80504390/152360015-9a0a658f-eab8-4d1e-9393-1e56e5e3153e.png">
> 2. Developer Settings 메뉴 (아래쪽에 위치함)<br>   
         <img src="https://user-images.githubusercontent.com/80504390/152360123-52833259-9b76-471b-a391-497c10c90cc0.png">
> 3. Personal access tokens > Generate new Token<br>   
         <img src="https://user-images.githubusercontent.com/80504390/152360230-5cb453fd-8598-4354-a669-5667118ff1d1.png">
> 4. 발급된 토큰 복사하여 노션같은 자신의 메모장에 저장해놓기*
> 5. Git Repository > [my project] > Remotes > origin > 우클릭 > Change Credentials
> 6. User : Git ID Email, Password : 발급받은 Token
> 7. Save!!
> 8. 성공 :)

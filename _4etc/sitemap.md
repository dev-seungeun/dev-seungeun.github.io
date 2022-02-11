---
title: 구글 검색 콘솔 - sitemap 등록 오류
author: SeungEun Baek
date: 2022-02-11 12:10 
description: Google Search Console 에서 sitemap.xml이 등록되지 않는 오류 해결
category: bse
layout: post
---
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fdev-seungeun.github.io%2F4etc%2Fsitemap%2F&count_bg=%23FEC8E6&title_bg=%23B2ADAD&icon=&icon_color=%23515050&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

<br> 
 <div style="font-size:15pt; color:orange; margin-bottom:12px"><b>포스팅 이유</b></div> 
 <div style="font-size:12pt; color:black">
  <b>
   &nbsp;&nbsp;&nbsp; 내 깃블로그를 구글 검색에 노출시키고 싶어서<br>
   &nbsp;&nbsp;&nbsp; Google Search Console에 내 사이트를 등록하려 하니 sitemap.xml 등록이 잘 되지않았다..
  </b>
 </div>
  

<br>   

## Google Search Console - <https://search.google.com/search-console/about>

SEO(Search Engine Optimization)관련 용어중 하나인 서치 콘솔.   
 
```
구글 검색 콘솔(Google Search Console)은 구글의 전 구글 웹마스터들이 개발한 웹 서비스로서, 
웹마스터들이 자신들의 웹사이트의 보이는 부분을 최적화하고 인덱싱 상태를 검사할 수 있게 해준다.

2015년 5월 20일까지 이 서비스의 이름은 구글 웹마스터 툴스(Google Webmaster Tools)였다.
2018년 1월, 구글은 새 버전의 검색 콘솔을 선보였으며 사용자 인터페이스의 변경이 포함되었다. 
2019년 9월, 홈과 대시보드 페이지를 포함한 오래된 검색 콘솔 보고서가 제거되었다.
```
출처 - [구글 검색 콘솔(위키백과)](https://ko.wikipedia.org/wiki/%EA%B5%AC%EA%B8%80_%EA%B2%80%EC%83%89_%EC%BD%98%EC%86%94)

<br><br>
우선, 아래화면처럼 시작하기 누르고 도메인 제출 후 차근차근히 진행하면 콘솔 시작 화면을 만날 수 있다.   
<br><br> 
<img style="width:800px" src="https://user-images.githubusercontent.com/80504390/153554327-292f35fc-b393-4a0e-a41b-49f1e956eab3.png" /><br>
<img style="width:500px" src="https://user-images.githubusercontent.com/80504390/153551882-db5fe834-167b-4c29-9b12-1ac2ee245c29.png" /> 


<br><br>   

## Sitemap 등록 오류!

Google Search Console 의 왼쪽 메뉴에서 sitemap.xml을 등록하는데 자꾸오류가 떨어졌다..<br>

<img style="width:800px" src="https://user-images.githubusercontent.com/80504390/153552031-f9b09d20-bb86-4a0c-89e5-53e580f1cc40.png" /><br><br>   

오류 상세내용을 보니 url 태그 누락이라고 하는데 누락되지 않았는데..ㅜㅜ<br>

<img style="width:800px" src="https://user-images.githubusercontent.com/80504390/153552080-d8a2c28e-4d90-4b73-b993-7b7a1b1101db.png" /><br><br>    

이렇게 내 사이트맵은 정상적으로 필수태그를 모두 가지고있다!!<br>

<img style="width:800px" src="https://user-images.githubusercontent.com/80504390/153552148-478ac8c5-f88c-47e2-9e5a-3d820b66bf5f.png" />
<br><br>    

몇날며칠 검색해보고 많은사람들의 솔루션을 적용해 봐도 되지않았다..  
어떤사람들은 2~3일 지나면 성공으로 바뀌어있었다고 해서 기다려도 봤지만 난 며칠을 기다려봐도 적용이 안된다!   
구글에 문의글도 남겼는데 별 도움되는 답변은 듣지 못했다.
 
<br><br>

## 해결 방법 (우회방법?)
```
sitemap을 통째로 등록 할 수 없다면 내 포스팅 url을 하나하나 등록해주기로 결정!!   
조금 번거롭지만 뭐 매일매일 글 쓰는것두 아니구   
포스트 작성 완료할 때 마다 콘솔들어가서 등록해주면되니까 그렇게 귀찮은 작업은 아닐것같다.   
```
<br>
1. Console 좌측 메뉴 중 'URL 검사' 클릭!
2. 그러면 화면이 살짝 어둡게 변하면서 위의 검색 창만 밝게 표시된다.
3. 검색창에 내 포스팅 url을 하나하나 넣고 엔터 치면 분석하는 작업이 진행 되고(로딩표시) 완료 되면    
4. 아래 화면처럼 Url 검사 결과페이지가 뜬다. 여기서 노란색 표시해둔 '색인 생성 요청'을 선택하면<br>

<img style="width:800px" src="https://user-images.githubusercontent.com/80504390/153553296-70f18c10-039e-42f8-9744-4c8bfcb0e144.png" /><br><br>  

5. 이렇게 검사가 또 진행되구 완료되면!<br>

![image](https://user-images.githubusercontent.com/80504390/153553392-5e6544b9-dd6e-4269-8758-8344c2cb5070.png)<br><br>

6. 확인 선택하면 끝!! 2,3일 뒤에 검색에 노출된다고 한다~!<br>

![image](https://user-images.githubusercontent.com/80504390/153553448-01aa7d74-7dca-4a7c-bd41-ba5a03b2d439.png)<br><br>


이제 기다려본다!! 😒

<br><br><br>







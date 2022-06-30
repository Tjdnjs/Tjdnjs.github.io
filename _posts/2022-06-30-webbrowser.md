---
layout : single
title : "STAGE2. Background - web (3)"
categories: Security
tag: [dreamhack, webhacking]
---
## 드림핵 웹 기본상식 - Background : Web Brower
<img src = "/images/webbackground/6.jpg">

### Domain Name
<img src = "/images/webbackground/7.jpg">


#### nslookup(name server lookup) 실습
1. nslookup www.naver.com <br><br>
<img src = "/images/webbackground/2.png"><br><br>

>>|code|설명|
|:------|:---|
|Server: 168.126.63.1|한국통신(KT)의 IP 주소|
|Address: 168.126.63.1#53|53 : DNS 표준 포트 번호|
|canonical name = www.naver.com.nheos.com|요청을 처리하려면, 클라이언트의 추가 동작이 필요함|
|Address: 223.130.200.104|www.naver.com의 IP 주소|
|Address: 223.130.195.95|www.naver.com의 IP 주소|


2. nslookup naver.com <br><br>
<img src = "/images/webbackground/3.png">
<img src = "/images/webbackground/1.png">


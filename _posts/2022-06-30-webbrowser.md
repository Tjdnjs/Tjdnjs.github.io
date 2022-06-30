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
**1. nslookup www.naver.com** <br><br>
<img src = "/images/webbackground/2.png"><br><br>

|code|설명|
|:------|:---|
|Server: 168.126.63.1|한국통신(KT)의 IP 주소|
|Address: 168.126.63.1#53|53 : DNS 표준 포트 번호|
|canonical name = www.naver.com.nheos.com|CNAME 이라고도 하며, 실제 호스트와 연결되는 별칭|
|Address: 223.130.200.104|www.naver.com의 IP 주소|
|Address: 223.130.195.95|www.naver.com의 IP 주소|

<br>
**2. nslookup naver.com** <br><br>
<img src = "/images/webbackground/3.png">
<br><br>
위의 www.naver.com을 불렀을 때와 형태가 같으나, IP주소 4개가 뜨는 것을 볼 수 있다.


<img src = "/images/webbackground/1.png">


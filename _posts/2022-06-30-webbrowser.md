---
layout : single
title : "[Dreamhack] STAGE2. Background - web (3)"
categories: web-hacking
tag: [dreamhack, webhacking]
sidebar:
    nav: "docs"
---
## Web Hacking - Background : Web Brower
<img src = "/images/webbackground/6.jpg">

### Domain Name
<img src = "/images/webbackground/7.jpg">


#### nslookup(name server lookup) 실습

**1. nslookup naver.com** <br><br>
<img src = "/images/webbackground/3.png">
<br><br>

**2. nslookup www.naver.com** <br><br>
<img src = "/images/webbackground/2.png"><br><br>

|code|설명|
|:------|:---|
|Server: 168.126.63.1|한국통신(KT)의 IP 주소|
|Address: 168.126.63.1#53|53 : DNS 표준 포트 번호|
|canonical name = www.naver.com.nheos.com|CNAME 이라고도 하며, 실제 호스트와 연결되는 별칭|
|Address: 223.130.200.104|www.naver.com의 IP 주소|
|Address: 223.130.195.95|www.naver.com의 IP 주소|

<br>
이 때, 위의 nslookup naver.com과 형태가 비슷하지만, IP 주소가 2개만 뜨는 것을 볼 수 있다. 이는 www. 로 시작하는 도메인의 경우, 웹서버에 해당하는 내용만 응답하기 때문이다.

**3. wireshark를 통한 패킷 분석** <br><br>

<img src = "/images/webbackground/4.png">
<br><br>
네이버 접속 시 교환되는 패킷을 wire shark로 캡쳐한 후, KT의 IP 주소 168.126.63.1을 필터링하여 확인한 결과 DNS 프로토콜만이 오고가는 것을 볼 수 있었다. 이에 따라, 이동통신사의 DNS 서버에서 IP 정보를 제공한다는 것을 알 수 있다
<br><br>
<img src = "/images/webbackground/1.png">
<br><br>
위 패킷 중 하나를 열어 확인해본 결과, UDP헤더의 source port가 DNS의 표준 포트인 53번 포트인 것을 볼 수 있었다. 이때 destination address는 10.0.2.15인데, 이는 칼리 리눅스 구동을 위해 사용한 가상머신의 IP 주소로, 이동통신사의 DNS 서버가 가상머신에게 요청한 도메인의 IP 주소를 제공해주었다는 것을 알 수 있다
<br><br>

<img src = "/images/webbackground/5.png">
<br><br>
nslookup을 통해 알아낸 네이버의 IP 주소 중 패킷에 캡쳐된 주소가 223.130.200.104였기에, 이를 필터링하여 확인해보았다. TCP와 TLSv1.3 프로토콜을 통한 통신이 일어나는 것으로 보아, 가상머신과 네이버 간 통신이 암호화되어 전송되는 것을 알 수 있다

### 웹 렌더링
<img src = "/images/webbackground/8.jpg">
<br><br>
**파싱** : 프로그램에서 나타나는 문자열을 해당 언어의 문법에 맞게 구문 분석 트리로 구성하는 일

---
layout: post
related_posts:
  - /ai
title:  "인터넷과 웹서비스"
date:   2025-10-10
categories:
  - ai
description: >
  
---
* toc
{:toc .large-only}

# 네트워크란?
![network](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2Fb0w6k9%2FbtsMYKQx9t0%2FAAAAAAAAAAAAAAAAAAAAALqGrXYk06EbR4BM8S9KRivy6FHUTearBy7a19p3cyNq%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DRx3JwkTla86MBSnbAX1t7Ec%252BGgA%253D)
여러 대의 컴퓨터와 장치들이 서로 데이터를 주고받을 수 있도록 연결된 구조이다. 

### IP주소
IP주소(Internet Protocol Address)는 인터넷이나 네트워크에 연결된 각 장치를 구분하기 위해 부여되는 고유한 번호이다.

### IPv4
IPv4 주소는 숫자 4개가 점(.)으로 구분된 형태이다. 각 숫자는 0부터 255까지 가능하며 총 4개 = 8비트 * 4 = 32비트로 구성되어 있다.
즉, IPv4는 32비트 주소 체계이며, 이론 상 약 43억 개 ($$2^32$$)의 주소를 만들 수 있다.
```
192.168.0.1
```

### IP주소 클래스(Class A~E)
과거에는 IP주소를 클래스 체계로 나누었지만, 요즘은 CIDR(사이더) 방식으로 더 유연하게 네트워크를 나눈다. 그러나 클래스 개념은 여전히 이해를 돕는데 유용하다.
![IPAddressClass](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FpS1d5%2FbtsMZXVCBzG%2FAAAAAAAAAAAAAAAAAAAAABf5ZVh_ivmfZIIpXLuq9Re57GErzTOoQm_TiuutosbM%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DNyzhS4PpEmVmzb2nK%252B9jbMjW66I%253D)

### 사설 IP주소 (Private IP)
인터넷에 직접 연결되지 않고 내부 네트워크(예: 가정, 사무실)에서 사용하는 주소이다. 이 주소들은 공유기나 사무실 LAN에서 사용되고, 외부에서는 직접 접근할 수 없다.
![PrivateIP](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbAMdou%2FbtsMX00NiBd%2FAAAAAAAAAAAAAAAAAAAAAFaYe2elxtM8io7NFXcHixVa1vA-eNRKICdRLAYmrKwP%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DV%252F3ygbQB6ENDK2zhHq4PQdgnaeI%253D)

### 특수한 IP 주소
![SpecialIP](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdna%2FbC5eSw%2FbtsMZ3g89LZ%2FAAAAAAAAAAAAAAAAAAAAAIS7zgWQJZYbuMO3UtIXSJqlqB78OixulU6PlEQ4OAAP%2Fimg.png%3Fcredential%3DyqXZFxpELC7KVnFOS48ylbz2pIh7yKj8%26expires%3D1761922799%26allow_ip%3D%26allow_referer%3D%26signature%3DX6BqDenYWpoVrwBSUnBFwI0IrIA%253D)

### IPv6
초기에 만들어질 때는 IPv4로 충분하다고 생각했지만, 기기 수가 많아 주소가 부족해졌고 이를 해결하기 위해 나온 게 IPv6이다. IPv6은 16비트 * 8블록 = 총 128비트 주소이다. 숫자와 영문자가 16진수로 표현되며, 콜론(:) 으로 구분된다. 너무 길기 때문에 0은 생략할 수 있다.

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### 비트와 바이트
비트(bit)는 컴퓨터가 이해할 수 있는 가장 작은 정보단위이다. 0과 1 두 가지 값만 표현할 수 있다.
* 1Byte = 8bit
* 8개의 0과 1이 모이면 1바이트가 된다.

### IP주소에서 비트
* IPv4    
8비트씩 4 부분으로 나누어서 10진수로 변환한 것
```
11000000.10101000.00000001.00000001 → 192.168.1.1
```
* IPv6    
16비트씩 8덩어리로 나누고, 16진수로 바꿔서 표현
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

# 인터넷이란?
전 세계의 컴퓨터와 스마트폰, 서버 같은 장치들이 서로 연결되어 정보를 주고받을 수 있게 해주는 거대한 네트워크이다.

### 프로토콜
프로토콜(protocol)은 컴퓨터나 네트워크 장치들이 서로 통신할 때 지켜야할 약속이나 규칙이다. 예를 들어, 우리가 웹사이트를 볼 때 사용하는 HTTP, 파일을 전송할 때 사용하는 FTP, 이메일을 주고받는 SMTP 같은 것들이 모두 프로토콜이다.

### HTTP (HyperText Transfer Protocol)
크롬이나 사파리 같은 브라우저로 웹사이트에 접속할 때, 그 웹페이지의 글, 사진, 영상 등을 서버에서 가져오는 방식이 바로 HTTP이다.

### FTP (File Transfer Protocol)
FTP는 말 그대로 파일을 주고받기 위한 규칙으로, 서버와 컴퓨터 사이에서 파일을 업로드하거나 다운로드할 때 사용한다.

### SMTP (Simple Mail Transfer Protocol)
이메일을 작성해서 보내면, 그 이메일을 상대방의 메일 서버로 전달해주는 역할을 하는 것이 바로 SMTP이다.

# 웹이란?
웹(Web)은 우리가 인터넷을 통해 정보를 보고, 읽고, 상호작용할 수 있게 해주는 서비스이다. 흔히 사용하는 웹사이트, 블로그, 유튜브, 쇼핑몰 등은 모두 웹의 한 형태이다. 웹은 HTML로 만들어진 문서(웹 페이지)를 브라우저(크롬, 사파리 등)가 읽어서 화면에 보여주는 방식으로 작동한다. 이 웹은 인터넷 위에서 돌아가는 서비스 중 하나로, 우리가 주소창에 URL을 입력하면 HTTP 같은 프로토콜을 통해 서버에 요청하고, 그 결과를 받아와서 보여주는 구조이다.

### 웹의 작동방식
1. 사용자가 웹 브라우저에 주소(URL)를 입력한다.
```
https://www.example.com
```
2. DNS 서버가 주소를 IP주소로 바꿔준다.
   * 컴퓨터는 사람이 쓰는 도메인(예: www.example.com)을 이해하지 못함.
   * rmfotj DNS(Domain Name System)가 나서서 이 주소를 IP 주소로 바꾼다.
3. 브라우저가 서버에 요청 (HTTP 요청)
  * 브라우저는 해당 IP 주소를 가진 서버에게 "웹 페이지 보여줘!" 라는 메시지를 보낸다.
  * 이 메시지는 HTTP 요청(Request) 이라고 부르고, 요청은 웹서버에 도착한다.
4. 서버가 요청을 받고, 웹 페이지 파일을 응답 (HTTP 응답)
  * 서버는 요청을 받으면, 내부에 저장된 HTML, CSS, JavaScript 등의 파일을 찾아서 브라우저에게 보낸다.
  * 이걸 HTTP 응답(Response) 이라고 한다.
5. 브라우저가 파일을 해석해서 화면에 보여준다.
  * 브라우저는 서버로부터 받은 HTML, CSS, JS 파일을 읽고 해석해서 웹페이지처럼 화면에 그려준다.
  * 사용자는 웹사이트를 눈으로 보고, 클릭하고, 입력할 수 있는 상태가 된다.

### DNS (Domain Name System)
DNS는 우리가 웹사이트 주소를 입력할 때 사용하는 도메인 이름(예: www.example.com)을 컴퓨터가 이해할 수 있는 IP 주소(예: 93.184.216.34)로 바꿔주는 시스템이다.

### Server 와 Client
클라이언트는 정보를 요청하는 쪽, 예를 들어 우리가 사용하는 웹 브라우저나 스마트폰 앱이 이에 해당하고, 서버는 그 요청을 받아 정보나 서비스를 제공하는 쪽, 즉 웹사이트의 본체나 데이터가 저장된 컴퓨터이다. 브라우저에 www.example.com을 입력하면, 클라이언트인 내 컴퓨터가 서버에게 웹페이지를 요청하고, 서버는 그에 대한 내용을 응답해주는 식이다.

### 최초의 웹 사이트
최초의 웹사이트는 1991년, 스위스에 있는 CERN(유럽 입자 물리 연구소)에서 팀 버너스리(Tim Berners-Lee)라는 과학자에 의해 만들어졌다. 이 웹사이트는 "월드 와이드 웹(WWW)이란 무엇인가?"를 설명하는 사이트로 웹 자체에 대한 소개서 같은 역할을 했다.
```txt
개설 시기: 1991년 8월 6일
주소(URL): http://info.cern.ch/
```

# 웹 서비스란?
인터넷을 통해 클라이언트(사용자)와 서버 간에 데이터를 주고받으며 기능이나 정보를 제공하는 소프트웨어 시스템이다.
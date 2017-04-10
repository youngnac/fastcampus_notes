17/01/23 

# 네트워크
>- types of computer network: LAN MAN WAN
>- network architecture: client/server(자료를 주로 받는지, 보내는지), peer to peer(정보의 받는 주체와 주는 주체가 구분되어 있지 않고 서로 주고 받고)
>- network topologies: star, ring, bus (네트워클 구성할 때, 어떻게 구성되어 있는지...)
>- network communications topology: internet extranet intranet

>- LAN: local area network 근거리 통신망 (한 건물, 층)
>- MAN: metropolitan area network 도시권 통신망 (시, 도별)
>- WAN: wide area network 광역 통신말 (국가, 대륙)

>- Server
>- Client

>- Internet: inter - network 컴퓨터로 연결하여 TCP/IP 프로토콜을 이용해 정보를 주고받는 컴퓨터 네트워크, 네트워크의 네트워크

### Protocol 

>	- TCP/IP:
>		- TCP: transmisison control protocol 전송제어규약 어떤 정보를 주고 받는 것을 제어 하는 규칙
>		- IP: internet protocol 인터넷 규약, 인터넷을 통해 주고 받을 때의 규칙
>	- 즉: 인터넷상으로 정보를 주고 받는 것을 제어하는 규약.

- WWW: world wide web, 문서(웹페이지, html ...)들이 있는 정보의 저장소, 분산과 연결
- url: uniform resoure locator (URI(identity)의 하나)
 - [protocol]://host:Port/Path
 - ex) file://localhost/movie/sdg.mp4
 - ex) ftp://id:pw....

## Protocol: 통신규약
- 장비 사이에서 메시지를 주고 받는 양식과 규칙의 체계. 즉, 통신(네트워크)할 떄 정해진 메세지 규칙 
- http - hyper text 전송하기 위한 규약
- https, sftp, telnet, ssh, ssl, smtp...
- ftp: file transfer portocol 파일전송 통신 규약
- telent: TErminal NETwork 원격 로그인을 위한 프로토콜
- SSH: secure shell 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 그 프로토콜 telnet의 대용 목적으로 설계
 - SSL: Secure Socket Layer웹서버와 브라우저 사이의 보안을 위한 프로토콜. 클라이언트의 프로세스와 웹서버 사이의 전송이 암호화 될 것임.
- SMTP: simple mail transfer protocol 전자메일 발송 프로토콜
- POP/IMAP: 이멜을 가져오고 싶다, pop는 다운로드하여 백업/가져오기만하는 비동기화 방식. imap은 서버에 두고 sync(동기화)하는 방식.
- IP address: internet protocol address 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신을 하기 위해서는 사용하는 번호
- Domain address: 네트워크상에서의 컴퓨터를 식별하는 호스트 이름
- DNS: domain name system. 호스트의 도메인 이름을 호스트의 네트워크 주소로 바꾸거나 그 반대의 변환을 수행. 이를 통해 도메인과 IP 매칭
   * 캐쉬에 담아두고 빨리 찾아간다. (서버와 나랑 쿠키는 세션정보를 담는것.)
- MAC address: media access control address 네트워크 어댑터에 부착된 식별자/물리적 주소. 보통 바꿀 수 없고. 고유하다. 
- port 번호: www.daum.net:8080 <- :#### 가상의 논리적 통신 연결단 , 번호로 구분. 신호가 컴퓨터로 들어왔을 때 어디 입구로 들어오는지 지정, 목적에 따라 나뉘어짐 (protocol마다) 
   [port wiki link:](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)

### HTTP
- GET: 어떤 웹을 단순히 보내는 것, 요청사항들을 url에 다 포함해서...
- POST: 조건의 맞는 정보 제공, 코드로 작성, url에 나오지 않음
 - 왜 get과 post가 나누어져 있나?
  - post는 민감한 정보를 교환할 때...
  - get으로 보내는 정보의 양이 한정적이다
  - get은 캐싱된 데이터를 보낼 수 있다. 따라서 빠르게 정보를 받을 수 있다. 그러나 실시간 데이터를 볼지 못 할 수도 있다. 
  - 반면 post는 캐싱된 데이터가 아닌 것들을 받기에 최신, 실시간의 데이터들을 받아 볼 수 있다. 
- PUT: 
- DELETE: 삭제요청
- HEAD:


# OSI 7계층 모형:
open system interconnection reference model


# 암호화
## 대칭키 암호화
>암호화화 복호화에 같은 키를 쓰는 알고리즘 ex) SEED, DES, AES

## 공개키(비대칭키) 암호화
>공개키로 암호화된 데이터를 비밀키로 사용하여 복호화할 수 있는 암호화 알고리즘. 이를 사용하기 위해서는 중재기관의 인증이 필수이다
>ex) RSA etc...	

> 우리나라의 공인인증서는 인증서 자체가 비밀키이고 이에 대한 비밀번호가 대칭키 SEED로 되어있다. 따라서 공인인증서(비밀키)의 보안이 굉장히 중요한데, 쉽게 복사가 가능하고 보관파일의 형태도 보안성이 낮아 매우 보안상 위험하다. 

### 암호화 해시 함수
> 임이의 data(암호 etc) 등을 고정된 길이의 데이터로 매핑하여 원래의 입력값과의 관계를 찾기 어렵게 만든것. 하지만 사실상 다 넣어보다보면 복호화 가능성이 높다. ex) SHA, MD5



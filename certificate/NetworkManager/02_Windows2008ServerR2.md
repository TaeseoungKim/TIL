본 게시글은 네트워크 관리사 2급을 준비하며 공부했던 내용을 순서대로 작성한 글 입니다.

## Windows 2008 Server R2

### 출제 유형

1. TCP / IP Setting
2. DNS Setting
3. DHCP Setting
4. IIS FTPServer Setting
5. IIS WebServer Setting
6. 계정추가
7. SMTP Setting
8. 로컬보안정책 Setting
9. Service Setting

### TCP / IP Setting

1. 네트워크 환경을 <아래>와 같이 설정하시오.

- IP Address: 10101100.00010000.10010110.01110011
- Subnet Mask: 22bit
- Default Gateway: 192.168.100.254
- DNS Server: 200.100.100.200
- 추가 Gateway: 192.168.100.253
- 보조 DNS Server: 201.100.100.201

Subnet Mask의 22bit라는 의미는 **앞에서부터 1이 22개**라는 의미이다.

풀이시작 -> Internet Protocol Version 4 (TCP/IP v4) 선택 후 속성 클릭(in 로컬 영역 연결 속성) -> 문제 보기와 같이 설정 (in Internet Protocol Version 4 (TCP/IP v4) 속성) -> 추가 게이트웨이 설정을 위해 고급 탭 클릭 -> 추가 -> 기본 게이트웨이 추가

### DNS Setting

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

1. <아래>의 설정 값을 참고하여 DNS 서버를 설정하시오.
   @ IN SOA ns.icqa.or.kr admin.icqa.or.kr (
   10 ; Serial
   15분 ; Refresh
   10분 ; Retry
   1일 ; Expire
   1시간 ); Minimum

**www IN A 192.168.100.20**
**ftp IN CNAME www**

풀이시작 -> 정방향 조회 영역 오른쪽 클릭, 새 영역 클릭 (in DNS 관리자) -> 다음, 다음(in 새 영역 마법사) -> 영역 이름에 icqa.or.kr 입력 후 다음 -> 다음 x3, 마침

(여기까지 하면 정방향 조회영역 앞에 +가 활성화 되고, icqa.or.kr 폴더가 생성되어 있을 것)
icqa.or.kr 폴더 우 클릭, 속성 클릭 -> SOA(권한 시작)탭에서 보기대로 ns.icqa.or.kr(주 서버) admin.icqa.or.kr(책임자)를 포함하여 입력 후 확인

남은 설정은 두 가지이다.
**1. www IN A 192.168.100.20**
**2. ftp IN CNAME www**

위 내용은 www라는 이름을 가지고 192.168.100.20의 IP를 배정받는 새 호스트를 만들어라.라는 뜻이다.

icqa.or.kr 폴더 우 클릭, 속성에서 새 호스트(A 또는 AAA) 클릭 후 -> 이름(www)과 IP 주소(192.168.100.20) 입력 후 호스트 추가

(1)번 끝,

icqa.or.kr 폴더 우 클릭, 속성에서 새 별칭 클릭 -> 별칭 이름(ftp), 대상 호스트의 FQDN(www)입력

(2)번 끝

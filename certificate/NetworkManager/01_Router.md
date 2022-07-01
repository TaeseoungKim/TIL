본 게시글은 네트워크 관리사 2급을 준비하며 공부했던 내용을 순서대로 작성한 글 입니다.

## Router(라우터)

### 명령어 정리

interface serial 5 - 인터페이스 시리얼 5를 위한 환경설정모드로 전환시킴. show 명령과 함께 사용되기도 한다.
ip address - 인터페이스상에 ip 어드레스를 설정한다.
line - 사용자모드 패스워드를 설정하거나 혹은 바꾸기 위한 환경설정모드로 전환시켜준다.
line console 0 - 콘솔 환경설정모드로 전환시켜준다.
line vty - VTY(Telnet) 인터페이스 환경설정모드로 이동시킨다.
no shutdown - 인터페이스를 작동시킨다.

### Router 출제 유형 순위

1. IP / SubnetMask 설정
2. 대역폭 설정
3. Description 설정, Secondary 설정, Default Gateway 설정, Telnet Password 설정, 인터페이스 확인 문제
4. 텔넷 시간초과시 연결종료, Console Password 설정, 활성화 시키기, 접속하고 있는 사용자 확인 문제, 라우팅 확인 문제
5. Hostname, Pass 설정 후 Login

### 확인 문제

라우터 세팅 문제의 경우 문제에서 어떤 명령어를 사용하여 저장해야 하는지 명시한다.

- write로 저장하라고 한다면 write**(wr)**
- startup-config에 저장하라고 한다면 copy running-config startup-config**(copy r s)**

ex) 라우터의 호스트 이름을 "ICQA"로 설정하라

> : 사용자 모드
> #: 관리자 모드

```
Router> enable // 관리자 모드로 전환
Router# config terminal // terminal 설정 모드
Router(config)# hostname ICQA
Router(config)# exit
ICQA# write // 또는 copy r s
```

1. 인터페이스 정보를 확인하다

```
enable
show interface
copy r s
```

2. 접속한 사용자를 확인하라

```
enable
show user(s)
copy r s
```

3. 라우팅 테이블을 확인하라

```
enable
show ip route
copy r s
```

4. 플래쉬를 확인하라

```
enable
show flash
copy r s
```

### IP / SubnetMask 설정

- show running-config: 현재 설정된 환경을 보여준다.

1. Router 1의 Ethernet 0 인터페이스를 설정하고, NVRAN에 저장하시오.

```
Router> enable **( en )**// 사용자 모드 -> 관리자 모드
Router# config terminal **( conf t )// 터미널 성정모드로 진입
Router(config)# interface Ethernet 0 **( int e0 )**// 인터페이스 이더넷 0으로 진입
Router(config-if)# ip address 192.168.200.2 255.255.255.252 // 문제에 주어진 환경대로 **ip와 서브넷 마스크 설정**
Router(config-if)# ip directed-broadcast // 주어진 환경대로 설정
Router(config-if)# exit // interface e0에서 빠져나온다.
Router(config)# exit // config terminal에서 빠져나온다.
Router# write // NVRAM에 저장
```

### BandWidth(대역폭), clock rate 설정

bandwidth 명령어

1. Serial 0의 대역폭을 2048로 설정하고, NVRAM에 저장하시오.

```
Router> enable
Router# config terminal
Router(config)# interface serial 0
Router(config-if)# bandwidth 2048
Router(config-if)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

2. clock rate를 72k(72000)로 설정하시오.

```
Router> enable
Router# config terminal
Router(config)# interface serial 0
Router(config-if)# clock rate 72000
Router(config-if)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

### Description(주석) 설정

1. Ethernet 0의 description을 설정하고 NVRAM에 저장하시오.
   description: ICQA

```
Router> en
Router# conf t
Router(config)# int eth0
Router(config-if)# des ICQA
Router(config-if)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

### Secondary 설정

IP와 서브넷 마스크를 설정하고, secondary까지 설정하면 끝이다.

1. Ethernet 0의 IP Address를 192.168.2.1/30과 192.168.3.1/30 Secondary로 설정하고 저장하시오,

```
Router> en
Router# conf t
Router(config)# int eth0
Router(config-if)# ip add 192.168.2.1 255.255.255.252
Router(config-if)# ip add 192.168.3.1 255.255.255.252 secondary
Router(config-if)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

위 예제를 에뮬레이터로 돌릴 때 secondary 부분이 오류가 날 수 있지만, 실제 시험장에서는 정상적으로 작동된다 한다.

문제에서 /30을 헷갈릴 수 있다. 이것은 서브넷 마스크를 다르게 표현한 것이다.
30은 1이 30개라는 뜻이다.
기본적으로 서브넷마스크나 IP는 32비트를 기준으로 한다.
1이 30개라 했으므로 192.168.2.1 **/30 = 11111111.11111111.11111111.11111100(252)**

### 기본 게이트웨이 설정

1. Default-Gateway를 설정하고, 저장하시오.
   IP: 192.168.0.10

```
Router> en
Router# conf t
Router(config)# ip default-gateway 192.168.0.10
Router(config)# exit
Router# write // 또는 copy r s
```

### 텔넷 패스워드 설정

텔넷의 비밀번호를 설정하는 문제이다.

1. Router1 Telnet에 접근하는 Password를 "TELPass"로 설정하고, 상태를 저장하시오

```
Router> en
Router# conf t
Router(config)# line vty 0 4 // 텔넷 interface에 접근하는 명령어
Router(config-line)# password TELPass
Router(config-line)# login
Router(config-line)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

**이 경우, 꼭 password 설정을 먼저 해준 뒤, 로그인을 해야한다.**

### 텔넷(콘솔) 자동종료 설정

1. Telnet에 5분 50초 동안 신호가 없을 시 해당 세션을 자동으로 종료하도록 라우터를 설정하시오.

```
Router> en
Router# conf t
Router(config)# line vty 0 4 // 또는 ex- (console 0)
Router(config-line)# exec-timeout 05 50 // 5분 50초
Router(config-line)# login // 문제에서 하라고 하면 하면 된다.
Router(config-line)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

### 콘솔 패스워드 섫정

1. Router1 console의 패스워드를 ICQACon으로 설정하고, 저장하시오.

```
Router> en
Router# conf t
Router(config)# line console 0
Router(config-line)# password ICQACon
Router(config-line)# login // 문제에서 하라고 하면 하면 된다.
Router(config-line)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

### 활성화 설정

1. Router2의 Interface Serial 0을 활성화 시키고 저장하시오.
   이 문제의 경우는 Interface Serial 0이 Shutdown 되어 있다는 것을 전제로 한다.
   해돵되는 라우터를 show running-config 할 경우 shutdown이 되어있는 것을 확인할 수 있다.
   (굳이 확인해볼 필요는 없고 serial 0을 들어가서 no shutdown만 해주면 된다.)

```
Router> en
Router# conf t
Router(config)# interface serial0
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

### 호스트(도메인) 네임, 패스워드 설정

1. Hostname을 network2로 변경하고, Console 0의 Password를 route5로 변경하고 Login하라.

```
Router> en
Router# conf t
Router(config)# hostname network2
network(config)# line console 0
Router(config-line)# password route5
Router(config-line)# login
Router(config-line)# exit
Router(config)# exit
Router# write // 또는 copy r s
```

2. 도메인 이름을 AAA로 설정하라.

```
Router> en
Router# conf t
Router(config)# ip domain-name AAA
Router(config)# exit
Router# write // 또는 copy r s
```

```

```

```

```

```

```

```

```

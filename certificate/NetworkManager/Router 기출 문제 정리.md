본 게시글은 네트워크 관리사 2급을 준비하며 공부했던 내용을 순서대로 작성한 글 입니다.

# 서브넷마스크 설정 문제

tip1) copy r s는 conf t(터미널 설정 모드)가 아닌 en(관리자 모드)에서 사용한다.

tip2) default가 들어갈 경우 "-"를 붙여준다.

---

아래와 같이 Router 1의 ethernet 0 인터페이스를 설정하고, NVRAN에 저장하시오.

<보기>

```
interface Ethernet 0
ip address 192.168.200.2 255.255.255.252
ip address directed-broadcast
```

<답>

```
en
conf t
int e0
ip address 192.168.200.2 255.255.255.252
ip address directed-broadcast
exit
exit
copy r s
```

---

ethernet의 ip address를 192.168.2.1/30와 192.3.1/30 secondary로 설정하고 저장하시오

<답>

```
en
conf t
int eth0
ip address 192.168.2.1 255.255.255.252
ip address 192.168.3.1 255.255.255.252 secondary
exit
exit
copy r s
```

---

# 대역폭 문제

serial 0의 대역폭을 2048로 설정하고 NVRAM에 저장하시오.

<답>

```
en
conf t
interface serial 0
bandwidth 2048
exit
exit
copy r s
```

---

serial 0의 clock rate를 72k로 설정하고 NVRAM에 저장하시오.

<답>

```
en
conf t
interface serial 0
clock rate 72000 // clock과 rate 사이 공백 있음
exit
exit
copy r s
```

---

# 확인 문제

인터페이스의 정보를 확인하라

<답>

```
en
show interface
copy r s
```

---

접속한 사용자를 확인하라.

<답>

```
en
show user
copy r s
```

---

**라우팅 테이블: ip route**

라우팅 테이블을 확인하라

<답>

```
en
show ip route
```

---

플래쉬를 확인하라

<답>

```
en
show flash
copy r s
```

---

호스트 네임을 ICQA로 변경하라

<답>

```
en
conf t
hostname ICQA
exit
copy r s
```

---

**소프트웨어/IOS 버전 확인: version**

소프트웨어 버전과 IOS 버전을 확인하라

<답>

```
en
show version
copy r s
```

---

**CPU process list 확인: process**

CPU process list를 확인하라

<답>

```
en
show process
copy r s
```

---

# 설정 문제

ethernet 0의 description을 ICQA로 설정하고 NVRAM에 저장하시오.

<답>

```
en
conf t
interface ethernet 0
description ICQA
exit
exit
copy r s
```

---

**default-gateway 설정: ip default-gateway**
default-gateway를 설정하고 저장하시오 (ip: 192.168.0.10)

<답>

```
en
conf t
ip default-gateway 192.168.0.10
exit
copy r s
```

---

**router telnet 설정 모드: line vty 0 4**
**password 설정 후 꼭 login을 해주어야 한다.**

router1 telnet에 접근하는 password를 "telpass"로 설정하고 상태를 저장하시오.

<답>

```
en
conf t
line vty 0 4
password telpass
login
exit
exit
copy r s
```

---

**te신호가 없을 시 해당 세션 자동 종료: exec-timeout**

telnet에 5분 50초 동안 신호가 없을 시 해당 세션을 자동으로 종료하도록 라우터를 설정하시오.

<답>

```
en
conf t
line vty 0 4
exec-timeout 05 50
exit
exit
copy r s
```

---

**router consol 설정 모드: line console 0e**

router1 console의 패스워드를 ICQACon으로 설정하고 저장하시오.

<답>

```
en
conf t
line console 0
password ICQACon
exit
exit
copy r s
```

---

**활성화: no shutdown**

router2의 interface serial 0을 활성화 시키고 저장하시오.

<답>

```
en
conf t
interface serial 0
no shutdown
exit
exit
copy r s
```

---

hostname을 network2로 변경하고, console 0의 password를 router5로 변경하고 login하시오.

<답>

```
en
conf t
hostname network2
line console 0
password router5
login
exit
exit
copy r s
```

---

**캡슐화: encapsulation frame-relay(꼭 "-" 붙여줘야 한다.)**

serial 2/0에 frame relay 방식으로 캡슐화 하시오

<답>

```
en
conf t
interface serial 2/0
encapsulation frame-relay
exit
exit
copy r s
```

---

**router1에 access-list 1이 설정되어 있을 때, fastethernet 0/0에 적용하시오.**

<답>

```
en
conf t
interface fastethernet 0/0
ip access-group 1 in
ip access-group 1 out
exit
exit
copy r s

```

---

192.168.1.1 네트워크로 default network를 설장하시오.

<답>

```
en
conf t
default-network 192.168.1.1
exit
copy r s
```

---

<보기>

```

```

<답>

```

```

---

<보기>

```

```

<답>

```

```

---

<보기>

```

```

<답>

```

```

---

<보기>

```

```

<답>

```

```

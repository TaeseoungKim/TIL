본 게시글은 네트워크 관리사 2급을 준비하며 공부했던 내용을 순서대로 작성한 글 입니다.

## Linux

리눅스를 알고 있거나 리눅스쪽으로 자격증을 더 취득하거나 취업을 할 사람은 괜찮지만, **단순히 네트워크관리사 2급을 취득하기 위해서라면 리눅스를 공부할 시간에 라우팅이나 Window Server를 한번 더 보는 것이 훨씬 낫다.**

아래의 내용은 기출 문제와 답을 정리하였다.
단순히 네트워크관리사 2급을 취득하기 위해서라면 문제와 답만 외울 것을 추천한다.

### 기출 문제 모음

---

윈도우의 ipconfig 명령어 같이 리눅스에서 ip address 정보를 확인하는 명령어는?
답: ifconfig

---

리눅스에서 파일이나 폴더를 찾을 때 사용하는 명령어는?
답: find

---

리눅스에서 현재 설치된 **하드디스크와 용량**을 확인하는 명렁어는?
답: df

---

리눅스에서 현재 위치를 확인하고 싶을 때 사용하는 명령어는?
답: pwd

---

리눅스에서 도움말-메뉴얼을 확인하는 명령어는?
답: man

---

리눅스에서 파일 및 디렉터리 별 **디스크 사용량**을 확인하는 명령어는 무엇인가요?
답: du

---

명령어 중 포트와 프로토콜 정보를 확인하는 명령어는?
답: /etc/services

---

현재 실행되는 프로세스들을 확인하는 명령어는?
답: ps

---

리눅스에서 네트워크 연결상태, 라우팅 테이블 정보등을 확인하는 명령어는?
답: netstat

---

파일 또는 디렉토리 삭제 명령어
답: rm

---

CPU 및 메모리 사용정보를 확인하는 명령어
답: top

---

계정추가 명령어: useradd
패스워드: passwd

---

각 계정들의 최근 접속 정보를 확인하는 명령어로 서버의 보안점검을 위하여 필수적으로 사용하는 명령어
답: last

---

마운트 명령어: mount
언마운트 명령어: umount

---

1. 인터페이스 정보를 확인하다

```
enable
show interface
copy r s
```

---

2. 접속한 사용자를 확인하라

```
enable
show user(s)
copy r s
```

---

3. 라우팅 테이블을 확인하라

```
enable
show ip route
copy r s
```

---

4. 플래쉬를 확인하라

```
enable
show flash
copy r s
```

---

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

---

Router(config-line)# exec-timeout 05 50 // 5분 50초
Router(config-line)# login // 문제에서 하라고 하면 하면 된다.

---

2. 도메인 이름을 AAA로 설정하라.

```
Router> en
Router# conf t
Router(config)# ip domain-name AAA
Router(config)# exit
Router# write // 또는 copy r s
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

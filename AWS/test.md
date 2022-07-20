## AWS 소개

AWS - 2006년 아래 3개의 서비스와 함께 시작

1. EC2
2. S3
3. SQS

현재는 글로벌 데이터 센터로, 약 200개의 안전하고 강력한 서비스를 제공한다.

클라우드는 "밀키트"다?
-> 클라우드도 밀키트처럼, 만들어져 있는 소프트웨어들을 조립하기만 하면 된다.
-> 좀 더 정확하게 말하면, **인터넷**의 **IT자원**을 내가 원하는 **종량제**만큼 사용하는 것

### 고객이 AWS로 이전하는 이유

민첩성 증가 - 출시 기간 단축, 혁신 확대, 원활한 크기 조정
복잡성 및 위험 감소 - 비용 최적화, 보안 취약성 최소화, 관리 복잡성 감소

AWS 서비스는 아래와 같이 나뉜다.

비관리형: EC2...

관리형: RDS...

- 용량 증감 등

완전관리형/서버리스: Lambda, DynamoDB...

- 설정이나 변경조차도 필요가 없음

region - AWS에서 서비스하고 있는 지리적 위치(ex. Seoul, Oregon, N.Viginia, Singapaul... 또는 ap-northeast-2와 같이 코드명으로 표기)

가용영역 - Availavility Zone(AZ), 데이터센터가 cluster 형태로 구성

Q) 왜 리전은 2개 이상의 AZ로 구성되어 있을까?

- "고가용성": 장애가 나지 않도록 설계하는 것
- User가 요청을 보냈을 때, 하나의 AZ에 장애가 있더라도 다른 AZ를 통해 정상적으로 사용한다 -> 마치 장애가 없는 것처럼 사용
- AZ는 낮은 레이턴시와 안정적인 대역폭을 가진 private link로 연결되어 있다.
- Seoul은 4개, Oregon은 4개, N.Viginia은 6개, Singapaul은 3개의 AZ로 구성되어 있다.

Region > AZ > Data Center > Server
각 Region들은 AWS Backbone network(해저 광케이블)로 연결되어 있다.

edge station: 캐싱이 가능한 시설물(Data Center), 전세계에 약 200개가 분포
Cloud Front와 같은 서비스가 주로 사용한다.

내결함성: **하나의 컴포넌트 내**에서 장애가 나지 않도록 설계하는 것
내결함성을 가진 컴포넌트들을 가지고 고가용성 있게 설계하는 것이 중요하다.

AWS 리전을 선택할 때는 **현지 데이터 규제**, **최종 사용자 지연 시간**을 고려해야 한다.

점주 -> 관리자 -> 바리스타, 캐셔, 재고관리자

AWS Account(점주)

- AWS Account을 직접적으로 사용하면, Account를 탈취 당했을 떄 큰 문제가 생긴다.
- 이를 해결하기 위해 admin 권한을 가지고 있는 IAM User를 만들어서 사용한다.

IAM User(관리자)

- admin 권한을 가진 사용자

developer, QA Engineerm devops Enginner(바리스타 등...)

- 권한을 가진 사용자

---

user

group

role

- 사용자 또는 **서비스**에게 임시권한을 주는 방법

policy -

---

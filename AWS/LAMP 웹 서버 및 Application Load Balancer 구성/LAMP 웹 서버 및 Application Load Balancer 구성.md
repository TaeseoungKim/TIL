## LAMP 웹 서버 및 Application Load Balancer 구성

클라우드 네트워크 환경에 Linux 기반의 가상 서버에 Apache 웹 서버, MySQL 데이터베이스, PHP 어플리케이션을 구성하고 Application Load Balancer를 이용하여 이중화 된 네트워크를 구성합니다.

LAMP 웹서버: 클라우드 네트워크 환경에 Linux 기반의 가상 서버에 Apache 웹 서버, MySQL 데이터베이스, PHP 어플리케이션을 구성한 서버

public subnet(including EC2 Bastion, NAT): 외부와 직접적인 통신이 가능하다.
private subnet(including EC2): 외부와 직접적인 통신이 블가능하다. 업데이트나 유지보수를 위해 public에 위치한 instance(EC2)를 통해 외부와 연결한다. 외부 인터넷과 통신하기 위해 public subnet의 NAT을 이용한다.

Load Balance: 트래픽 과부하 방지

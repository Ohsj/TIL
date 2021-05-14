# VPC란

VPC(Virtual Private Cloud)

-- 정리 필요


public subnet안에 있는 ec2로 접근 -> vpc 내부로 들어 왔으니 ec2에서 private subnet 안에 있는 app으로 접근 -> 나갈때는 nat gateway로 나가서
nat gateway로 elastic ip를 할당해 놓으면 나가는 ip(out bound)가 하나로 통일 된다.

들어오는 경우에 ALB(Application Load Balancer)를 public subnet 안에 만들고 elastic ip를 할당해 놓으면 private subnet 안에 있는 app으로 접근할때 
고정된 하나의 ip(in bound)로 접근이 가능하다.

### Subnet

### IGW

IGW(Internet GateWay)

### NatGW

NatGW(Nat GateWay)

### Routing Table

```Routing Table```에는 ```Subnet``` 또는 ```Gateway```의 네트워크 트레픽이 전송되는 위치를 결정하는 데 사용되는 ```Routing```이라는 규칙 집합이 포함되어 있습니다.

#### 기본 개념

 - Main Route Table : VPC와 함께 제공되는 라우트 테이블. 다른 라우텡 테이블과 명시적으로 연결되지 않은 모든 서브넷의 라우팅을 제어.
 - Custom Route Table : VPC에 대해 생성하는 라우트 테이블.
 - Edge Association : 어플라이언스로 VPC 트래픽을 인바운드 하는데 사용되는 라우트 테이블.
 - Route Table Association : 라우트 테이블과 서브넷, 인터넷 게이트 웨이 또는 가상 프라이빗 게이트웨이 간의 연결
 - Subnet Route Table : 서브넷과 연결된 라우트 테이블
 - Gateway Route Table : 인터넷 게이트웨이 또는 가상 프라이빗 게이트웨이를 연결한 라우트 테이블
 - Local Gateway Route Table : Outposts 로컬 게이트웨이와 연결한 라우트 테이블.
 - Destination : 원하는 트래픽으로 이동할 수 있는 IP 주소의 범위.
 - Propagation : 라우트 전파을 사용하면 가상 프라이빗 게이트웨이가 라우팅 테이블에 라우팅을 자동으로 전파할 수 있다.(VPC 라우팅을 수동으로 입력할 필요 없다.)
 - Target : 대상 트레픽을 전송할 때 사용할 게이트 웨이, 네트워크 인터페이스 또는 연결(예: 인터넷 게이트웨이)
 - Local Route : VPC 내 통신을 위한 기본 라우트

&#8251; Appliance : 특정 목적에 최적의 성능을 낼 수 있도록 하드웨어, 운영체제, 소프트웨어가 다 설치되고 셋팅된 제품을 구매하여 바로 사용하는 개념
&#8251; Outposts  : 일관된 하이브리드 환경을 위해 동일한 AWS 인프라, AWS 서비스, API 및 도구를 모든 데이터 센터, 코로케이션 공간, 온프레미스 시설에 제공하는 완전관리형 서비스.

#### 작동 방식

VPC에는 암시적 라우터가 있으며 라우팅 테이블을 사용하여 네트워크 트래픽이 전달되는 위치를 제어한다.
VPC의 각 서브넷을 라우팅 테이블에 연결해야 한다. 테이블에서는 서브넷에 대한 라우팅을 제어한다.(서브넷 라우팅 테이블)
서브넷을 특정 라우팅 테이블과 명시적으로 연결할 수 있다. 그렇지 않으면 서브넷이 기본 라우팅 테이블과 암시적으로 연결된다.
```서브넷은 한 번에 하나의 라우팅 테이블만 연결```할 수 있지만 ```여러 서브넷을 동일한 서브넷 라우팅 테이블에 연결```할 수 있다.

라우팅 대상은 모든 IPv4 주소를 나타내는 0.0.0.0/0 이며 대상은 인터넷 게이트웨이다.
```IPv4 및 IPv6 CIDR 블록은 별도로 취급```된다. 예를 들면 대상 CIDR이 0.0.0.0/0인 라우팅에는 모든 IPv6 주소가 자동으로 포함되지 않는다.
모든 IPv6 주소에 대한 대상 CIDR이 ::/0인 라우팅을 생성해야 한다.

```모든 라우팅 테이블에는 VPC 내부 통신을 위한 로컬 라우팅이 포함```되어 있다. 이 라우팅은 기본적으로 모든 라우팅 테이블에 추가된다.
VPC에 IPv4 CIDR 블록이 연결되면 라우팅 테이블에 IPv4 CIDR 로컬 경로가 포함되고 IPv6 CIDR 블록이 연결되면 라우팅 테이블에 IPv6 CIDR 로컬 경로가 포함된다.
라우팅 테이블에서는 이런 라우팅을 수정하거나 삭제할 수 없다.


### ref
 - [가장쉽게 VPC 개념잡기](https://medium.com/harrythegreat/aws-%EA%B0%80%EC%9E%A5%EC%89%BD%EA%B2%8C-vpc-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-71eef95a7098)
 - [VPC의 라우팅 테이블](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
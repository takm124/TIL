### 1. Internet 기반 네트워크 입문 <br>
<br>

> 네트워크와 네트워킹 그리고 개념

  - 네트워크 = 관계
    - 네트워킹 = 상호작용 

  - 상호작용(networking)을 위해서 전제조건이 필요하다
    - 언어가 통하거나, 거리가 가깝거나, 소통은 무엇으로 할 것인가 등
 

> User mode와 Kernel mode
  - L5, L6, L7은 User mode의 영역
    - process 수준의 설명 가능
  - L4, L3는 kernel 영역
  - L2는 kernel과 H/W 부분에 걸쳐있음
  - L1은 H/W 영역

> OSI 7 layer와 식별자
  - 실제 process가 진행될 때 각 Layer가 담당하는 역할이 무엇인지 확인하기
  - 식별자
    - L2 = MAC Address (Physical address) = 랜카드에 붙어있음 (랜카드 식별)
    - L3 = IP주소 (host 식별)
    - L4 = Port 번호 (인터페이스(L2), 서비스(L4), 프로세스 식별)

> Host는 이렇게 외우자
  - Host = Computer + Network (네트워크에 연결된 컴퓨터)
    - switch = 네트워크 그 자체를 이루는 host(infra), ex) 라우터
    - End-point (단말기) = 네트워크 이용 주체 (ex) client, server, 단말기의 역할에 따른 분류임)


> 스위치가 하는 일과 비용
  - 하는 일 : 경로 선택 (= interface 선택, switching)
  - 비용 (Matric 값) = 경로 상의 네트워크 또는 링크들을 모두 거쳐 지나가는데 할당되는 비용 (싼게 좋음)

<br>
<br>

### 2. L2

> NIC, L2 Frame, LAN 카드 그리고 MAC 주소
  - NIC (Network Interface Card) = LAN(Local Area Network) 카드
    - NIC는 H/W이며 MAC주소를 가짐
  - MAC 주소 = 식별자

> L2 스위치에 대하여
  - L2 Access Switch 라고 함
  - End-Point와 직접 연결되는 스위치
  - MAC 주소를 근거로 스위칭
  - Link-up : pc와 L2 스위치 케이블 연결됐다.
  - Link-down : pc와 L2 스위치 케이블 끊어졌다
  - upLink : 상위 케이블로의 연결
  - L2 Distribution switch
    - L2 Access 스위치를 위한 스위치
    - VLAN 기능을 제공하는 것이 일반적

### 3. L3

> IPv4주소의 기본 구조
  - Network ID + Host ID
    - network id : 그룹 구분
    - host id : 대상 구분

> L3 IP Packet
  - 단위 데이터
  - packet을 언급하면 L3 Layer를 생각하면 된다.
  - 최대 크기는 MTU (Maximum Transition Unit), 주로 1500 bytes

> 계층별 데이터 단위
  - Socket interface를 이용해서 data를 보낼 때의 데이터 단위
    - data가 TCP 헤더 붙으면 segment    [MSS]
    - segment에 IP 헤더 붙어서 packet   [MTU]
    - packet에 frame 헤더 붙어서 frame (L1, L2 level)
  - 역으로 socket에서 data가 나올땐 stream
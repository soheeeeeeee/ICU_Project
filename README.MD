## 융복합 프로젝트 (AI, Big data, IoT, Cloud) _ 7조 _ ICU \_ 포트폴리오

---

#### 🖋SUMMARY

- 프로젝트 기간
  - 5월 23일 ~ 6월 28일

</br>

- 프로젝트 주제
  - 스토킹으로 인한 2차 범죄 예방을 위한 지능형 CCTV 개발

</br>

- 프로젝트 기획의도

  - 증가하는 스토킹
  - 피해자 보호 미흡
  - 단순 스토킹에서 흉악범죄로 이어지는 스토킹
  - CCTV의 범죄 예방 효과를 이용한 스토킹으로 인한 범죄 예방안 제시

</br>

- 프로젝트 목표
  - 지능형 CCTV 설치를 통한 개인 보안 강화 (실시간 영상 확인)
  - 스토킹 영상의 저장을 통한 증거 자료 수집 (영상 저장)
  - 이상행동 감지를 통한 스토킹 이후 2차 범죄를 예방 (이상행동 감지 및 알림)
  - 추후 효율적인 서비스 확대를 위한 지능형 CCTV의 우선입지 선정 조사

</br>

- 💬 지능형 cctv의 기능 구현 목표:

<B>💾 1. 증거자료 확보:</B> 사람의 이상행동을 감지하는 지능형 CCTV가 이상행동(스토킹과 비슷한 상황)을 스스로 감지하고 영상을 저장하여 범죄에 대한 증거자료를 확보함.

<B>📩 2. 실시간 영상 전송 및 알림:</B> 이상행동을 감지한 지능형 CCTV가 사용자의 휴대폰에 실시간으로 알림을 전달, 알림을 확인한 사용자는 실시간 영상 및 저장된 영상을 확인하여 상황을 파악함.

<B>☎ 3. 응급 전화 및 SMS 전송 기능:</B> 실시간 알림확인 후 상황을 파악한 사용자가 신속하게 위험상황에 대처가능함.

---

#### 👨‍👨‍👧‍👦 분야별 팀원 소개

- AI, Big data, IoT, Cloud 분야별 팀원 소개

#### 📖 프로젝트 구성도 소개

- 서비스 전체 구성도 및 상세 구성도

#### 📺 프로젝트 기능 소개 (시연영상, PPT)

- 서비스 전체 시연영상 및 PPT

#### 📚 회의록

- Notion 칸반 보드를 사용한 협업 및 주차별 수행일지
- 전체 WBS

#### 😀 느낀점

- 개인별로 직접 쓰시면 좋을거같습니다.

<br>

<B>💡 [자세한 정보 보러 가기](https://rustic-mailman-444.notion.site/7-f448609b0c2e4a69b5191be6b305f322) 💡</B>

<br>

---

## [클라우드] 기능 구현 및 트러블 슈팅 정리

---

### 1. Ubuntu에 mqtt서버(broker) 설치하기

##### 설치환경

Ubuntu 20.04 LTS

##### 라이브러리 설치

$ sudo apt-get update

$ sudo apt-get install build-essential libc-ares-dev uuid-dev libssl-dev

$ libcurl4-openssl-dev libmysqlclient-dev

##### 폴더 만들고 1.4.10 버전 다운받아서 압축 풀기

$ cd ~/

$ mkdir mosquitto_sources

$ cd mosquitto_sources/

$ wget http://mosquitto.org/files/source/mosquitto-1.4.10.tar.gz

$ tar xvf mosquitto-1.4.10.tar.gz

##### libwebsockets 설치 (라이브러리)

$ sudo apt-get install libwebsockets-dev

##### config.mk 파일 수정

$ WITH_WEBSOCKETS:=yes 로 변경 후 저장

##### build해서 설치하기

$ cd ~/mosquitto_sources/mosquitto-1.4.10/

$ make
$ sudo make install

##### mosquitto.conf 파일에서 아래 내용 추가하기

listener 9001
protocol websockets

listener 1883
protocol mqtt

##### Start Broker / 수정한 conf 파일로 시작하기

$ mosquitto -c mosquitto.conf -v

#### 지켜야할 사항

1. 꼭 위의 순서를 지켜서 설치해주자 특히 make 전에 conf파일을 수정해줘야하고 이후 make install 해줘야한다.

<HR></HR>

### 2. IoT에서 수신하는 mqtt데이터를 jpg파일로 변환 후 보관할 DB 구축

#### goofys를 사용하면 쉽게 s3버킷을 EC2에 마운트 해줄 수 있다.

1. S3 만드는 계정의 액세스키 발급받기
2. Amazone s3 콘솔에서 s3 버킷 default 값으로 생성 / 리전확인!!
3. ec2 접속
4. Ubuntu 에서 s3 버컷 마운트해주기

$ mkdir [마운트할 경로]

$ apt-get update

$ apt-get install -y fuse awscli

$ aws configure →
access key /
secret access key /
region : eu-west-2 /
data : json

$ wget https://github.com/kahing/goofys/releases/latest/download/goofys

$ chmod +x goofys

$ /goofys [s3버킷 이름] [마운트할 경로]

- 제대로마운트 해줬다면 EC2의 폴더에서 S3 버킷의 객체를 확인 할 수 있다.

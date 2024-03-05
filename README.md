# AWS_Oracle_Docker
## AWS Ubuntu 환경에 Docker Container 에 오라클 이미지 설치 후 사용하기

# 1. AWS 회원가입 LightSail 들어가기

# 2. Create instance
- 인스턴스 생성
  
![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/17f7fd75-3551-4f2e-a1d8-70e903c138b1)

- 선택 및 설정
  
![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/8cad1bd6-378e-4131-a2c8-5c88343ca36a)

- 키페어 만들기
  
![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/45976a5f-a62e-40b4-aa19-90d9e603a535)

- 클릭시 아래와 같이 나옴

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/1e233687-ed04-4e55-954b-96ae15d7c857)

- 저장 후 나중에 MobaXterm, Putty 등에서 사용해야 하므로 관리 잘 하기

- 원하는 사양에 따라 알맞게 선

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/cd509d78-5298-473c-87cf-90ef49652646)

- 인스턴스 이름

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/ef55854a-3242-46e1-ad95-3c496529a08f)

  
# 3. SSH 접속 / MobaXterm
- 1. 인스턴스 홈에서 바로 접속하기

  ![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/d1cb7776-a95e-4dd0-865a-6675e1176a56)

- 2. 인스턴스 이름 클릭 후 상세정보 페이지에서 접속하기
 
  ![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/7503b484-5b3e-44d0-b27a-cb96102d2ff2)

- 3. MobaXterm 으로 접속하기
> [MobaXterm 다운로드 사이트](https://mobaxterm.mobatek.net/download.html)

> Session 클릭
  ![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/32e33a1f-1c08-4c2b-937b-83a87b9e573f)
> SSH 클릭 후 아래 셋팅
> 1. Remote host : 인스턴스 상세페이지에서 **Public IPv4** 를 입력
> 2. Specify username은 작성 X
> 3. 아래의 Use private Key 체크박스 체크
> 4. 오른쪽의 파일 모양 클릭하여 ***키페어 만들기*** 에서 저장해놓은 키페어 찾기 (xxx.pem)
> 5. 가장 아래의 ok 버튼 클릭 하면 접속이 된다.
  ![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/c9a2f260-e76a-46b3-a40a-487d7a491386)

  ![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/49141b96-01a7-4789-be4c-a69e62b743b8)

경고창이 나오면 Accept
login as : 
가 나오면 ubuntu
를 입력하면 됨

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/a1090959-3897-426f-9523-cdba986c21d9)

이렇게 출력이 되고 

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/4b5c8b29-289c-400e-a2bb-6261063fbb34)


이런식으로 나오면 연결이 된 것

# 4. 인스턴스 Networking 텝에 들어가서 포트를 열어줘야 한다.
- 간략하게 설명
- 22 : 외부에서 SSH로 접속하기 위해 열어둬야하는 포트
- 80 : 아이피주소만 입력했을 때 연결되는 포트
- 1521 : Oracle DB 사용시 DB의 기본 포트

# 5. AWS 에서 접속하는 SSH에서 한글은 입려되지 않으므로 입력이 되지 않을 때 한글인지 확인

# 6. SSH 접속 후 Docker, OracleDB 설

$ xxxx << 와 같이 앞에 $ 가 붙으면 SSH 에서 입력한 것이나 아래의 설명에선 생략하겠습니다.

sudo 는 Superuser do 약어로 관리자 권한으로 실행하는것 
root 권한으로 실행한다 라는 뜻

-  패키지 관리를 업데이트하고 시스템의 모든 패키지를 최신 버전으로 업그레이드
  
sudo apt-get update && sudo apt-get upgrade

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/d3d6d154-f195-46a4-b3e2-203bb218829f)

- Y 입력 후 Enter 

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/bf6549ef-e5fb-4b4a-bb7a-f43ab4e6d834)

- keep the local version currently installed << Enter

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/7b630863-3156-4e36-bf9f-b12a5a05ab6c)

- 한 번 더 Enter

- 업데이트가 끝난 후 Docker 를 설치해야 한다.

- 필요한 페키지 설치
  
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/b909da32-a7b1-4210-a9f2-5672c381798b)

- Enter

- Docker 공식 GPG키 추가
  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/6885d1e6-0dfc-494f-9d91-66814b665a50)

- Docker 의 공식 apt 저장소 추가
  
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/cf46ac6d-fd60-41a0-ba01-39459330beca)


- 한 번 더 업데이트
  
sudo apt-get update

- Docker 설치
  
sudo apt-get install docker-ce docker-ce-cli containerd.io

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/590f2eeb-e65a-47a8-bb38-50d5b793873a)

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/03966d43-edab-42ef-bfeb-bcace5e2e4ff)

Enter

- Docker 실행상태 확인
  
sudo systemctl status docker

Active: active (running) 이므로 실행중

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/f5a3bf5c-e20d-4071-9cb2-f9b370c909a7)

- Docker 실행(Test 용도)
  
sudo docker run hello-world

### 이 아래부터는 sudo -i 를 입력하여 root 권한으로 진행하므로 sudo 빠져있을 수 있음

- root(관리자) 권한
  
sudo -i

ubuntu -> root 로 바뀌는 것을 확인할 수 있

- Docker 에서 현재 실행 중인 컨테이너뿐만 아니라 중지된 컨테이너도 모두 표시\
  
docker ps -a

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/284b2692-38bd-4779-9fed-0eaf697baf30)

- Docker 컨테이너 삭제
  
docker rm [컨테이너 ID] 또는 [컨테이너 이름]

- 다운로드 가능한 oracle-xe-11g 리스트 출력
  
docker search oracle-xe-11g

- 추천 수가 높은 이미지를 선택해서 다운로드 받기
- "oracleinanutshell/oracle-xe-iig" 사용
- 이미지 다운로드
  
docker pull oracleinanutshell/oracle-xe-11g

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/d7a4cdff-8446-4050-abe5-41796cc5be2a)

-  Pull complete 여러 개 뜨면 성공

- Oracle 이미지 실행

docker run -d -p 1521:1521 oracleinanutshell/oracle-xe-11g

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/9131ec16-de8f-4954-b40c-efe8ffa5f575)

컨테이너 가 출력됨

- 컨테이너 확인
  
docker ps -a

- 위에 있는 것이 오라클 이미지이다.
- 아래는 위에서 테스트 했던 것
- 오라클 컨테이너 상태를 보면 Up xx ~~~ 로 Up 이라고 나와 있으면 실행

![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/588d5744-8fc7-4aa0-8ff0-7de9c21643b9)

- docker 컨테이너 실행 방법

docker start [컨테이너 ID]

- docker 이미지 실행 방법
- 이미지는 컨테이너가 실행중일 때 가능하므로 컨테이너가 실행중이 아니라면 실행 후 진행

docker exec -it [이미지_NAME] bash

- 위와 같이 이미지를 실행하면 root 옆에 숫자가 바뀌는 것을 확인할 수 있고

- sqlplus 로 OracleDB에 접속할 수 있다

- system 계정의 초기 비밀번호는 oracle이다.

# 7. Sql Developer 에서 접속하기
![image](https://github.com/hnymon/AWS_Oracle_Docker/assets/151509541/400f8152-e10b-442a-a6c7-2d2cfc5983ec)

- 호스트 이름에는 인스턴스의 Public IPv4 를 입력하면 된다. 나머지 설정은 그대로


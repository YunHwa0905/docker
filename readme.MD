# Docker Study

Docker 기초 문법 및 컨테이너 운영을 단계별로 학습한 실습 저장소입니다.

---

## 기술 스택

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat&logo=nginx&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![OpenJDK](https://img.shields.io/badge/OpenJDK_17-007396?style=flat&logo=openjdk&logoColor=white)

---

## 프로젝트 구조

```
docker/
├── readme.MD              # Docker CLI 전체 명령어 정리 (이미지·컨테이너·네트워크·볼륨)
│
├── lec1/
│   └── Dockerfile         # FROM — 베이스 이미지로 이미지 생성
│
├── lec2/
│   └── Dockerfile         # ADD — 압축파일·URL·파일 패턴 추가
│
├── lec3/
│   └── Dockerfile         # RUN·CMD·EXPOSE — nginx 설치 및 포트 설정
│
├── lec4/
│   └── Dockerfile         # WORKDIR·COPY — 작업 디렉토리 지정 및 소스 복사
│
├── lec5/
│   └── Dockerfile         # LABEL·ARG — 이미지 메타데이터 및 빌드 변수
│
└── lec6/
    └── Dockerfile         # VOLUME — 볼륨·바인드 마운트 설정
```

---

## 실행 방법

각 lec 폴더의 Dockerfile을 독립적으로 빌드하여 실습합니다.

```bash
# 이미지 빌드 예시 (lec3 — nginx)
docker build -t test_ub:3.0 ./lec3

# 컨테이너 생성 및 가동
docker run -d -p 80:80 --name test_ub30 test_ub:3.0

# 브라우저에서 확인
# http://localhost
```

```bash
# lec4 — WORKDIR·COPY 실습
docker build -t test_ub:4.0 ./lec4
docker run -it --name test_ub40 test_ub:4.0
# 컨테이너 내부 파일 구조 확인
# ls /home/ubuntu/app -al
```

```bash
# lec6 — VOLUME 실습 (nginx + 볼륨 마운트)
docker build -t test_ub:6.0 ./lec6
docker run -itd -p 80:80 --name test_ub60 test_ub:6.0
```

> `readme.MD`의 CLI 명령어를 순서대로 따라가며 이미지·컨테이너·네트워크·볼륨을 함께 실습합니다.

---

## 학습 내용

| 폴더 / 파일 | 주요 학습 내용 |
|------------|---------------|
| `readme.MD` | docker version·info·df, 이미지 pull·push·tag·inspect·rm, 컨테이너 run·exec·logs·ls, 네트워크 create·connect·disconnect, 볼륨 마운트·바인드 마운트 |
| `lec1` | `FROM` — 베이스 이미지 지정, `docker build` 기본 흐름 |
| `lec2` | `ADD` — 압축 파일 자동 해제, URL 다운로드, 와일드카드 패턴(`*`, `?`) 파일 추가 |
| `lec3` | `RUN` — 패키지 설치, `EXPOSE` — 포트 선언, `CMD` — 컨테이너 기동 명령 |
| `lec4` | `WORKDIR` — 작업 디렉토리 설정, `COPY` — 소스·리소스를 이미지 내 특정 위치에 복사 |
| `lec5` | `ARG` — 빌드 타임 변수, `LABEL` — 이미지 메타데이터(`docker image inspect`로 확인) |
| `lec6` | `VOLUME` — 볼륨 마운트·바인드 마운트 경로 선언, nginx 설정 파일 외부 제어 |

---

## 개발 환경

- **Docker Desktop** — [다운로드](https://www.docker.com/products/docker-desktop/)
- **Windows 11** + PowerShell / WSL2

```bash
# MySQL 컨테이너 실행
docker run -d -p 3306:3306 --name my_mysql \
  --env MYSQL_USER=dev \
  --env MYSQL_PASSWORD=1234 \
  --env MYSQL_ROOT_PASSWORD=1234 \
  mysql
```

```bash
# 3-tier 네트워크 구성 예시 (web — was — db)
docker network create --driver=bridge web
docker network create --driver=bridge was
docker network connect web <nginx-container>
docker network connect was <springboot-container>
docker network connect was <mysql-container>
```

---

## Docker CLI 명령어 노트

> 상세 실습 명령어는 [readme.MD](readme.MD)를 참고하세요.

## 도커 기본 명령어, 이미지
```
# 1. 도커 버전 확인, 설치 확인
# 클라이언트, 서버측, 아키텍처 상세 정보 노출됨
docker version

# 2. 실행 환경에 대한 구체적인 정보 제공
# 컨테이너 수, 도커 버전, os, 드라이버, ...
docker system info

# 3. 디스크 이용 정보
# EC2에서 운영시 검토 -> 필요하면 볼륨 스케잉링 처리
docker system df

# 4. 이미지 관련
# ubuntu, nginx, java, jdk17, node, python
# 한 개의 od, s/w 등등이 통상 이미지 (오피셩, 유명한 이미지)
# 한 개의 개발(운영)환경 (nginx + react 세팅된) -> 자유롭게 구성 가능 함
# 고유함 명령어라면(2차) 1차 명령어 없이 사용 가능
docker image(1차 명령어) 2차 명령어 옵션

# 4-1. 도커 허브로부터 이미지 다운로드
# docker image pull 옵션 이미지명[:태그명] <- 대괄호 생략 가능함
# ubuntu 이미지 다운로드
# hub.docker.com > 이미지 검색 > 검토 > 다운로드
docker image pull ubuntu

# nginx
docker image pull nginx

# my sql
docker image pull usql

# jdk17 (내려갈수록 용량이 작아짐)
# image 생략한 명령어 
docker pull openjdk:17
docker pull openjdk:17-slim
docker pull openjdk:17-apline

# 4-2. 도커 내에 다운로드 된 이미지 목록 확인
docker image ls

# 4-3. 이미지 상세 정보(inspect)
# 설치된 내용, 환경변수, 생성일, ... 다양한 정보 조회
docker image inspect openjdk:17

# 4-4. 이미지 검색
docker search nginx
# STARS 순으로 정렬하여 상위 3개만 검색 결과 출력
docker search nginx --limit 3

# 4-5. 이미지 삭제 (강제 삭제, 미사용 이미지 삭제, ...)
docker image rm nginx
docker image ls

# 4-6. 도커 허브 로그인(사전에 가입 후)
# 명령어 -> 브라우저 작동(링크) -> 로그인 -> 성공 메세지 전달
docker login

# 4-7. 이미지를 커스텀 생성 -> 허브에 업로드
# 개발환경 배포, 도커 기반 CI/CD 활용
# 도커 허브 (도커 제공, AWS 자체적 서비스로 제공(cloud native 하게 구성))

# 4-7-1. 원본 이미지 획득
docker image pull nginx

# 4-7-2. 커스텀 진행(생략) -> 커스텀 이미지 생성
# 업로드될 이미지명 => 계정명 필수/이미지명[:태그(버전)]
# 가정 : nginx에 리액트로 개발한 내용을 빌드하여 세팅 후 이미지로 굽는다(생성)
# 이미지 -> 이미지 or 컨테이너(커스텀 후) -> 이미지
docker image tag nginx yunhwa0905/frontend:1.0
# 확인
docker image ls

# 4-8. 커스텀 이미지 도커 허브에 업로드(push)
docker image push yunhwa0905/frontend:1.0

# 4-9. 로그아웃
docker logout
```


## 도커 컨테이너 
```
# 실질적으로 도커 위에서 작동하는 인스턴스, 이미지로는 아무것도 작업하지 못함
# 5. 컨테이너
# 명령어 구조
docker container(1차) COMMAND(2차) 옵션
docker container(1차) COMMAND(2차) --help -> 도움말

# 5-1. 우분투 설치(컨테이너 구성, 가동)
# a.이미지 다운 -> b.컨테이너 생성 -> c.컨테이너 가동 -> d.사용 ... -> 컨테이너 중단 -> 컨테이너 삭제 -> 이미지 삭제
# 위의 a~d 한 번에 처리 => run(고유한 2차 명령어, container 생략 가능함)
# i:입력, t:출력, d:백그라운드
# --name           : 컨테이너의 이름
# ubuntu 자리에 값은 : 이미지명
docker run -itd --name my_ubuntu ubuntu
8597d72798030803927473ba49b8c88dc664fe124bad47826f64d427fb433eaf <- 해시값(고유관리번호)

# 도커 위에 우분투 기능을 수행하는 컨테이너 1개 구성(독립덕, 격리, 언제든지 삭제 가능함)

# 5-2. 컨테이너 접속
# EC2 인스턴스 -> 연결 -> 터미널 오픔 -> 명령어 업무 수행
# 통상 모든 컨테이너는 리눅스 위에서 세팅 -> bash 쉘을 통해서 접근 가능
docker exec -it my_ubuntu bash
---
root@8597d7279803:/# #
root@8597d7279803:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var     
root@8597d7279803:/# cd /home/ubuntu
root@8597d7279803:/home/ubuntu# exit
exit
---

# 5-3. nginx 컨테이너 생성 및 가동 -> 서비스개념(포트관리)
# nginx -> 웹서버 -> 80 포트 사용 -> URL 요청시 생략됨
# -p 80:80 => 포트 지정, [로컬 PC에서 접근 포트]:[컨테이너 내부에서 작동 포트]
docker run -d -p 80:80 --name my_nginx nginx
# 접속 확인 (브라우저)
http://localhost or http://127.0.0.1 

# 컨테이너 가동후 my_nginx 에 접속 정보 확인 
# cat /etc/nginx/conf.d/default.conf
# 접속
docker exec -it my_nginx bash
---
# nginx의 구성 파일 (기존 내용 수정, 혹은 추가 생성 방식 취한다)
# - 가상 호스트 처리 (1개의 IP에서 포트롤 나눠서 여러개의 도메인 연결 호스팅)
# - 프록시 처리
#   - 요청 -> nginx(정적데이터처리) + springboot(동적처리)
#   - was 서버가 웹서버 만큼의 포퍼먼스를 부여되지는 않음(용도가 다름)
root@c78e4821a341:/# cat /etc/nginx/conf.d/default.conf
server {
    listen       80;
    ...

exit
---

# nginx 로그 확인
docker logs my_nginx

# 5-4. 자바
docker run -itd --name my_jdk17 openjdk:17
# bash 셀을 이용하여 접속
docker exec -it my_jdk17 bash
---
bash-4.4# java -version
openjdk version "17.0.2" 2022-01-18
OpenJDK Runtime Environment (build 17.0.2+8-86)
OpenJDK 64-Bit Server VM (build 17.0.2+8-86, mixed mode, sharing)
bash-4.4# exit
exit
---
# 향후, 해당 컨테이너에 빌드된 *.jar 배포, 
# 컨테이너 가동시 java -jar *.jar 자동 수행 -> sb 가동되서 컨테이너 가동

# 5-5. mysql 설치
# 기존에 가동중인 컨테이너 중에 mysql 있으면 중단 -> 포트 충돌남
# dev 유저 생성
# --env MYSQL_USER=dev --env MYSQL_PASSWORD=1234 : 환경변수, 여기서는 사용자 계정 1개 생성
# --env MYSQL_ROOT_PASSWORD=1234 : root 계정의 비번 설정
docker run -d -p 3306:3306 --name my_mysql --env MYSQL_USER=dev --env MYSQL_PASSWORD=1234 --env MYSQL_ROOT_PASSWORD=1234 mysql
# 접속
docker exec -it my_mysql bash
---
# root 계정으로 로그인
bash-5.1# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
...
mysql>

show databases;
use mysql;
show tables;
desc user;
select Host, User from user;

# 환경변수 입력 => 계정 생성 확인
+-----------+------------------+
| Host      | User             |
+-----------+------------------+
| %         | dev              |
| %         | root             |
...
+-----------+------------------+
---

# 5-6. 컨테이너 목록 조회
# 구동중인 전체 컨테이너 목록 조회
docker container ls -> 목록
docker container ps -> 구동중(시간 차를 두고 구동 중단중인 컨테이너 차이가 날 수 있음)

# 6. 도움말 (--help)
# docker container --help
```


## 도커 네트워크
```
# 여러 개의 컨테이너들 연결
# 3-tier(web(nginx:react), was(springboot), db(mysql)) 연동 -> 동일 네트워크 구성 (상호간 통신 가능)
# net1 : web, was
# net2 : was, db

# 7. 네트워크 
docker network ls

NETWORK ID     NAME      DRIVER    SCOPE
89a8abf6a89d   bridge    bridge    local <- basic network
1ba2ff523dd7   host      host      local
2a6403eff04b   none      null      local

# 7-1. 네트워크 생성
docker network create --driver=bridge web

# 7-2. 네트워크 목록 조회 
docker network ls
# filtering
docker network ls --filter driver=bridge

# 7-3. 컨테이너 <- 웹 네트워크 연결
docker network connect web my_jdk17

# 7-4. information check
docker container inspect my_jdk17

# 7-5. 네트워크 연결 종료
docker network disconnect web my_jdk17

# 7-6. 네트워크 제거
docker network rm web
```

## 도커 볼륨
```
# 특징
# - 컨테이너의 생애주기와 별개로 운영
# - 호스트 OS(PC)등에 별도로 위치, 공유 가능
# - 여러 컨테이너가 공유 가능
# - 저장공간 !!
# 종류
# - 볼륨 마운트
#   - 도커 엔진 내부에 저장, 리눅스 경로를 따름
# - 바인드 마운트
#   - 호스트 OS 경로를 따름(컨테이너 내부의 데이터를 호스트 OS 엑세스 가능)
# - 메모리 마운트
#   - 호스트 OS가 리눅스 일때만 가능

# 8. 볼륨 마운트
# 8-2. 목록 확인
docker volume ls

# 8-2. 볼륨 생성
docker volume create my_vol

# 8-3. 볼륨 상세 정보
# 도커 클라이언트 삭제 전에는 계속 남아있다!!
docker volume inspect my_vol

# 8-4. 컨테이너 생성시, 볼륨 적용 필요, 생성 후 적용 불가
# -v <볼륨명>:<컨테이너상 특정 위치>:[읽기 전용(ro) 등 모드 지정]
# -v my_vol:/home
docker run -itd --name my_ub -v my_vol:/home ubuntu 
# 접속
docker exec -it my_ub bash
---
root@bb3b866a57f7:/# pwd
/
root@bb3b866a57f7:/# cd home
root@bb3b866a57f7:/home# ls
ubuntu
root@bb3b866a57f7:/home# ls -al
total 12
drwxr-xr-x 3 root   root   4096 Jan 21 05:52 .
drwxr-xr-x 1 root   root   4096 Jan 21 05:52 ..
drwxr-x--- 2 ubuntu ubuntu 4096 Jan 21 05:52 ubuntu
root@bb3b866a57f7:/home# echo "hi" > test.txt
root@bb3b866a57f7:/home# ls
test.txt  ubuntu
root@bb3b866a57f7:/home# cat test.txt
hi 
---

# 8-5. 컨테이너 2개 생성. 볼륨 마운트 공유
# my_vol을 볼륨 마운트 지정 -> 특정 컨테이너에서 파일 생성 -> 다른 컨테이너에서도 공유됨
docker run -itd --name my_ub2 -v my_vol:/home ubuntu
docker run -itd --name my_ub3 -v my_vol:/home ubuntu 
# my_ub2 접속 
docker exec -it my_ub2 bash
---
# 다른 컨테이너에서 만든 파일이 확인(공유됨)
root@e2b4312c31f8:/# ls /home
test.txt  ubuntu
root@e2b4312c31f8:/# cat /home/test.txt
hi
root@e2b4312c31f8:/# echo "hello" > /home/test2.txt
root@e2b4312c31f8:/# ls /home
test.txt  test2.txt  ubuntu
root@e2b4312c31f8:/# exit
exit
---

# 8-6. 읽기 전용 -> ro
docker run -itd --name my_ub4 -v my_vol:/home:ro ubuntu 
---
# cd home
# pwd
/home
# echo "1234" > t.txt
/bin/sh: 3: cannot create t.txt: Read-only file system
---

# 9. 바인드 마운트
# 호스트 OS의 특정 경로를 볼륨으로 사용
# -v <호스트OS의 경로>:<컨테이너 경로>:[권한]
# <호스트OS의 경로>법
# bash shell (맥) => "$(pwd)"
# power shell (윈) => "${pwd}"
docker run -itd --name my_ub5 -v ${pwd}:/home ubuntu
# 접속
docker exec -it my_ub5 bash
docker cli end
```

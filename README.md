# Docker 정리 자료
[초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)과 도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문(야마다 아키노리)를 정리하여 만들었습니다.

Table of contents
=================
<!--ts-->
   * [Docker](#Docker)
   * [Container](#Container)
   * [Why_Popular](#Why_Popular)
   * [Installation](#Installation)
   * [Run](#Run)
   * [Command](#Command)
   * [Docker_compose](#Docker_compose)
<!--te-->

Docker
=======
* `도커`: 컨테이너 기반의 가상화 오픈소스 플랫폼
* 기능: 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해준다.


Container
=======
* `컨테이너`: 컨테이너는 격리된 공간에서 `프로세스`가 동작하는 기술
* 가상머신(OS 가상화)과 도커(컨테이너)의 차이: 기존의 가상머신들은 0S 자체를 가상화하여 호스트 OS 위에 게스트 OS 전체를 가상화하여 사용하였으나 컨테이너는 전체 OS를 가상화하는 방식이 아니기 때문에 가볍고 빠르다.
* `이미지`: 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않는다. 컨테이너는 이미지를 실행한 상태이다. `ex) Ubuntu, MySQL, Go, Redis, Nginx....`


Why_Popular
=======
* `레이어 저장 방식`: 유니온 파일 시스템을 이용하여 여러 개의 레이어를 하나의 파일시스템으로 사용하여 이미지 파일이 추가되거나 수정되면 새로운 레이어가 생성되고 기존의 레이어를 제외한 내용만 새로 디운로드 받기 때문에 효율적으로 이미지 관리를 할 수 있다.
* `DockerFile`: 자체 DSL(Domain Specific Language)언어를 이용하여 이미지 생성 과정을 적는 DockerFile을 통해서 프로그램 설치 및 설정 과정을 간단하게 하고 쉽게 관리할 수 있다.
* `Docker Hub`: 큰 용량의 이미지를 서버에 저장하고 관리하는 것은 쉽지 않은데 도커는 Docker Hub를 통해 공개 이미지를 무료로 관리해주는 서비스를 제공한다.
* `Command와 API`: 도커의 대부분의 명령어는 직관적이고 사용하기 쉬우며 REST API도 지원하여 확장성이 좋다.

Installation
=======
* 리눅스에서 도커 설치하기
  * Docker Script를 이용한 설치 
  ```sh
  $ curl -fsSL https://get.docker.com/ | sudo sh
  ```
  * Docker 설치 확인
  ```sh
  $ docker version
  ```
* 도커 실행 시의 특징: 도커는 하나의 실행파일이지만 실제로 클라이언트와 서버역할을 각각 할 수 있다. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터미널에 출력한다.

Run
=======
* 컨테이너 실행하기 
  ```sh
  $ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```
* 컨테이너 실행 옵션
| 옵션 | 설명 |
|:--------|:--------|
| -d | detached mode 흔히 말하는 백그라운드 모드 |
| -p | 호스트와 컨테이너의 포트를 연결 (포워딩) |
| -v | 호스트와 컨테이너의 디렉토리를 연결 (마운트) |
| -e | 컨테이너 내에서 사용할 환경변수 설정 |
| -name | 컨테이너 이름 설정 |
| -rm | 프로세스 종료시 컨테이너 자동 제거 |
| -it | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| -link | 컨테이너 연결 [컨테이너명:별칭] |
* 우분투 컨테이너 실행하기
  * 컨테이너 실행하기
  ```sh
  $ sudo docker run ubuntu:16.04
  ```
  * 실행 프로세스가 전달된 컨테이너 실행
  ```sh
  $ sudo docker run --rm -it ubuntu:16.04 /bin/bash
  ```
* Redis 컨테이너 실행하기
  * 컨테이너 실행하기
  ```sh
  $ sudo docker run -d -p 1234:6379 redis
  ```
* MySQL 컨테이너 실행하기
  * 컨테이너 실행하기
  ```sh
  $ docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
  ```

Command
=======
* 컨테이너 목록 확인하기
  * 컨테이너 확인하기
  ```sh
  $ docker ps [OPTIONS]
  ```
  * 실행 중인 모든 컨테이너 확인하기
  ```sh
  $ docker ps -a
  ```
* 컨테이너 중지하기
  ```sh
  $ docker stop [OPTIONS] CONTAINER [CONTAINER...]
  ```
* 컨테이너 제거하기
  ```sh
  $ docker rm [OPTIONS] CONTAINER [CONTAINER...]
  ```
* 다운로드된 이미지 목록 확인하기
  ```sh
  $ docker images [OPTIONS] [REPOSITORY[:TAG]]
  ```
* 이미지 다운로드하기
  ```sh
  $ docker pull [OPTIONS] NAME[:TAG|@DIGEST]
  ```
* 이미지 삭제하기
   ```sh
   $ docker rmi [OPTIONS] IMAGE [IMAGE...]
   ```
* 컨테이너 로그 확인
  ```sh
  $ docker logs [OPTIONS] CONTAINER
  ```
* 구동 중인 컨테이너에 명령어 실행하기
  ```sh
  $ docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  ```

Docker_Compose
=======

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
* `컨테이너`: 컨테이너는 격리된 공간에서 프로세스가 동작하는 기술
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
  * 

Run
=======

Command
=======

Docker_Compose
=======
# Docker 정리 자료
[초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)과 도커/쿠버네티스를 활용한 컨테이너 개발 실전 입문(야마다 아키노리)를 정리하여 만들었습니다.

Table of contents
=================
<!--ts-->
   * [Docker](#Docker)
   * [Container](#Container)
   * [Why Popular](#Why-Popular)
   * [Installation](#Installation)
   * [Run](#Run)
   * [Command](#Command)
   * [Docker Compose](#Docker-Compose)
   * [Making Image](#Making-Image)
   * [Docker Hub](#Docker-Hub)
   * [Deployment](#Deployment)
   * [Docker API](#Docker-API)
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


Why Popular
=======
* `레이어 저장 방식`: 유니온 파일 시스템을 이용하여 여러 개의 레이어를 하나의 파일시스템으로 사용하여 이미지 파일이 추가되거나 수정되면 새로운 레이어가 생성되고 기존의 레이어를 제외한 내용만 새로 디운로드 받기 때문에 효율적으로 이미지 관리를 할 수 있다.
* `Dockerfile`: 자체 DSL(Domain Specific Language)언어를 이용하여 이미지 생성 과정을 적는 Dockerfile을 통해서 프로그램 설치 및 설정 과정을 간단하게 하고 쉽게 관리할 수 있다.
* `Docker Hub`: 큰 용량의 이미지를 서버에 저장하고 관리하는 것은 쉽지 않은데 도커는 Docker Hub를 통해 공개 이미지를 무료로 관리해주는 서비스를 제공한다.
* `Command와 API`: 도커의 대부분의 명령어는 직관적이고 사용하기 쉬우며 `Docker REST API`도 지원하여 확장성이 좋다.
* `배포 효율성`: 명령어를 통해 쉽게 프로그램을 배포 확장할 수 있고 실행 시점에 상관없이 구성 시점을 채택할 수 있다.
* `성능`: 이미지 용량이 크게 줄어든다.

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
  * Docker에 sudo 권한 부여
  ```sh
  $ sudo setfacl -m user:$USER:rw /var/run/docker.sock
  ```
* 도커 실행 시의 특징: 도커는 하나의 실행파일이지만 실제로 클라이언트와 서버역할을 각각 할 수 있다. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터미널에 출력하는 구조이다.

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
  * 실행 프로세스(Bash)가 전달된 컨테이너 실행
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
* 컨테이너 실행하기
  ```sh
  $ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
  ```
* 컨테이너 중지하기
  ```sh
  $ docker stop [OPTIONS] CONTAINER [CONTAINER...]
  ```
* 컨테이너 제거하기
  ```sh
  $ docker rm [OPTIONS] CONTAINER [CONTAINER...]
  ```
* 이미지 빌드하기
  ```sh
  $ docker build [OPTIONS] PATH | URL | -
  ```
* 이미지 태깅하기
  ```sh
  $ docker tag  [IMAGE>[:TAG]] [Docker REGISTRY URL]/[IMAGE[:태그]
  ```
* 이미지 업로드하기
  ```sh
  $ docker push [Docker REGISTRY URL]/[IMAGE[:TAG]]
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
* 이미지 내역 확인하기
  ```sh
  $ docker image history IMAGE
  ```
* 컨테이너 로그 확인
  ```sh
  $ docker logs [OPTIONS] CONTAINER
  ```
* 구동 중인 컨테이너에 명령어 실행하기
  ```sh
  $ docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
  ```
* 구동 중인 컨테이너에 지속적 명령을 위해 접속하기
  ```sh
  $ docker exec -it CONTAINER /bin/bash
  ```

Docker Compose
=======
* `Docker Compose`: 컨테이너의 복잡한 설정을 쉽게 관리하기 위해 YAML방식의 설정파일을 이용하는 툴
* Docker Compose 설치하기
  * 설치하기
  ```sh
  $ curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  ```
  * 권한설정
  ```sh
  $ chmod +x /usr/local/bin/docker-compose
  ```
  * 설치확인
  ```sh
  $ docker-compose version
  ``` 
* docker-compose.yml 파일을 통해 Wordpress 컨테이너 만들기
  * docker-compose.yml 파일 정의
  ```yml
      version: '2'
      services:
      db:
      image: mysql:5.7
      volumes:
            - db_data:/var/lib/mysql
      restart: always
      environment:
            MYSQL_ROOT_PASSWORD: wordpress
            MYSQL_DATABASE: wordpress
            MYSQL_USER: wordpress
            MYSQL_PASSWORD: wordpress

      wordpress:
      depends_on:
            - db
      image: wordpress:latest
      volumes:
            - wp_data:/var/www/html
      ports:
            - "8000:80"
      restart: always
      environment:
            WORDPRESS_DB_HOST: db:3306
            WORDPRESS_DB_PASSWORD: wordpress
      volumes:
      db_data:
      wp_data:
  ```
  * 실행하기
  ```sh
  $ docker-compose up
  ```

Making Image
=======
* 도커 이미지 만들기: 도커는 가상머신의 스냅샷과 같은 방식으로 애플리케이션을 설치하고 그 상태로 이미지를 저장한다.
* Sinatra 애플리케이션 `Dockerizing`
  * Ruby 패키지 파일 생성
  ```sh
  $ vi Gemfile
  ```
  ```yml
  source 'https://rubygems.org'
  gem 'sinatra'
  ```
  * Ruby 소스 파일 생성
  ```sh
  $ vi arr.rb
  ```
  ```ruby
  require 'sinatra'
  require 'socket'
  get '/' do
    Socket.gethostname
  end
  ```
  * 도커를 활용하여 실행
  ```sh
  $ docker run --rm -p 4567:4567 -v $PWD:/usr/src/app -w /usr/src/app ruby bash -c "bundle install && bundle exec ruby app.rb -o 0.0.0.0"
  ```
  * Dockerfile(빌드 파일) 생성
  ```yml
  # 1. ubuntu 설치 (패키지 업데이트 + 만든사람 표시)
  FROM       ubuntu:16.04
  MAINTAINER subicura@subicura.com
  RUN        apt-get -y update

  # 2. ruby 설치
  RUN apt-get -y install ruby
  RUN gem install bundler

  # 3. 소스 복사
  COPY . /usr/src/app

  # 4. Gem 패키지 설치 (실행 디렉토리 설정)
  WORKDIR /usr/src/app
  RUN     bundle install

  # 5. Sinatra 서버 실행 (Listen 포트 정의)
  EXPOSE 4567
  CMD    bundle exec ruby app.rb -o 0.0.0.0
  ```
  또는
  ```yml
  FROM ruby:2.3
  MAINTAINER subicura@subicura.com
  COPY Gemfile* /usr/src/app/
  WORKDIR /usr/src/app
  RUN bundle install --no-rdoc --no-ri
  COPY . /usr/src/app
  EXPOSE 4567
  CMD bundle exec ruby app.rb -o 0.0.0.0
  ```
  * 이미지 빌드하기
  ```sh
  $ docker build -t app .
  ```
  * 이미지 확인하기
  ```sh
  $ docker images
  ```
* Dockerfile 기본 명령어
  * FROM: 베이스 이미지를 저장
  ```yml
  FROM <image>:<tag>
  ```
  ```yml
  FROM ubuntu:16.04
  ```
  * MAINTAINER: Dockerfile을 관리하는 사람의 정보를 저장 
  ```yml
  MAINTAINER <name>
  ```
  ```yml
  MAINTAINER subicura@subicura.com
  ```
  * COPY: 파일이나 디렉토리를 이미지로 복사
  ```yml
  COPY <src>... <dest>
  ```
  ```yml
  COPY . /usr/src/app
  ```  
  * ADD: COPY와 유사하나 `src`에 파일 대신 URL을 입력할 수 있음 
  ```yml
  ADD <src>... <dest>
  ```
  ```yml
  ADD . /usr/src/app
  ``` 
  * RUN: 명령을 실행
  ```yml
  RUN <command>
  RUN ["executable", "param1", "param2"]
  ```
  ```yml
  RUN bundle install
  ``` 
  * CMD: 도커 컨테이너가 구동됐을 때 실행할 명령어를 정의
  ```yml
  CMD ["executable","param1","param2"]
  CMD <command> <param1> <param2>
  ```
  ```yml
  CMD bundle exec ruby app.rb
  ``` 
  * WORKDIR: RUN, CMD, ADD, COPY등이 이루어질 기본 디렉토리를 설정
  ```yml
  WORKDIR <path>
  ```
  ```yml
  WORKDIR /path/to/workdir
  ```
  * EXPOSE: 도커 컨테이너가 실행되었을 때 요청을 기다리고 있는(Listen) 포트를 지정
  ```yml
  EXPOSE <port> [<port>...]
  ```
  ```yml
  EXPOSE 4567
  ``` 
  * VOLUME: 컨테이너 외부에 파일시스템을 마운트 할 때 사용
  ```yml
  VOLUME <path>
  ```
  ```
  VOLUME ["/data"]
  ``` 
  * ENV: 컨테이너에서 사용할 환경변수를 지정
  ```yml
  ENV <key> <value>
  ENV <key>=<value> ...
  ```
  ```yml
  ENV DB_URL mysql
  ``` 

Docker Hub
=======
* `도커 레지스트리`: 도커는 빌드한 이미지를 서버에 배포하기 위해 직접 파일을 복사하는 방법 대신 도커 레지스트리 `Docker Registry`라는 이미지 저장소를 사용하여 push와 pull하는 구조이다. `Docker Registry`를 사용하기 싫다면 `Docker Hub`를 사용할 수 있다.
* `Docker Hub`: 도커에서 제공하는 기본 이미지 저장소로 대용량의 이미지를 무료로 저장 및 다운로드 할 수 있다. 단, 기본적으로 모든 이미지는 공개되어 누구나 접근 가능하므로 비공개로 사용하려면 유료 서비스를 이용해야한다.
* Docker Hub 사용하기
  * 도커 허브 페이지에서 회원 가입을 하고 허브 계정을 사용하기
  ```sh
  $ docker login
  ```
  * 도커 이미지 태깅하기
  ```
  $ docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
  ```
  cf) 도커 이미지 구조: `[사용자 ID]/[이미지명]:[tag]`
  * 도커 허브에 이미지를 전송
  ```sh
  $ docker push IMAGE[:TAG]
  ```

Deployment
=======
* 도커 배포하기
  * 서버 환경에서 등록된 이미지를 다운받고 컨테이너를 실행
* 도커 컨테이너 업데이트하기
  * 최신 이미지를 기반으로 새 컨테이너를 만들고 이전 컨테이너를 삭제


Docker API
=======
* 컨테이너 리스트 얻기: `GET /containers/json`
* 컨테이너 생성: `POST /containers/create` + Request Body
* 컨테이너 시작: `POST /containers/{id}/start`
* 컨테이너 정지: `POST /containers/{id}/stop`
* 컨테이너 재시작: `POST /containers/{id}/restart`
* 컨테이너 종료: `POST /containers/{id}/kill`
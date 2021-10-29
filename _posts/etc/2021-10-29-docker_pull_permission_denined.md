---
title:  "Docker Troubleshooting"
excerpt: docker 이용 시 문제 상황 해결 기록하기

categories:
  - 'docker'
tags:
  - docker
  - Troubleshooting

toc: true
toc_sticky: true

date: 2021-10-26
last_modified_at: 2021-10-29

---


# docker? 

* Linux 기반의 container runtime opensource
* VM과 유사하지만 더 가벼운 형태로 배포 가능
* 다양한 프로그램 또는 실행 환경 (window, linux etc..)을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 배포 및 관리를 단순화
* Image와 Container로 구성됨
  + Image : 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있음, 상태값이 없으며 변하지 않음
  + > Build and Run
  + Container : 어플리케이션이 동작하기 위해 필요한 요소를 패키지화하고 격리하는 기술

# docker 실행 기본 명령어

* docker 서비스 실행

```
sudo service docker start
```

* booting 시 자동 실행

```
sudo chkconfig docker on
```

# docker command

* docker 이미지 받아오기

```
sudo docker pull <image name>
```

* docker의 container를 실행

```
sudo docker run <-d|-it|--name|-e|-p|-v|--rm> <image name> <run file name>
```

* docker container 목록 출력

```
sudo docker ps <-a>
```

* docker container 시작 / 재시작 / 접속 / 정지 / 삭제

```
sudo docker start/restart/attach/stop/rm <contanier name|container id>
```

* docker image 삭제

```
sudo docker rmi <image name>
```

# Troubleshooting

## docker pull image 실행 시, docker.sock: permission denied

* docker.sock
  + docker deamon이 사용하는 unix domain socket
  + console에서 docker 명령어를 사용할 때, docker CLI가 명령어를 받아 docker.sock UDS를 이용하여 deamon에 전달하기 위해 사용

* 문제 발생 시 출력되는 메시지

```
pi ~/Workspace/test master] docker pull <image name>
Using default tag: latest
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.0/images/create?fromImage=<image name>&tag=latest: dial unix /var/run/docker.sock: connect: permission denied
```

* 해결 방법

```
pi ~/Workspace/test master] sudo chmod 666 /var/run/docker.sock
[sudo] pi의 암호:
pi ~/Workspace/test master] docker pull <image name>
Using default tag: latest
latest: Pulling from <name>
Digest: sah256:305b76a7e479f7bea11068ecff0245231568abacaefd7777a123000298defab
Status: Downloaded newer image for <image name>:latest
<image-name>:latest
```
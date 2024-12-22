---
{"dg-publish":true,"permalink":"/learning-notes/docker/docker/","created":"2024-12-19T14:27:32.263+09:00","updated":"2024-12-23T00:57:34.355+09:00"}
---

## 도커 설명
- 도커 소개
    - 등장배경(등장 이전)
        - 개발 환경과 배포 환경이 다른 경우, 환경 설정이 문제가 됨
            - 서버 환경까지도 모두 한번에 소프트웨어화 하기 위해서 가상화 사용
        - VM을 많이 사용했었음
            - 무거움
    - 소개
        - 도커 이미지
            - 컨테이너를 실행할 때 사용할 수 있는 템플릿
        - 도커 컨테이너
            - 이미지로 실행된 인스턴스
    - 도커로 할 수 있는 일
        - MySQL, Jupyter Notebook 등을 도커로 실행 가능
        - 이미지로 다른 사람에게 공유 가능
        - 원격 저장소에 이미지를 넣고 배포하는 방식으로 서버에 배포하기도 함
        - DockerHub에서 공개된 모든 이미지를 다운로드 받을 수 있음

## 도커 사용 방법 및 명령어 정리
- 도커 데스크탑 다운로드
	- 도커 공식 홈페이지에서 환경에 맞는 도커 데스크탑을 다운로드 받는다.
	- WSL2와 연동되어 사용하기 매우 편함
- 도커 이미지 다운로드
```
docker pull mysql:8
# MySQL 8버젼의 이미지를 도커로 다운로드한다
```
- 다운 받은 도커 이미지들 확인
```
docker images

# REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
# mysql        8         106d5197fd8e   2 months ago   816MB
```
- 다운받은 도커 이미지 삭제
```
docker rmi 106d5197fd8e # IMAGE ID
```
- 도커 이미지를 사용해 도커 컨테이너를 만들고 실행하기
```
docker run --name mysql-tutorial -e MYSQL_ROOT_PASSWORD=1234 -d -p 3306:3306 mysql:8
```
	- --name : 컨테이너의 이름 설정
	- -e : 환경변수 설정, 비밀번호를 설정함
	- -d : 컨테이너를 백그라운드 형태로 실행, 없으면 현재 셸 위에 컨테이너가 실행되게 됨
	- -p : 포트 지정, 로컬 호스트 포트:컨테이너 포트 형태, 로컬 호스트 포트로 접근 시 컨테이너의 포트 번호로 접근하는 것과 같게 됨
	- mysql:8 : 이미지 이름과 태그
- 실행중인 컨테이너 확인
```
docker ps
# 뒤에 -a를 붙이면 작동을 멈춘 컨테이너들까지 전부 보여줌(all)
```
- 도커 컨테이너로 진입
```
docker exec -it mysql-tutorial /bin/bash

# mysql-tutorial : 자신의 컨테이너 이름(혹은 ID)
# Ctrl + Q : 컨테이너 나가기
```
- 도커 컨테이너 중지
```
docker stop mysql-tutorial
```
- 도커 컨테이너 삭제
```
docker rm mysql-tutorial

# mysql-tutorial : 자신의 컨테이너 이름(혹은 ID)
# 단, 멈춘 컨테이너만 삭제 가능
# 뒤에 -f를 붙이면 실행중인 컨테이너도 삭제 가능
```

## 도커 사용 시 주의사항
- 도커는 가상 환경이기 떄문에 컨테이너를 삭제하면 내부의 데이터나 파일들이 같이 사라지게 됨
	- 만약 파일을 유지하고 싶다면 로컬의 저장소와 Volume Mount 시켜야함
	- run 시킬 때 -v 옵션을 사용하여 Port처럼 사용함. -v Host_Folder:Container_Folder
```
docker run -it -p 8888:8888 -v /some/host/folder:/home/docker/workspace mysql:8
```

## 도커 이미지 만들기
- 폴더(02-docker)를 만들고 폴더에 poetry 세팅과 torch 관련 패키지들을 설치함
- 간단히 모델을 학습시키는 python 파일을 만듬
- Dockerfile 생성 (도커 이미지를 빌드하기 위한 정보를 저장)
- FROM "이미지 이름:태그"
	- 이미지 빌드할 때 사용할 베이스 이미지를 지정
	- 이미 만들어진 베이스 이미지로부터 새로운 설정을 추가함
```
FROM pytorch/pytorch:1.13.1-cuda11.6-cudnn8-runtime
```
- COPY "로컬 디렉토르(파일)" "컨테이너 내 디렉토리(파일)"
	- 로컬 디렉토리 내 파일을 컨테이너 내 디렉토리로 복사함
```
COPY . /app
# 로컬의 현재 폴더를 컨테이너의 /app 폴더로 복사하겠다는 의미
```
- WORKDIR "컨테이너 내 디렉토리"
	- Dockerfile의 RUN, CMD, ENTRYPOINT 등의 명령어를 실행할 컨테이너 경로 지정
```
WORKDIR /app
# 이 라인 뒤에 존재하는 RUN, CMD 명령어는 모두 컨테이너 내부의 /app 경로에서 실행한다는 의미
```
- ENV "환경변수 이름=값"
	- 컨테이너 내 환경변수를 지정
```
ENV PYTHONPATH=/app
ENV PYTHONDUFFERED=1
# 보통 파이썬 애플리케이션은 위의 두 값을 지정함
```
- RUN "실행할 리눅스 명령어"
	- 컨테이너 안에서 리눅스 명령어를 실행
	- 여러 개를 실행해야 할 경우 && \을 통해 이어줌
```
RUN pip install pip==23.0.1 && \
	pip install poetry==1.2.1 && \
	poetry export -o requirements.txt && \
	pip install -r requirements.txt
```
- CMD ["실행할 명령어", "인자", ...]
	- 이 이미지를 기반으로 나중에 컨테이너를 만들 때 실행되는 명령어
	- 띄어쓰기가 없고 아래처럼 사용함
```
CMD ["python", "main.py"]
```
- EXPOSE : 컨테이너 외부에 노출할 포트 지정
- ENTRYPOINT : 이미지를 컨테이너로 띄울 때 항상 실행하는 커맨드
	- CMD : 실행 시점에 오버라이딩 가능
	- ENTRYPOINT : 오버라이딩 불가능, 보안적인 면에서 더 안전 

- 도커 이미지 빌드
	- 태그 미 지정시 "latest"로 채워짐
```
docker build -t 02-docker:latest .
# 02-docker : 빌드할 이미지 이름
# latest : 빌드할 이미지의 태그
# . : 현재 폴더를 의미
```

- 도커 이미지 컨테이너 실행
```
docker run 02-docker:latest
```

## 레지스트리에 도커 이미지 푸시
- 컨테이너 레지스트리 : Dockerhub, GCP GCP, AWS ECR 등
	- 지정하지 않으면 기본적으로 Dockerhub 사용

- 도커 허브 계정 생성
- CLI에 도커 계정 연동
```
docker login
```
- doker tag 설정
	- docker tag "기존 이미지:태그" "새 이미지 이름:태그"
	- 새 이미지 이름은 내 계정 ID/이미지 이름
```
docker tag 02-docker:latest xogks5479/02-docker:latest
```
- docker push
	- docker push "이미지 이름:태그"
```
docker push xogks5479/02-docker:latest
```
- docker pull로 어디서든 다운로드 받을 수 있음
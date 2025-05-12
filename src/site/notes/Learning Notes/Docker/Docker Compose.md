---
{"dg-publish":true,"permalink":"/learning-notes/docker/docker-compose/","created":"2024-12-23T02:17:52.644+09:00","updated":"2025-02-23T16:00:03.923+09:00"}
---

## Docker Compose가 필요한 이유
- 하나의 Docker 이미지가 아니라 여러 Docker 이미지를 동시에 실행하고 싶은 경우
- A image로 Container를 띄우고, 그 이후에 B Container를 실행해야만 하는 경우: 의존성 문제
- docker run할 때 다양한 옵션들을 다르게 적용해야 하는 경우

## Docker Compose 사용
- 여러 컨테이너를 한번에 실행할 수 있음
- 여러 컨테이너의 실행 순서, 의존도를 관리할 수 있음
- docker-compose.yml 파일에 작성

## Docker Compose 파일 만들기
- version : Docker Compose 버전
- services : 실행할 컨테이너 정의. 각 서비스는 하나의 컨테이너로 세부 설정을 저장
	- container1 : 컨테이너 명
		- image : 이미지 명시
		- environment : 환경 변수
		- port : 포트 설정
	- container2 : 컨테이너 명
		- depends_on : 명시된 서비스(컨테이너)가 정상적으로 동작한 이후에 실행
		- restart : 컨테이너 재실행 정책
		- volumes : 호스트와 컨테이너의 저장소를 지정
		- secrets : 보안이 필요한 데이터 전달
		- configs : 컨테이너에 사용할 config 파일
		- command : 컨테이너가 시작될 때 실행할 명령 지정

## Docker Compose 파일 실행하기
- 일괄 실행
	- 필요한 이미지를 pull 하거나 build 하는 과정도 포함
	- 옵션
		- -d : 백그라운드에서 실행
```
docker-compose up

# 서비스 중단
docker-compose down

# 로그 확인
docker-compose logs <서비스명>
```
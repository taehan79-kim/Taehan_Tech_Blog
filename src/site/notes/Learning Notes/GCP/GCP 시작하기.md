---
{"dg-publish":true,"permalink":"/learning-notes/gcp/gcp/","created":"2024-12-23T09:58:22.387+09:00","updated":"2025-02-23T18:36:39.761+09:00"}
---

## GCP 목차
- Compute Engine : 서버 띄우기
- 방화벽 설정
- 서버에 주피터 랩 설정
- Service Account(서비스 계정)
- Cloud Storage => Object Storage
- Composer(Managed Airflow)

### 시작 전 클라우드 서비스 제품 탐색
- 클라우드 서비스 제품 알아보기
	- 구글에 google cloud component 검색 후 이미지 탐색
	- ![Pasted_image_20241223150519.png](/img/user/Pasted_image_20241223150519.png)

### Compute Engine 서버 띄우기
- 새 VM 인스턴스 만들기를 통해서 생성해도 되지만, Starage의 이미지에서 인스턴스 만들기를 하면 다른 사람이 만들어 놓은 이미지를 통해 쉽게 인스턴스를 생성할 수 있음
- 방화벽에 HTTP 트래픽 허용, HTTPS 트래픽을 허용하고 만듬

### 방화벽 설정
- VPC 네트워크 - 방화벽
- 방화벽 규칙 만들기
- 태그 설정 후 해당 태그를 인스턴스 방화벽 설정에 수정하여 연결함

### 서버에 주피터 랩 설정
- 서버에 주피터 랩 설치
```
pip install jupyterlab
```
- 패스워드 설정 ipython3로 진입
```
from notebook.auth import passwd
passwd()
```
- 주피터 랩 설정을 위한 config 파일 설정(CLI)
```
jupyter lab --generate-config
```
- config 설정 파일 수정
	- i로 insert 진입 후 수정
```
vi ~/.jupyter/jupyter_lab_config.py
```
```
c = get_config()
c.JupyterApp.config_file_name = 'jupyter_lab_config.py'
c.NotebookApp.allow_origin = '*'
c.NotebookApp.ip='내부 포트'
c.NotebookApp.open_browser=False
c.NotevookApp.password = 'ipython에서 나온 passwd 텍스트'
```
- 주피터 랩 실행
```
jypyter lab --ip=0.0.0.0 --port 8888
```
- 단, 현재는 ssh 연결을 끊으면 jupyter lab도 연결이 끊기는 문제가 있음
- 백그라운드에서 실행하게 하여 ssh 연결을 하지 않아도 계속 jupyter lab을 킬 수 있게 만듬
```
nohup jupyter lab --ip=0.0.0.0 --port 8888 --no-browser --allow-root 1>/dev/null 2>&1 &
```
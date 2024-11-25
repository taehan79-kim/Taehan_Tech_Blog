---
{"dg-publish":true,"permalink":"/projects/ocr-data-centric/","created":"2024-11-25T11:09:06.427+09:00","updated":"2024-11-25T11:48:05.388+09:00"}
---

## 프로젝트 개요
- 2024.10.28 ~ 2024.11.7
- 영수증 글자 인식을 위한 OCR 대회
- Data-Centric AI 관점 대회
- Naver Connect & Upstage 주관 대회
## 대회 소개
카메라로 영수증을 인식할 경우 자동으로 영수증 내용이 입력되는 어플리케이션이 있습니다. 이처럼 **OCR** (Optical Character Recognition) 기술은 사람이 직접 쓰거나 이미지 속에 있는 문자를 얻은 다음 이를 컴퓨터가 인식할 수 있도록 하는 기술로, 컴퓨터 비전 분야에서 현재 널리 쓰이는 대표적인 기술 중 하나입니다.

본 대회는 **Data-Centric AI**의 관점에서 모델 활용을 경쟁하는 대회입니다. 이에 따라 제공되는 베이스라인 코드 중 모델 관련 부분을 변경하는 것이 금지되어 있습니다. 보통의 대회에서는 AI 모델의 구조나 기술에 집중하지만 모델만큼이나 중요한 **데이터의 관점**에서 (데이터 정제, 제작, 성능 평가, 후처리 등) 다양한 방식으로 모델의 성능을 향상시키는 대회입니다. 
## 개발 환경
- Language : Python
- Environment
	- CPU : Intel(R) Xeon(R) Gold 5120
	- GPU : Tesla V100-SXM2 32GB x 1
- Framework : PyTorch
- Collaborative Tool : GitHub, Tensorboard, Notion
## Leaderboard
![Pasted image 20241125111946.png](/img/user/Pasted%20image%2020241125111946.png)
	이번 대회에서는 10등이라는 높지도 낮지도 않은 결과를 얻게 되었습니다. 하지만, 그 전 대회의 내용들보다 더욱 많이 배울 수 있는 기회가 되었습니다. 데이터 중심의 AI라는 대회의 특성 상 전처리, 데이터 제작, 외부 데이터 사용 등 다양한 가설을 설정하고 시도를 해볼 수 있었습니다. 한 가지 아쉬운 점은 대회 규정상 모델 코드 변경 관리에 대한 문제로 데이터 전처리 부분이 금지된 점이었습니다. 해당 대회에서 저는 전처리를 위주로 진행했지만 테스트셋 데이터의 전처리 작업은 금지되었기 때문에 최종 결과에 적용하지는 못했습니다. 
## 타임라인
![Pasted image 20241125114625.png](/img/user/Pasted%20image%2020241125114625.png)
## EDA

## Method

## 결과

## 개인 회고
---
{"dg-publish":true,"permalink":"/projects/hand-bone-segmentation/","created":"2025-02-16T02:25:19.021+09:00","updated":"2025-02-16T03:59:57.610+09:00"}
---

#naverboostcamp #ai_tech #computer_vision #semantic_segmentation
## 프로젝트 개요
- 2024.11.13 ~ 2024.11.28
- X-ray 이미지를 통한 Semantic Segmentation 대회
- Naver Connect & Upstage 주관 대회
- [git](https://github.com/boostcampaitech7/level2-cv-semanticsegmentation-cv-14-lv3.git)

---
## 대회 소개
![Pasted image 20250216024207.png](/img/user/Pasted%20image%2020250216024207.png)
- X-ray Hand bone 이미지를 이용해 Segmentation Task를 수행하는 모델을 개발하는 대회입니다. 
- 하나의 이미지당 29개의 class를 가지고 있고 왼손, 오른손 동일한 양의 이미지가 존재합니다.
- 데이터셋 (X-ray 이미지, 2048 x 2048)
	- Train: 800장
	- Test: 288장

---
## 개발 환경
- Language : Python
- Environment
	- CPU : Intel(R) Xeon(R) Gold 5120
	- GPU : Tesla V100-SXM2 32GB x 1
- Framework : PyTorch
- Collaborative Tool : GitHub, WandB, Notion, Slack

---
## Leaderboard
![Pasted image 20250216024409.png](/img/user/Pasted%20image%2020250216024409.png)
	이번 프로젝트에서는 새로운 팀원들과 진행을 하게 되면서 결과보다는 과정에 초점을 두고 진행을 하게 되었습니다. 비록 순위는 8위이지만 매우 많은 가설 실험을 설계, 시도하고 팀원들과 적극적으로 공유를 하면서 참여한 팀 중 가장 많은 제출 횟수를 기록하였습니다. 또한, 가장 많은 제출 횟수 답게 팀원들과 그 과정에서 많은 내용을 배울 수 있었습니다. 특히, 그중 저는 U3+Net 설계 및 2-Stage 분리 실험, CRF 후처리를 진행하면서 이론을 통해 다양한 방법론을 도출하고 이를 직접 실험하여 결과를 확인해보는 과정을 배웠습니다. 

---
## 타임라인
![Pasted image 20250216025201.png](/img/user/Pasted%20image%2020250216025201.png)

---
## 프로젝트 수행 내용
이번 프로젝트에서는 결과보다 과정을 중시하며 최대한 다양한 방법론을 체계적으로 시도하는 것을 목표로 하였습니다. 크게 아래와 같은 내용을 시도하였습니다.
- EDA
- Cross Validation Seed 설정
- Model, Augmentation, Hyperparameter, Loss Setting
- 2-Stage 분리 실험
- CRF 후처리
- 앙상블

## EDA
---
## Cross Validation Seed 설정

---
## Model, Augmentation, Hyperparameter, Loss Setting

---
## 2-Stage 분리 실험

---
## CRF 후처리

---

# 앙상블

---
## 결론

---
## 개인 회고
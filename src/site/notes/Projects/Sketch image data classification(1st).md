---
{"dg-publish":true,"permalink":"/projects/sketch-image-data-classification-1st/","created":"2024-11-09T19:00:30.487+09:00","updated":"2024-11-11T22:52:28.539+09:00"}
---

#naverboostcamp #ai_tech #computer_vision #classification
## 프로젝트 개요

- 2024.09.11 ~ 2024.09.26
- ImageNet Sketch 이미지 데이터 분류
- 1st Prize 🏆
- Naver Connect & Upstage 주관 대회
- [main code](https://github.com/boostcampaitech7/level1-imageclassification-cv-18/blob/main/main)
## 대회 소개
![image](https://github.com/user-attachments/assets/e889ae72-c64f-48bb-95f0-ce7c73d56e4c)

Sketch 이미지 분류 경진대회는 주어진 Sketch 이미지 데이터를 활용하여 모델을 제작하고 어떤 객체를 나타내는지 분류하는 대회입니다.

## Leaderboard
[![리더보드](https://github.com/user-attachments/assets/05f98560-85fb-43b7-b272-bef54f9a97e1)
	 팀원들과의 협업 및 노력으로 Sketch 이미지 분류 대회에서 **0.9390**의 private Accuracy를 달성하며 **1등**으로 마무리할 수 있었습니다. 이러한 성과의 배경에는 다양한 **가설 설정, 실험 설계, 결과 분석** 및 팀원들과의 **지식 공유**가 가장 큰 힘이 되었습니다. 이를 통해 단순히 순위뿐 아니라, 다양한 지식을 습득하는 값진 경험을 얻을 수 있었고, **협업의 중요성** 또한 깊이 체감할 수 있는 경험이었습니다.
## 타임라인
- 프로젝트 타임라인
![프로젝트 타임라인](https://github.com/user-attachments/assets/82d524f8-79c1-4bbb-ab78-44b9220b8d8b)

- 단계별 성능 변화 
![accu_timeline](https://github.com/user-attachments/assets/e2109364-d711-40a8-b5e0-2ffb7330e616)

## 프로젝트 수행 내용
- 모델 탐색 및 선정
	- 모델 선정 과정에서는 **다양한 모델의 Inductive Bias**를 깊이 이해하고, 데이터 특성에 최적화된 모델을 선택하기 위해 신중을 기했습니다. 먼저, 데이터의 **locality 정보를 활용한 분류**를 목표로 CNN 기반 모델 중 **기울기 소실 문제를 해결한 ResNet**을 후보로 고려했습니다.
	- 또한, 데이터의 **semantic한 정보를 반영한 분류**를 위해, ViT 기반 모델들 중 **ImageNet sketch 데이터에서 최고 성능(SOTA)을 기록한 EVA-Giant 모델**과 **EVA02-Large 모델**을 후보로 선정하였습니다. 이처럼 데이터의 특성에 적합한 모델을 선정함으로써, 최종적으로 높은 정확도를 목표로 한 최적의 모델을 탐색할 수 있었습니다.
	- 이러한 모델 선정 과정은 데이터의 본질을 이해하고, 그에 맞는 Inductive Bias를 가진 모델을 신중히 선택하는 것이 중요함을 다시 한번 느끼게 해주는 기회가 되었습니다.
	![image](https://github.com/user-attachments/assets/0302c586-42ae-492f-be48-292009b86f77)
	- Fine-tuning layers
		- MLP-3 : Linear(1024, 1024)-ReLU()-Linear(1024, 500)
		- Linear :(1024, 500)
	- Prediction method
		- train-validation split : train set을 한 번 나누어 성능을 평가하고 테스트 합니다.
		- 5-fold CV : 5-fold cross validation 방법을 통해 모델을 다섯 번 학습하고 voting 하여 테스트 합니다.
	- 하이퍼파라미터 튜닝
	![image](https://github.com/user-attachments/assets/b6e1b279-b541-434e-abba-5267f93bba9b)
## 문제 해결
![Pasted image 20241111212423.png](/img/user/Pasted%20image%2020241111212423.png)
- 학습된 모델의 성능은 90%이상으로 높았지만 부족한 10%의 성능을 끌어올리기 위해서 예측 결과를 시각화하고 이를 분석하였습니다.
- 총 두가지의 문제점을 찾을 수 있었고 이를 해결하기 위해서 아래와 같은 가설을 세우고 시도하였습니다.
- **문제 1 : (b) 수직으로 뒤집힌 Sketch 이미지 예측에 대한 취약점에 대한 보완 필요**
	- 우선적으로 사람도 두 클래스의 차이를 구분하기 어려운 이미지(a)의 경우보다는 수직 변형된 이미지에서의 예측을 보완하기로 하였습니다.
- 가설 1 : 수직 변환 데이터 증강을 추가하면 수직으로 변환된 이미지에 대한 인식 성능이 향상될 것이다.
- 실험 설계 1 : 기존 증강 + Vertical Flip 후 성능 비교
- 결과 분석 1 : 
	![image](https://github.com/user-attachments/assets/15b09b70-0a0d-4df0-a064-f6b4e20a6126)
	- 같은 조건에서 Vertical Flip 증강을 추가함으로써 더 높은 성능을 보이는 것을 확인하였습니다.
- **문제 2 : (a) 같은 클래스에 대해서 다양한 Sketch 이미지 표현에 대한 성능 취약점 보완 필요**
	- Sketch 이미지의 특성 상 같은 클래스(개)에서도 그리는 사람에 따라 **다양한 변형과 왜곡**이 발생할 수 있습니다.
	- 초기 학습 단계에서 복잡한 변형이 많이 포함된 Sketch데이터를 활용할 경우 모델이 패턴을 제대로 학습하지 못합니다.
- 가설 2 : **Curriculum learning**을 활용하여 쉬운 데이터부터 시작하여 점진적으로 어려운 데이터를 학습한다면 **초기에 기본적인 클래스의 윤곽선 패턴을 인식**하고 이후 **Elastic 변형, Grid Distortion 등 복잡한 변형**을 적용 후 학습하여 다양한 **변형과 왜곡에도 강한 일반화 성능**을 가지는 모델을 만들 수 있을 것이다. 
- 실험 설계 2 : **Curriculum learning**을 활용하여 기본 이미지 - 간단한 변형 - 복잡한 변형 순으로 학습 진행
	- Curriculum learning
		- 초기 단계(0~5 epoch) : 기본 이미지
			- 스케치 데이터의 기본 패턴을 학습하는데 집중하기 위해, 증강 없이 학습을 진행
			 ![Pasted image 20241111224327.png|200](/img/user/Pasted%20image%2020241111224327.png)
		- 중간 단계(5~10 epoch) : 수직/수평 뒤집기, 최대 10도 회전
			- 모델이 다양한 구도에 적응할 수 있도록, 수직/수평 뒤집기와 10도 회전 같은 간단한 증강을 추가
			![Pasted image 20241111224348.png](/img/user/Pasted%20image%2020241111224348.png)
		- 후반 단계(10~20 epoch) : 수직/수평 뒤집기, 최대 15도 회전, Elastic 변형, Grid Distortion
			- 모델이 다양한 변형과 왜곡에 강한 일반화 성능을 가질 수 있도록 Elastic 변형과 Grid Distortion 같은 고난이도 증강 추가
			![Pasted image 20241111224405.png|200](/img/user/Pasted%20image%2020241111224405.png)
- 결과 분석 2 : 
	![Pasted image 20241111225000.png](/img/user/Pasted%20image%2020241111225000.png)
	- 증강이 복잡해질 때마다, 정확도가 떨어지고 다시 올라가는 과정에서 모델이 보다 복잡한 패턴 학습
	- 최종적으로 단일 모델 최고 성능 달성(Accuracy(Test) : 0.9370)

## 앙상블 및 결과

## 개인 회고
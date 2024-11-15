---
{"dg-publish":true,"permalink":"/projects/sketch-image-data-classification-1st/","created":"2024-11-09T19:00:30.487+09:00","updated":"2024-11-16T00:02:46.604+09:00"}
---

#naverboostcamp #ai_tech #computer_vision #classification
## 프로젝트 개요
- 2024.09.11 ~ 2024.09.26
- ImageNet Sketch 이미지 데이터 분류
- 1st Prize 🏆
- Naver Connect & Upstage 주관 대회
- [git](https://github.com/boostcampaitech7/level1-imageclassification-cv-18)

## 대회 소개
![image](https://github.com/user-attachments/assets/e889ae72-c64f-48bb-95f0-ce7c73d56e4c)

Sketch 이미지 분류 경진대회는 주어진 Sketch 이미지 데이터를 활용하여 모델을 제작하고 어떤 객체를 나타내는지 분류하는 대회입니다.

## Leaderboard
![리더보드](https://github.com/user-attachments/assets/05f98560-85fb-43b7-b272-bef54f9a97e1)
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
			 ![Pasted image 20241111224327.png](/img/user/Pasted%20image%2020241111224327.png)
		- 중간 단계(5~10 epoch) : 수직/수평 뒤집기, 최대 10도 회전
			- 모델이 다양한 구도에 적응할 수 있도록, 수직/수평 뒤집기와 10도 회전 같은 간단한 증강을 추가
			![Pasted image 20241111224348.png](/img/user/Pasted%20image%2020241111224348.png)
		- 후반 단계(10~20 epoch) : 수직/수평 뒤집기, 최대 15도 회전, Elastic 변형, Grid Distortion
			- 모델이 다양한 변형과 왜곡에 강한 일반화 성능을 가질 수 있도록 Elastic 변형과 Grid Distortion 같은 고난이도 증강 추가
			![Pasted image 20241111224405.png](/img/user/Pasted%20image%2020241111224405.png)
- 결과 분석 2 : 
	![Pasted image 20241111225000.png](/img/user/Pasted%20image%2020241111225000.png)
	- 증강이 복잡해질 때마다, 정확도가 떨어지고 다시 올라가는 과정에서 모델이 보다 복잡한 패턴 학습
	- 최종적으로 단일 모델 최고 성능 달성(Accuracy(Test) : 0.9370)

## 앙상블 및 결과
- Hard Voting, Soft Voting
![image](https://github.com/user-attachments/assets/34e6350e-a600-451f-a148-ab25359eb4bc)
- Soft-Soft
![image](https://github.com/user-attachments/assets/12143ac2-88a5-4a29-bb42-0ca47834625f)
- Soft-Hard
![image](https://github.com/user-attachments/assets/3923ba34-a779-40ee-b178-16ce783c5fb6)
- hard-hard
![image](https://github.com/user-attachments/assets/395f99f5-2339-49b9-87e2-d245b9f2f3b3)
- 앙상블 성능 비교
![image](https://github.com/user-attachments/assets/10d75c04-f6c0-44c0-8228-f9133998e50e)
- EVA 02-large-Linear, EVA-giant-Linear, EVA 02-large-curriculum-mlp-3 를 hard-hard 앙상블한 모델이 가장 좋은 성능을 보였다.
- Soft-Soft, Hard,Hard 앙상블 Method 모두 비슷하게 좋은 성능을 보였다.

## 개인 회고
- **프로젝트에 앞선 학습 목표**
	- **다양한 문서와 자료를 탐색하여 최근 트렌드에 가장 가깝고 성능이 좋은 모델을 사용해보자**
	- **전반적인 AI 프로젝트의 진행을 전부 적극적으로 참여하여 단계 별 중요한 부분들을 배우자**
- **학습 목표 달성을 위해 무엇을 어떻게 했는가?**
	- 목표 달성을 위해 다양한 논문과 자료들을 탐색하며 최신 기술 트렌드를 파악하고, 성능이 우수한 모델들을 연구했습니다. 특히, 기존에 학습된 우수한 모델들을 활용한 전이학습에 중점을 두어 기학습된 모델의 중요성을 알게 되었습니다.
	- 모든 단계에 적극적으로 참여하며 실제 프로젝트 진행 과정을 경험하고, 팀원들과 협력하여 문제 해결 능력을 향상시킬 수 있었습니다.
- **나는 어떤 방식으로 모델을 개선했는가?**
	- Hugging face 내 다양한 베이스라인 모델들을 비교 분석하여 프로젝트 목표에 가장 적합한 모델을 선정하고 전이 학습을 적용했습니다.
	- k-fold cross validation과 소프트 보팅 앙상블 기법을 통해 모델의 성능을 향상시켰습니다. 
	- 단순히 성능 지표만을 고려하지 않고, 학습 과정에서 모델이 어떤 데이터에 취약한지를 분석하여 데이터 증강 기법을 적용했습니다.
- **어떤 깨달음을 얻었는가?**
	- 기존에 학습된 뛰어난 모델을 활용하는 것도 문제 해결에 있어서 큰 부분 중 하나라는 것을 깨달았습니다. 또한, 사전 학습된 우수 모델을 활용하는 **전이 학습의 효용성**을 실감했습니다.
	- k-fold cross validation을 활용함으로써, 모델이 데이터의 다양한 부분을 학습할 수 있게 하여 **과적합을 방지**하고, 모델의 일반화 성능을 높일 수 있다는 것을 확인하였습니다. 이를 통해 더욱 신뢰할 수 있는 평가 지표를 얻을 수 있었습니다.
	- 데이터 분석을 통해 증강 전략을 세우고 실제 성능 개선을 확인한 것은 매우 유익한 경험이었습니다. 데이터에 맞는 증강 전략의 중요성을 이해하고, 그 효과를 체감할 수 있는 기회였습니다.
- **마주한 한계는 무엇이며, 아쉬웠던 점은 무엇인가?**
	- 초기 EDA에서 데이터의 특성과 패턴을 이해하는데 어려움을 느꼈고 Inductive bias를 기반으로 한 모델링 방향을 설정하는데 어려움을 겪었습니다.
	- 코드 작성 방식과 협업 방식에서 크로스체크를 하지 못해 사소한 실수가 크게 퍼져나간 일이 있었다. 다양한 협업 툴을 사용하여 쉽게 확인할 수 있었던 실수를 빠르게 확인하지 못한 것이 아쉽다.
- **한계/교훈을 바탕으로 다음 프로젝트에서 시도해볼 것은 무엇인가?**
	- 이후 프로젝트에서는 EDA 분석에 더욱 집중하여 데이터 특성을 정확히 파악하고, inductive bias와 representation을 기반으로 적절한 모델을 선택할 것입니다.
	- 개인이 작성한 코드를 팀원들과 코드 리뷰를 통해 혹시 실수한 부분이 없는지 확인하는 과정을 거칠 것입니다.
	- Git과 같은 버전 관리 시스템과 내부 툴을 적극 활용하여 팀원들과의 원활한 소통과 코드 공유를 통해 안정적인 베이스라인을 구축하고 브랜치 전략을 효율적으로 활용하여 협업 효율성을 높일 것입니다.
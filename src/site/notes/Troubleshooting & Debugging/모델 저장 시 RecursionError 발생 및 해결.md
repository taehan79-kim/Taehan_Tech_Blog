---
{"dg-publish":true,"permalink":"/troubleshooting-and-debugging/recursion-error/","created":"2024-11-20T17:11:23.179+09:00","updated":"2025-02-15T18:06:37.948+09:00"}
---

## 에러 화면![Pasted_image_20241120171250.png](/img/user/Pasted_image_20241120171250.png)
## 에러 발생 과정
- Hand Bone SemanticSegmentation project 진행 중 u3+ 모델 구조에서 backbone을 resnet50에서 EfficientNet-b0으로 바꿔서 학습을 진행하던 중 원래 잘 돌아가던 train 코드에서 save model함수를 실행하는 과정에서 에러가 발생하였습니다. 
## 에러 발생 이유
- 이 오류는 `torch.save`를 호출할 때 발생하며, 모델 객체 내부에서 **순환 참조(circular reference)**가 존재하여 직렬화(serialization) 과정에서 최대 재귀 깊이를 초과했기 때문에 발생합니다.
- 원인 분석
	- **순환 참조(circular reference)**:
		- 모델의 일부 서브모듈 또는 속성이 다시 모델 자체를 참조하거나, 서로를 참조하고 있을 가능성이 있습니다.
		- `torch.save`는 모델의 모든 속성을 탐색하고 직렬화하려고 시도하며, 순환 참조가 있으면 무한 루프에 빠지게 됩니다.
	- **`save_model` 함수에서 전체 모델 저장**:
		- `torch.save(model, path)`는 모델 전체(구조 + 가중치)를 저장합니다. 모델 구조가 복잡하거나 순환 참조가 있을 경우 문제가 발생할 수 있습니다.
	- **잘못된 객체 포함**:
		- 모델에 추가된 사용자 정의 객체가 직렬화할 수 없는 경우도 이 오류의 원인이 될 수 있습니다. 예를 들어, 데이터 로더, 파일 핸들, 또는 잘못된 클래스 인스턴스 등이 모델의 속성으로 포함되어 있을 경우 발생할 수 있습니다.
- 나의 경우 에러 발생 원인 가능성
	- **`save_model` 함수에서 전체 모델 저장**에 해당하고 있었습니다. 
		- 그 전에는 크기가 작은 모델을 사용하고 있었기 때문에 문제 없이 코드가 돌아갔던 것으로 예상됩니다.
		- 기존 resnet 보다 훨씬 복잡한 구조를 가진 EfficientNet-b0의 경우 이러한 에러가 발생핳 수 있다고 예상됩니다.
	- **잘못된 객체 포함**:
		- 노드 및 feature 출력층에서 사용하는 텐서의 형태가 직렬화시 문제가 일어날 수 있습니다.
## 에러 해결
- 임시방편
	- 디버깅 및 재귀 깊이 수정
	- 일시적으로 재귀 깊이를 늘려서 임시적으로 해결이 가능합니다.
	- 하지만, 밑의 해결 방법을 통해 근본적인 원인을 수정하는 것이 좋습니다.
```
	  import sys 
	  sys.setrecursionlimit(1500)
```
- 해결
	- 모델의 가중치만 저장하는 방식으로 전환해서 해결하였습니다.
	- ![Pasted_image_20241123192606.png](/img/user/Pasted_image_20241123192606.png)
	- ![Pasted_image_20241123192538.png](/img/user/Pasted_image_20241123192538.png)
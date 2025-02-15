---
{"dg-publish":true,"permalink":"/projects/recycling-waste-object-detection-2nd/","tags":["naverboostcamp","ai_tech","computer_vision","object_detection"],"created":"2024-11-12T02:10:04.243+09:00","updated":"2025-02-15T18:25:20.875+09:00"}
---

## 프로젝트 개요
- 2024.10.02 ~ 2024.10.24
- 재활용 품목 분류를 위한 Object Detection
- 2nd Prize 🏆
- Naver Connect & Upstage 주관 대회
- [git](https://github.com/boostcampaitech7/level2-objectdetection-cv-18)

---
## 대회 소개
![image](https://github.com/user-attachments/assets/c3f7a3e7-dffc-427e-ac34-57b2c4659b21)
재활용 품목 분류를 위한 Object Detection 모델 개발. 10종류의 쓰레기 품목 Object Detection 모델의 객체 탐지 성능 향상시키는 것을 목적으로 하는 대회입니다.

---
## 개발 환경
- Language : Python
- Environment
	- CPU : Intel(R) Xeon(R) Gold 5120
	- GPU : Tesla V100-SXM2 32GB x 1
- Framework : PyTorch
- Collaborative Tool : GitHub, Tensorboard, Notion

---
## Leaderboard
![image](https://github.com/user-attachments/assets/a6e460ca-b192-4db5-b9e8-39f0f685b84c)
	다양한 모델을 테스트하고 수많은 가설을 실험한 끝에, **mAP_50 0.7482**를 기록하며 **2위**의 성과를 달성했습니다. 최신 SOTA 모델인 **Co-DETR**부터 **ATSS**, **Faster R-CNN**, **DyHead**, **YOLO** 등 여러 모델을 시도했으며, 최종적으로 **앙상블**을 통해 성능을 극대화했습니다. 특히, 성능이 낮았던 YOLO 모델을 앙상블함으로써 성능을 극대화 할 수 있었습니다. 이로써, 다양한 모델을 조합하여 앙상블하는 것이 성능 향상에 중요한 역할을 한다는 점을 깨달았습니다. 
	또한, 팀원들과의 협업 과정에서 **Git Issue**, **Pull Request**, **Project Table**을 적극적으로 활용하여 프로젝트 진행 상황을 체계적으로 관리했습니다. 이러한 협업 도구의 활용은 원활한 소통과 효율적인 업무 분배에 큰 도움이 되었고, 결과적으로 높은 성과로 이어질 수 있었습니다.

---
## 타임라인
![Pasted_image_20241113223533.png](/img/user/Pasted_image_20241113223533.png)

---
## 프로젝트 수행 절차 및 방법
사진에서 **쓰레기를 탐지**하기 위해 **MMDetection 오픈소스 객체 탐지 라이브러리**를 활용하여 다양한 객체 탐지 모델을 실험하고 최적화했습니다. 여러 객체 탐지 모델을 **재활용 품목 분류 과제에 맞게 튜닝**하고 조합한 결과, 네이버 부스트캠프에서 개최한 **“재활용 품목 분류를 위한 Object Detection 대회**"에서 **mAP50 0.7482**를 기록하며 **2위**를 차지할 수 있었습니다.

쓰레기 Detection 성능을 높이기 위해 제가 시도한 주요 접근법은 다음과 같습니다:
1. **다양한 객체 탐지 모델 실험**  
    여러 모델을 테스트하며, 각 모델이 재활용 품목 분류에 적합하도록 **하이퍼파라미터 튜닝**을 진행했습니다.
    
2. **모델 조합 및 앙상블**  
    성능을 극대화하기 위해 **여러 모델을 조합하여 앙상블**을 시도했으며, 다양한 모델을 결합하는 것이 성능 향상에 큰 도움이 되었습니다.
    
3. **MMDetection의 활용**  
    MMDetection 라이브러리를 통해 다양한 모델 구조와 기능을 효율적으로 적용할 수 있었으며, 이를 통해 실험 과정이 크게 최적화되었습니다.

---
## 데이터 처리 및 EDA
### 1-1 데이터 분할
모델 학습 과정에서 성능을 객관적으로 평가하기 위해 **학습 데이터셋**과 **검증 데이터셋**으로 데이터를 분할했습니다. 특히, 학습 데이터셋의 **클래스 분포 불균형** 문제를 해결하기 위해 **Stratified Group K-Fold** 방식을 적용하여 각 fold에서 클래스 분포가 균등해지도록 했습니다.
![Pasted_image_20241113224501.png](/img/user/Pasted_image_20241113224501.png)
최종적으로 **Fold 1에 대해 실험을 진행해 성능을 측정**하고, 이 실험 결과를 모든 fold에 반영하여 **5-Fold 앙상블**로 최종 결과물을 도출했습니다. 이 과정에서 Stratified Group K-Fold 방식은 모델의 일반화 성능을 높이고, 더 일관된 평가를 가능하게 했습니다.

이러한 접근은 데이터 분포를 효과적으로 반영하며 최종 성능을 극대화하는 데 큰 도움이 되었습니다.

### 1-2 Anchor box 
- 클래스별 bounding box aspect ratio
![Pasted_image_20241113224624.png](/img/user/Pasted_image_20241113224624.png)
모델 성능을 극대화하기 위해, 주어진 데이터에서 **각 클래스의 bounding box aspect ratio를 시각화하여 분석**했습니다. 이를 통해 모든 클래스에서 객체의 aspect ratio가 **Q1-Q3 범위 내 [0.4725, 2.1209]**에 분포하는 것을 확인할 수 있었습니다.

이 분석을 바탕으로 **데이터셋에 최적화된 anchor box ratio를 설정**하여, 객체 탐지 성능을 향상시키고 불필요한 anchor의 사용을 최소화했습니다. 이를 통해 anchor가 각 객체의 크기와 형태에 보다 잘 맞도록 조정함으로써, 모델의 학습 효율을 높일 수 있었습니다.

객체 탐지 작업에서 anchor 설정의 중요성을 다시금 실감한 경험이었으며, 앞으로도 데이터 분포에 맞는 anchor 최적화를 적극적으로 고려할 계획입니다.

### 모델 Predictions bounding box 및 PR 곡선 시각화
![Pasted_image_20241113224744.png](/img/user/Pasted_image_20241113224744.png)
모델의 성능을 직관적으로 평가하기 위해 **검증 데이터셋에 대해 Prediction bounding box와 Ground Truth bounding box, 그리고 PR (Precision-Recall) 곡선을 시각화**했습니다. 이를 통해 **모델의 성능을 시각적으로 확인**하고, 모델의 약점을 파악할 수 있었습니다.

시각화 결과, 모델이 **작은 객체(General trash)**에 대해서는 localization 성능이 떨어지고, **겹쳐 있는 객체(Plastic)**에 대해서는 classification 성능이 저하되는 경향을 확인할 수 있었습니다.

이러한 분석을 통해 모델의 취약점을 구체적으로 파악하고, 이후 성능 개선을 위한 전략을 수립할 수 있는 중요한 인사이트를 얻을 수 있었습니다. 앞으로도 시각적 분석을 통해 모델의 성능을 다각도로 평가하고 최적화할 계획입니다.

---
## MMdetection
### baseline 모델 탐색
베이스라인 초기 실험에서는 **MMDetection 라이브러리**를 활용하여 다양한 객체 탐지 모델들의 성능을 비교했습니다. **Fold 1 데이터셋**을 사용하여 실험을 진행했으며, 이를 통해 각 모델의 초기 성능을 확인할 수 있었습니다.

아래 표는 **Faster R-CNN, Cascade R-CNN, ATSS, UniverseNet, RetinaNet, VFNet** 모델들의 성능 비교 결과를 보여줍니다. 이를 통해 각 모델의 성능 차이를 분석하고, 최적의 모델을 선정하기 위한 기초 자료로 삼았습니다.

| Model         | Backbone | Neck | Optimizer | lr     | Epoch | Test mAP50 |
| ------------- | -------- | ---- | --------- | ------ | ----- | ---------- |
| Faster R-CNN  | ResNet50 | FPN  | SGD       | 0.02   | 12    | 0.3734     |
| Cascade R-CNN | SwinL    | FPN  | SGD       | 0.02   | 20    | 0.5161     |
| ATSS          | SwinL    | FPN  | SGD       | 0.02   | 20    | 0.5015     |
| UniverseNet   | SwinL    | FPN  | AdamW     | 0.0001 | 20    | 0.5545     |
| RetinaNet     | SwinL    | FPN  | AdamW     | 0.0001 | 20    | 0.5438     |
| VFNet         | SwinL    | FPN  | AdamW     | 0.0001 | 20    | 0.5623     |

### Backbone 탐색
Cascade R-CNN 모델의 **Backbone**으로 **ResNet50, Swin Transformer Small, Swin Transformer Large**를 사용하여 성능을 비교했습니다.

- **ResNet50**은 전통적인 CNN 기반의 모델로, 이미지의 로컬 정보를 효과적으로 학습하지만, Transformer 기반 모델에 비해 상대적으로 낮은 정확도를 보였습니다.
- **Swin Transformer**는 **Transformer 기반**의 Backbone으로, 이미지 패치를 처리하면서 전역적인 특징을 효과적으로 추출합니다. 이를 통해 Cascade R-CNN 모델이 더 풍부한 정보를 학습할 수 있도록 도와줍니다.

결과적으로, **Backbone을 Swin Transformer 모델**로 전환함으로써 성능이 유의미하게 향상됨을 확인할 수 있었습니다. 특히, **Swin Transformer 모델**은 **ImageNet-22K** 데이터셋에서 (384x384) 이미지로 학습된 **사전 학습 가중치**를 활용하여 **fine-tuning** 과정을 거쳐 최적화했습니다.

아래는 각 Backbone 모델의 성능 비교 결과입니다:

| Model         | Backbone | Test mAP50 |
|---------------|----------|------------|
| Cascade R-CNN | ResNet50 | 0.3613     |
| Cascade R-CNN | SwinS    | 0.4628     |
| Cascade R-CNN | SwinL    | 0.5161     |

### 결과
![Pasted_image_20241113225944.png](/img/user/Pasted_image_20241113225944.png)
객체 탐지 성능을 높이기 위해 **다양한 모델 구조와 Backbone, Neck** 조합을 시도하고, **하이퍼파라미터 튜닝** 및 **데이터 증강 기법**을 변경하며 성능 변화를 분석했습니다. 각 조합이 성능에 미친 영향을 요약한 결과는 다음과 같습니다.

- **강력한 Backbone과 Multi-scale 기법**의 조합이 높은 성능을 보였습니다. 특히, **Swin Transformer** 같은 강력한 Backbone을 사용하는 모델은 복잡한 데이터를 더욱 효과적으로 학습할 수 있었습니다.
    
- **ATSS 모델**에서는 **SwinL Backbone**과 **FPN Neck**, 그리고 **다중 스케일 증강**을 결합했을 때 가장 좋은 성능인 **mAP50: 0.6752**를 기록했습니다.
    
- **Cascade R-CNN**과 **UniverseNet 모델** 역시 **다중 스케일 증강**을 적용했을 때 높은 성능을 보였습니다. 이로써 다양한 크기의 객체에 대한 강인한 대응력을 갖춘 모델 구조가 효과적임을 확인할 수 있었습니다.
    
- 반면에 **Mosaic 증강 기법**은 성능에 큰 도움이 되지 않음을 확인했습니다. 일부 모델에서 Mosaic을 적용했을 때 오히려 성능이 떨어지는 경향이 관찰되었습니다.
    

이 실험을 통해 최적의 조합을 찾아가는 과정이 성능 향상에 매우 중요함을 실감할 수 있었으며, 데이터 특성에 맞는 Backbone과 증강 기법을 선택하는 것이 성능 향상에 효과적임을 검증했습니다.

---
## Yolo
**YOLO (You Only Look Once)**는 객체 탐지에서 널리 사용되는 모델로, 이미지 내의 객체를 한 번의 전방향 패스(forward pass)만으로 탐지하는 1-Stage 모델입니다. 다른 전통적인 객체 탐지 방식들은 객체를 여러 단계에 걸쳐 탐지하지만, YOLO는 이를 동시에 처리하는 특징을 가집니다. 이 방식은 **속도**와 **효율성**을 크게 향상시켜 실시간 객체 탐지에 적합합니다.
### baseline 모델 탐색
| Model    | Image Size | FLOPs     | Validation mAP40 |
| -------- | ---------- | --------- | ---------------- |
| **YOLOv5x6** | **1280**   | **209.8** | **55.0**         |
| YOLOv11x | 640        | 194.9     | 54.7             |

YOLO 모델을 선택할 때, **YOLOv5**와 **YOLOv11** 중 하나를 고려했습니다. **YOLOv11x**는 최신 모델로, 속도가 빠르고 상대적으로 낮은 **FLOPs**를 요구하는 장점이 있습니다. 하지만, 이번 대회의 목표는 **성능 우선**이었기 때문에, **속도**나 **경량화**보다는 **정확도**에 더 중점을 두기로 했습니다.

이에 따라, **YOLOv5x6** 모델을 선택했습니다. YOLOv5x6는 **YOLOv11x**보다 **파라미터**가 약 2.5배 많아 속도는 느리지만, 높은 정확도를 보였습니다. 또한, **Image Size**를 **1024x1024**로 설정했을 때, **YOLOv5x6**가 더 적합하다고 판단하여 이를 모델로 채택하게 되었습니다.

따라서, 최종적으로 성능을 최우선으로 고려한 선택이었으며, 속도보다는 정확도 향상을 목표로 했습니다.

### 하이퍼파라미터 튜닝
| Model      | Image size | lr    | momentum | decay  | Step size | anchor box    | test  | mAP   |
|------------|------------|-------|----------|--------|-----------|---------------|-------|-------|
| YOLOv5x6   | 1280       | 0.1   | 0.937    | 0.005  | 3         | original      |       | 0.4770|
| YOLOv5x6   | 1280       | 0.01  | 0.937    | 0.0005 | 3         | original      |       | 0.5015|
| YOLOv5x6   | 1280       | 0.01  | 0.937    | 0.0005 | 3         | anchor box tunning |  | 0.5303|
## Co-DINO
### baseline 모델 탐색
| Model                   | Backbone | Pre-training Dataset | Fine-Tuning Dataset | Dataset split          | validation mAP50 | test mAP50 |
|-------------------------|----------|-----------------------|----------------------|------------------------|------------------|------------|
| Co-Deformable-DETR       | R50      | COCO                  | Train set            |                        | 0.3341           |            |
| Co-DINO                  | Swin-T   | COCO                  | Train-Validation split |                        | 0.4220           |            |
| Co-DINO                  | Swin-L   | COCO                  | Train-Validation split |                        | 0.7170           | 0.7071     |
| Co-DINO                  | Swin-L   | COCO                  | Train set            |                        | 0.7190           |            |
| Co-DINO                  | Swin-L   | COCO                  | 5-fold CV            |                        | 0.7283           |            |

실험 결과, 다양한 모델 중 **Co-DINO**가 가장 우수한 성능을 기록했습니다. Co-DINO 모델은 **Contrastive Denoising** 기법을 사용하여 효율적인 앵커 박스를 추출하고, 이를 통해 객체 검출 성능을 크게 향상시킬 수 있었습니다.

또한, Backbone 모델 분석에서 **Swin-L**은 이전 실험 결과대로 Transformer 기반의 모델 중 가장 높은 성능을 보였습니다. 실험에서는 **COCO** 데이터셋으로 pre-training된 모델 가중치를 활용하였으며, 학습 데이터 분할 방식을 조정하면서 성능 변화를 비교했습니다.

특히, **K-fold cross validation**을 통해 데이터셋을 분할하고 학습한 뒤 앙상블 기법을 적용한 결과, 성능이 크게 향상됨을 확인할 수 있었습니다. 이를 바탕으로 데이터셋을 여러 Fold로 나누고 앙상블을 활용하는 방법을 채택했습니다. 최종적으로, **Co-DINO** 모델을 베이스라인 모델로 선정하여 최상의 성능을 얻을 수 있었습니다.
### 하이퍼파라미터 튜닝
경진대회의 특성상 시간적 제약이 있었기 때문에, **Co-DINO Swin-L** 모델은 12에폭 학습에 약 36시간이 소요되는 문제점이 있었습니다. 이에 따라 기존 논문에서 제시된 최적의 하이퍼파라미터를 참고하여 기준을 설정하였습니다.

학습에 사용된 하이퍼파라미터는 다음과 같습니다:
- **Optimizer**: AdamW
- **Learning rate**: 0.0002

| Model   | Backbone | Fine-Tuning Dataset    | Input image size | validation mAP50 | test mAP50 |
| ------- | -------- | ---------------------- | ---------------- | ---------------- | ---------- |
| Co-DINO | Swin-T   | Train-validation split | (1024, 1024)     | 0.4160           |            |
| Co-DINO | Swin-T   | Train-validation split | **(1280, 1280)**     | **0.4220**       |            |
| Co-DINO | Swin-T   | Train-validation split | (1536, 1536)     | 0.0720           |            |
| Co-DINO | Swin-L   | Train set              | (512, 512)       |                  | 0.6686     |
| Co-DINO | Swin-L   | Train set              | **(1280, 1280)** |                  | **0.7790** |


다양한 실험을 통해 입력 이미지의 크기가 클수록 해상도가 높아져 더 많은 객체를 탐지할 수 있다는 사실을 확인한 바 있습니다. 이를 Co-DINO 모델에 적용하여 다양한 입력 이미지 크기에서의 성능을 확인해본 결과, 이미지 크기가 원본보다 작을수록 정보 손실이 발생하여 성능이 떨어짐을 확인할 수 있었습니다. 그러나 입력 이미지 크기를 1280x1280 이상으로 늘렸을 때는 오히려 성능이 감소하는 현상을 발견했습니다.

그 이유는, 특정 크기 이상의 이미지에서는 backbone 모델의 윈도우 크기 및 모델 아키텍처의 구조 상, 이미지에서 추출할 수 있는 feature 정보가 제대로 뽑히지 않기 때문으로 예상됩니다. 이는 큰 이미지 크기에서 모델이 다룰 수 있는 feature 맵의 크기나 계산 효율성에 한계가 있기 때문으로 보이며, 이를 통해 적절한 이미지 크기의 선택이 모델 성능에 중요한 영향을 미친다는 점을 확인할 수 있었습니다.

---
### 최종 성능
| Model            | Backbone | Pretraining Dataset | Fine-Tuning Dataset              | Input image size | validation mAP50 | test mAP50 |
| ---------------- | -------- | ------------------- | -------------------------------- | ---------------- | ---------------- | ---------- |
| CoDeformableDETR | R50      | COCO                | Train set (400~800) (multi-size) | -                | -                | 0.3341     |
| Co-DINO          | Swin-T   | COCO                | Train-validation split           | (1024, 1024)     | 0.4160           | -          |
| Co-DINO          | Swin-T   | COCO                | Train-validation split           | (1280, 1280)     | 0.4220           | -          |
| Co-DINO          | Swin-T   | COCO                | Train-validation split           | (1536, 1536)     | 0.0720           | -          |
| Co-DINO          | Swin-T   | COCO                | Train-validation split           | (1280, 1280)     | 0.7170           | 0.7071     |
| Co-DINO          | Swin-L   | COCO                | Train set                        | (512, 512)       | -                | 0.6686     |
| Co-DINO          | Swin-L   | COCO                | Train set                        | (1280, 1280)     | -                | 0.7190     |
| Co-DINO          | Swin-L   | COCO                | 5-fold CV                        | (1280, 1280)     | -                | **0.7283** |

---
## 앙상블
Object Detection Task 에선 대표적으로 NMS, soft NMS, NMW, WBF 4가지의 앙상블 기법이있다. 간단한 기본 모델에 대해서 각 앙상블의 성능을 평가한 뒤, 결과가 좋았던 앙상블 기법으로 최종 결과물을 만들었다.

| 앙상블 기법 | ATSS | Cascade RCNN | UniversNet | Co-DINO | YOLOv5x6 | test mAP50 |
| ------ | ---- | ------------ | ---------- | ------- | -------- | ---------- |
| WBF    | o    | o            |            |         |          | 0.7055     |
| NMW    | o    | o            |            |         |          | 0.7118     |
| NMW    | o    | o            | o          |         |          | 0.6948     |
| WBF    | o    |              |            | o       |          | 0.7198     |
| NMS    | o    |              |            | o       |          | 0.7217     |
| NMW    | o    | o            |            | o       |          | 0.7327     |
| NMW    | o    | o            |            | o       | o        | **0.7553**     |


최종적으로 ATSS, Cascade RCNN, Co-DINO, YOLOv5x6을 NMW 기법으로 앙상블했을 때 가장 좋은 성능을 보였고, 그 결과 대회를 2등으로 마무리할 수 있었습니다. 흥미로운 점은 YOLOv5x6 모델의 성능이 다른 모델들에 비해 상대적으로 낮은 편이었지만, 앙상블을 적용했을 때 성능이 크게 향상되었다는 것입니다. YOLOv5x6 모델은 정확히 맞출 수 있는 객체에 대해서만 bounding box를 예측하는 경향이 있기 때문에, 앙상블 기법을 통해 서로 보완하는 효과가 나타나 더 높은 성능을 발휘할 수 있었습니다. 이 결과는 앙상블 기법이 각 모델의 강점을 결합하여 더 뛰어난 성능을 낼 수 있음을 잘 보여주었습니다.

---
## 개인 회고
**프로젝트에 앞선 학습 목표**

- 다양한 문서와 자료를 탐색하여 최근 트렌드에 가장 가깝고 성능이 좋은 모델을 사용해보자
- 문제 해결을 위해 다양한 가설을 제시하고 그에 맞는 실험을 진행하여 결과를 분석해보자

**학습 목표 달성을 위해 무엇을 어떻게 했는가?**

- Papers with code와 논문, git을 참고하여 현재 SOTA 모델인 Co-DETR 모델을 학습에 사용해보았습니다.
- 입력 이미지 크기에 따른 성능 변화, Super Resolution을 활용한 이미지 해상도 증대 및 Center Crop에 활용 등 다양한 실험을 진행하고 결과를 분석하였습니다.

**나는 어떤 방식으로 모델을 개선했는가?**

- SOTA 모델인 Co-DETR의 config 파일을 분석하고 pre-trained model 가중치를 불러와 쓰레기 object detection학습에 적용하였습니다.
- Bbox_s 지표를 활용하여 작은 객체에서의 탐지 성능을 향상시키기 위해 입력 이미지 크기를 조절하고 다양한 모델에 증강을 통해 적용시켰습니다.
- 단순히 성능 지표만을 고려하지 않고, 학습 과정에서 모델이 어떤 데이터에 취약한지를 분석하기 위해 이미지의 예측 bbox와 pr 곡선을 시각화하여 직접 취약한 데이터를 눈으로 확인하였습니다.

**내가 한 행동의 결과로 어떤 지점을 달성하고, 어떤 깨달음을 얻었는가?**

- bbox AP 60.7의 성능을 가지는 Co-DINO Swin-L모델을 분석하고 적용하여 다른 팀들보다 가장 먼저 0.70을 넘어선 0.7190의 결과를 얻었습니다.

- 기존에 학습된 뛰어난 모델을 활용하는 것도 문제 해결에 있어서 큰 부분 중 하나라는 것을 깨달았습니다. 또한, 사전 학습된 우수 모델을 활용하는 전이 학습의 효용성을 실감했습니다.

- k-fold cross validation 기법과 Non-maximum-weighted ensemble을 적용하여 기존 71.90%의 모델 성능을 72.83%까지 향상시켰습니다.

- k-fold cross validation을 활용함으로써, 모델이 데이터의 다양한 부분을 학습할 수 있게 하여 과적합을 방지하고, 모델의 일반화 성능을 높일 수 있다는 것을 확인하였습니다.

- bbox와 pr 곡선을 시각화하여 모델의 작은 객체에서의 취약점을 확인하였고 이를 해결하기 위한 방향성 설정할 수 있었습니다.

- 큰 성능 개선으로 이어진 것은 아니였지만 단순히 성능 지표를 분석하는 것에서 벗어나 직접 모델의 예측을 눈으로 확인하고 gt와 비교하며 모델의 취약점을 확인할 수 있었습니다.

**마주한 한계는 무엇이며, 아쉬웠던 점은 무엇인가?**

- EDA를 통해 데이터의 특성과 패턴을 파악하고 이를 통해 문제를 정의하고 가설을 설정하는 단계에서 많은 어려움이 있었습니다.
- SOTA 모델 중 사전학습된 가중치를 활용하는 과정에서 대회 룰에서 벗어난 데이터임에도 제대로 확인하지 못하는 실수를 범했습니다. 이 후 모델을 활용하기 전 모델의 구조와 사용된 데이터를 다시 한 번 확인해야 한다는 것을 깨달았습니다.
- 효율적인 협업 방식을 통한 코드 공유가 중요하다는 것을 깨달았습니다. 각자 진행하면서 작성한 코드를 공유하여 팀원이 사용할 경우 많은 어려움이 있었고 이를 해결하기 위해 결국 더 많은 시간을 소요하게 되었습니다.
- 모델 선정에서 성능만을 치중하여 생각했던 부분이 있었습니다. YOLO 모델의 경우 성능이 0.53으로 높지 않았고 좋지 않은 모델이라 생각했지만 ensemble과정에서 매우 큰 성능향상을 보여 모델을 구조적 관점에서 바라보는 것의 중요성을 깨달았습니다. 단일 모델으로는 높은 성능을 내지 못하더라도 그 모델의 장점을 활용할 수 있다면 여러 측면으로 바라볼 수 있다는 것을 알게 되었습니다.

**한계/교훈을 바탕으로 다음 프로젝트에서 시도해볼 것은 무엇인가?**

- EDA 분석 및 기존 진행된 다양한 대회들의 solution및 discussion을 참고하여 다양한 insight를 얻을 것입니다.
- 개인이 작성한 코드를 팀원들과 코드 리뷰를 하는 시간을 가져 정확한 코드 이해를 돕고 개인의 지식을 공유하는 과정을 거치겠습니다.
- 모델을 다양한 관점에서 바라보고 그 모델의 장단점, 취약점을 파악하고 이를 해결하기 위한 방법을 도출하여 시도해보겠습니다.
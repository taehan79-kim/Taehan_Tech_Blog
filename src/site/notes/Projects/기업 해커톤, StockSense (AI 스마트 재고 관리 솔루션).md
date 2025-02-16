---
{"dg-publish":true,"permalink":"/projects/stock-sense-ai/","created":"2025-02-16T15:21:09.018+09:00","updated":"2025-02-16T15:54:17.956+09:00"}
---

## 프로젝트 개요
### StockSense : AI 스마트 재고 관리 솔루션
- 2025.01.06 ~ 2025.02.12
- Upstage 기업 연계 해커톤
- 트렌드 & 리콜 상품을 파악해 자동으로 최적의 재고를 관리하는 AI 재고 관리 솔루션

---
## 프로젝트 소개
- 문제 정의 및 해결 과제
	최근 유통업계의 급격한 변화 속에서 대형마트 및 소매점의 점포 감소, 무인점포의 경제적 어려움, 자영업자의 지속적인 폐업 증가 등이 주요 이슈로 부각되고 있다.
	- [2024.05.20 메트로신문 '소비 트렌드 변했다' 대형마트 점포 7년 새 37개 감소..."본업 집중 효율성 강화"](https://www.emetro.co.kr/article/20240520500503)
	- [2024.02.18 한국경제 “월세가 160만원인데 매출이 60만원“...’무인점포‘의 눈물](https://www.hankyung.com/article/202402168434g)
	- [2024.02.18 서울경제 “1년도 못 버티고 폐업…코로나때보다 힘들어” 깊어지는 자영업자 한숨](https://www.sedaily.com/NewsView/2DGQ6ND1FB)
	따라서, 재고 부족이나 과잉 재고로 인한 손실이 발생하여 비효율적인 재고관리가 이루어지고 매장 관리자가 데이터를 수동으로 분석하고 의사결정을 내려야 하는 낮은 효율성 구조 문제, 소비 트렌드 및 리콜 상품의 정보를 놓쳐 **`잠재 매출 감소 및 운영 비용 증가`**와 같은 문제가 발생한다.
- 해결 방안
	- **AI 기반 실시간 재고 관리 시스템 도입**
	    - 판매량 데이터를 머신러닝을 통해 판매량을 예측하여 재고 부족 시 관리자에게 즉각 알림을 제공한다.
	- **통합 대시보드 제공**
	    - 재고 상태와 판매 데이터를 통합하여 시각화된 대시보드로 제공하고, 관리자들이 데이터를 직관적으로 파악하고 빠르게 의사결정을 내릴 수 있도록 지원한다.
	- **맞춤형 매장 관리 솔루션 제공**
	    - 최신 트렌드 상품 추천 및 리콜 상품 방지 솔루션을 통해 트렌드 & 리콜 상품을 파악해 자동으로 최적의 재고를 관리한다.

---
## 개발 환경
- Language : Python
- Environment
	- CPU : Intel(R) Xeon(R) Gold 5120
	- GPU : Tesla V100-SXM2 32GB x 1
- Framework : PyTorch
- Collaborative Tool : GitHub, WandB, Notion, Slack
---
## 타임라인
![Pasted image 20250216154228.png](/img/user/Pasted%20image%2020250216154228.png)
---
## 프로젝트 수행 내용
### 프로젝트 워크플로우
![Pasted image 20250216154303.png](/img/user/Pasted%20image%2020250216154303.png)
### 주요 기능
- DB Repository : **`Supabase & PostgreSQL`**
    - 데이터셋은 Dacon[온라인 채널 제품 판매량 예측 AI 온라인 해커톤]의 시계열 학습 데이터셋과 실제 Amazon.com에서 사용하는 Amazon Categories 상품명의 9500개의 taxonomy를 활용해 서비스 완성도를 높였다.
    - Database tool은 Supabase를 통한 PostgreSQL을 활용해 네트워크 원격 접속 및 다중 사용자가 동시에 데이터베이스에 접근하고 수정이 가능하도록 관리했다.
- 우리 프로젝트의 전체흐름은 아래와 같다.
![Pasted image 20250216154402.png](/img/user/Pasted%20image%2020250216154402.png)
### Trend Searching Workflow

Naver Datalab에서 실시간 트렌드 상품을 불러와 자동 주문에 요청되는 상품을 파악하기 위한 Workflow이다.

- `Airflow Dag`를 통해서 일정 주기마다 셀레니움으로 크롤링된 `Naver Datalab Shopping Insights` 페이지를 스크린샷을 추출한 후 각 카테고리별 트렌드 상품을 **`Upstage Document OCR API`**를 사용해 데이터베이스에 해당 상품이름과 순위를 저장한다.
- 관련 정보를 요약한 리포트를 Gmail로 전송해 개인 사업자가 주마다 트렌드 상품 정보를 받아볼 수 있다.
- 웹 내 Chabot에서 예측한 트렌드 근거와 다음달 트렌드 상품 예측 정보를 제공받아 개인 사업자가 마케팅 정보를 바로 활용하기 어렵던 문제를 해결할 수 있다.
#### Category Search

- 트렌드 상품들이 Category Search 기법으로 실제 판매 중인지 확인할 수 있는 계층 구조 기반의 카테고리 탐색 기법으로 `Hierarchy (main > sub1 > sub2 > sub3)` ****기반으로 작동하는 **parent & child** 구조를 통해 검색 범위를 제한한다.
    
- Parent category 탐색 결과 및 reasoning 과정을 child category에서 Context로 추가 활용한다.
    
    > ex. If input_text = Potato chip and main = food, then add more context(ex. Potato chip is food) when searching sub1.
    
- `Few Shot` 기반 카테고리 분류
    
    - Reasoning 과정에서 각 카테고리의 레벨별 예제를 구축하고 이를 Prompt로 활용하여 더 많은 Context를 사용함으로써 Category Searching Algorithm 를 개선한다.![Pasted image 20250216154514.png](/img/user/Pasted%20image%2020250216154514.png)

### Recall 상품 파악 Workflow

- `safetykorea(제품안전정보센터)`의 공시 정보를 통해서 리콜 상품 파악 Jina API로 주기적으로 웹 화면 정보를 가져와 Database에 저장한다.
- 만약 조회된 리콜 상품이 판매중이라면, 사업자에게 알림 제공 및 자동으로 재고에서 제거하고 인벤토리 대시보드에서 반영결과를 확인함으로써 사후 문제를 예방할 수 있다.

### 대시보드


| 챗봇 페이지                                              | 대시보드 페이지                                                   | 인벤토리 페이지                                                    |
| --------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------------- |
| ![Pasted image 20250216154642.png](/img/user/Pasted%20image%2020250216154642.png)                | ![Pasted image 20250216154645.png](/img/user/Pasted%20image%2020250216154645.png)                       | ![Pasted image 20250216154650.png](/img/user/Pasted%20image%2020250216154650.png)                        |
| 챗봇을 이용해 매장재고에 관련된 분석결과와, 트렌드 상품 관련 분석결과를 제공받을 수 있다. | 일, 주, 월, 연간 매출등의 수치와 변화량을 확인할 수 있고 최다 매출 상품과 트렌드 상품을 보여준다. | 현재 상품의 재고 현황, 예측된 다음 판매량을 기준으로 매장의 최소 재고 기준, 주문 상태현황을 보여준다. |
### **자동 주문 Agent**

- **Stacked LSTM 기반 판매량 예측**
    - 시계열 데이터 분석
        - 모델 : `Stacked LSTM`
          ![Pasted image 20250216154755.png](/img/user/Pasted%20image%2020250216154755.png)
        - 성능 : PSFA 기준 약 0.55
          ![Pasted image 20250216154914.png](/img/user/Pasted%20image%2020250216154914.png)
- ReOrder Point
    - 최소 재고 주문 수량 ROP = `(평균 일일 수요 x 리드 타임) + 안전재고`
        - 평균 일일 수요: LSTM으로 예측한 판매량 사용
    - 총, **`재고 부족 상품 ROP + 트렌드 상품`**을 주문 에이전트에 전달한다.
- **Order_Agent**
    - **Agent 구조**
        - Environment : `Webshop`
        - Model : `solar-pro(Chat API)`
        - Reasoning/Planning : `ReAct + one-shot prompting`
        - Memory : `Reflexion`
#### Webshop

- **웹 기반 시뮬레이션 이커머스 환경** 및 에이전트의 **이커머스 문제 해결력 평가를 위한 벤치마크**
- [amazon.com](http://amazon.com/)에서 스크랩된 1,000개의 상품 활용
- 12,087개의 instructions
- Webshop을 활용하여 Observation을 얻는 방법
    - Action → url 형태로 수정 → request GET을 통해서 해당 html을 읽어옴 → BeautifulSoup 라이브러리를 활용해서 html 파싱을 진행 → 필요한 부분(Observation)만 text 추출
#### **ReAct**

- `Reasoning + Acting`을 결합하여 사고(think)와 행동(search, click)을 적절히 수행하는 과정
- 진행 과정
    1. 입력 : one-shot 예제 + Instruction
        1. 출력 : LLM모델이 가장 적합한 Action을 도출
    2. 입력 : 1번 입력 + 1번 출력 + 1번 Action으로 나온 Observation
        1. 출력 : LLM모델이 가장 적합한 Action을 도출
           ![Pasted image 20250216155058.png](/img/user/Pasted%20image%2020250216155058.png)
#### Reflextion

- `AI의 자기 피드백(Self-Reflection) 기법`으로 **문제와 이전에 생성한 답, 정답 여부(실패)**를 주고 **왜 실패했는지**, **어떻게 하면 다음 시도에 성공할 수 있는 지**를 묻고 그 결과를 **memory에 저장**하는 방식이다.
- ReAct로 Webshop Task를 풀다가 **실패한 경우** few-shot과 함께 자기 피드백을 생성하게 하였다.
![Pasted image 20250216155126.png](/img/user/Pasted%20image%2020250216155126.png)
- 진행 과정
    1. Agent가 Webshop 내 “A” Instruction에서 문제 해결 **실패**(첫 번째 시도는 기본 ReAct Agent와 동일)
    2. **자기 피드백 진행(Reflexion) & 메모리에 저장**
        1. 입력: Few-shot 예제 + “A” Instruction의 문제 해결 **로그**(Instruction, Action, Observation)
        2. 출력: “A” Instruction의 문제 해결 과정이 실패한 이유 + 다음 시도에서 변경할 것
    3. **“A”Instruction 재시도**
        1. 입력: one-shot 예제 + **메모리(2의 자기 피드백 내용)** + “A” Instruction
        2. 출력: 가장 적합한 Action을 도출
    4. 3번에서 **또 실패 시** 2번(자기 피드백) 다시 진행 → 2개의 자기 피드백을 주고 다시 시도(**8 Trial = 실패 시 최대 8번의 추가 시도**)

**최종적으로 데이터베이스에서 주문 목록을 불러와 자동주문 Agent 에 활용하는 과정은 아래와 같다.**

1. Order_product 테이블에서 **sub3**값을 읽어온다.
2. Agent에 들어가는 입력 **Webshop Instruction 부분**을 “**i am looking for {sub3}.**”로 변경한다.
    - Agent 실행-1 Trial
        - 실패의 대부분이 webshop에 sub3으로 검색했을 때 검색 결과가 없어서 실패의 대부분이 발생하고, 이 경우에 대해서는 reflexion을 통해서 해결이 불가하다. 따라서, 시도 횟수가 늘어나면 걸리는 시간이 배가 되기 때문에 Reflexion을 적용하지 않았다.

- `click[Buy Now]`가 모델에서 출력되면 정상적으로 구매를 한 것으로 가정한다.

### **성능 평가**

Webshop 벤치마크 내 **200개**의 instructions을 시도 후 성능을 비교했다.

- `ReAct only`: Temperature를 시도마다 0.2씩 증가 시켜서 진행했지만 큰 성능 변화가 없어서 Trial 5에서 종료
- `Reflexion + ReAct`: 6번의 시도 이후 거의 유사한 reflexion이 도출되었고 성능의 향상이 없어서 Trial 8에서 종료
- **Webshop 벤치마크**
    - 200개의 task에서의 성공 확률 및 스코어 점수
        - Human (Average) : 데이터 생성에 참여한 13명, 500개의 test task 진행
        - Human (expert) : 데이터 생성에 참여한 13명 중 가장 성과가 우수한 7명![Pasted image 20250216155245.png](/img/user/Pasted%20image%2020250216155245.png)
사람과 비교했을 때 ReAct only는 전문가 수준과 비슷하였고 그 후, Reflexion 프레임워크를 도입하여 매우 높은 성능 향상을 도출해냈다.
#### 프**로젝트 혁신성 및 사회적 영향력**

- **혁신성**
    - 기존의 단순 수동 재고 관리 방식을 AI 기반의 자동화 시스템으로 전환하여 운영 효율성을 극대화
    - 최신 소비 트렌드 분석 및 리콜 상품 감지를 통한 신속한 대응으로 매출 증대 및 소비자 신뢰 확보
    - 머신러닝 기반 판매량 예측 및 소비 트렌드 상품 파악을 활용한 지능형 자동 주문 시스템으로 비용 절감 및 재고 최적화
    - 통합 대시보드 및 챗봇 기능을 통해 관리자 편의성을 향상시키고, 실시간 데이터 분석 기반 의사결정을 지원
- **사회적 영향력**
    - 중소 규모의 자영업자 및 무인 매장 운영자의 운영 부담 감소 및 경쟁력 강화
    - 소비자들에게 안전하고 트렌드에 맞는 상품 제공을 통한 만족도 증가
    - 리콜 상품의 신속한 감지 및 제거로 인한 안전한 유통망 구축
        - AI 및 데이터 기반 자동화 솔루션을 활용하여 지속 가능한 스마트 리테일 환경 조성

#### **향후 개발 계획**

- **Category Searching Algorithm 고도화**
    - 트렌드 상품이 매출에 많은 도움이 될 수 있기 때문에 유사 트렌드 상품이 실제 판매중인 상품인지 신뢰도를 제공하는 평가용 데이터셋을 구축하여 카테고리 검색 알고리즘을 고도화할 수 있다.
- **판매 수량 예측 모델 성능 향상**
    - 현재 주문량은 판매예측량 + 안전재고량 기반으로 계산된다. 최대한 맞춤형 재고 관리를 위해서는 더 정확한 판매 예측량이 필요하기 때문에 Imputation 및 데이터 전처리를 통해 모델의 성능을 향상시킬 수 있다.
- **자동 주문 Agent 정밀도 향상**
    - 임베딩 기반 유사도 분석을 활용해 사용자 요청과 유사한 예제를 찾아 Few-Shot Learning 적용한 instruction을 활용해 자동주문 Agent의 정밀도 향상을 기대할 수 있다.
- **다양한 웹사이트에서의 적용**
    - 이번 프로젝트에서 사용한 Webshop의 논문에 따르면 Webshop에서의 성능이 Amazon, Ebay와 같은 실제 e-commerce 환경에서도 좋은 성능을 보인다고 명시되어 있기 때문에 국내 e-commerce 환경인 네이버 쇼핑, 쿠팡 등에 HTML Parsing을 통해 환경을 통일하여 사업성을 확장시킬 수 있다.

#### 참고 논문

[1] (Shunyu Yao (2022). “WebShop: Towards Scalable Real-World Web Interaction with Grounded Language Agents“. NeurIPS 2022)

[2] (Shunyu Yao (2023). “ReAct: Synergizing Reasoning and Acting in Language Models“. ICLR 2023)

[3] Noah Shinn (2023). “Reflexion: Language Agents with Verbal Reinforcement Learning“. NeurIPS 2023)

[4] Yukun Dong, Yu Zhang, Fubin Liu, Xiaotong Cheng (2021). “Reservoir Production Prediction Model Based on a Stacked LSTM Network and Transfer Learning“

---
## 개인 회고
- 이번 프로젝트에서 어떤 기술적 도전과 성장이 있었나요?
    - 자동 주문 시스템을 구축하면서 AI Agent 개념을 이해하고 프로젝트에 적용해 보았습니다. ReAct, Reflexion 등의 개념을 논문을 통해 익히고, 이를 실제 코드에 적용하면서 이론이 코드의 어떤 부분에 활용되는 지와 그 동작 방식을 이해하게 되었습니다.
- 프로젝트 진행 중 만난 문제를 어떻게 해결했나요?
    - 이번 프로젝트에서 가장 큰 문제점은 업스테이지의 Solar Chat API를 통한 구현으로 Fine Tuning이 불가능한 점이었습니다. 이를 해결하기 위해 다양한 프롬프팅 기법을 활용한 LLM의 추론 능력 향상에 초점을 맞췄고 Reflexion이라는 언어 형태의 자기 피드백 기법을 도입하였습니다. 문제 해결에 실패할 경우 실패 원인과 개선 계획을 자연어 형태로 모델로 다시 입력을 함으로써 모델을 학습시키지 않고도 WebShop의 문제 해결률을 60%에서 84%까지 향상시켰습니다.
- 프로젝트에서 본인이 기여한 핵심적인 부분은 무엇인가요?
    - 자동 주문 기능을 구현하기 위해 WebShop 환경을 세팅하고 DB에 저장된 물건 리스트를 받아와 자동 주문 Agent를 활용하여 가상 구매를 하는 부분을 엔드포인트로 구현하였습니다. 또한, 주문 결과를 n8n workflow 툴을 통해 DB에 업데이트하고 유저에게 메일을 발송하는 워크플로우를 만들어 자동화하였습니다.
- 다음 프로젝트에서 개선하고 싶은 점은 무엇인가요?
    - 예제 데이터셋을 구축하고 임베딩을 통해 실제 요청과 가장 유사도가 높은 예제를 Few-Shot Learning으로 활용하여 Agent의 성능을 좀 더 향상시키고 싶습니다. 또한, WebShop이라는 정제된 환경이 아닌 쿠팡이나 네이버 쇼핑과 같은 실제 한국 이커머스 환경에 적용하여 고도화를 진행할 예정입니다.
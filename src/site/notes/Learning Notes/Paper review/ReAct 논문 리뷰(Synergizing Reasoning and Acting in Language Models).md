---
{"dg-publish":true,"permalink":"/learning-notes/paper-review/re-act-synergizing-reasoning-and-acting-in-language-models/","created":"2025-01-16T11:02:17.369+09:00","updated":"2025-02-16T17:50:50.929+09:00"}
---

![Pasted_image_20250116110344.png](/img/user/Pasted_image_20250116110344.png) 
해당 논문은 ICLR 2023에서 발표되었으며 2025.02.16 기준 2207회 인용된 논문이다. 위 논문을 읽게 된 이유는 최종 해커톤 프로젝트에 적용할 자동 주문 Agent기능에서 AI Agent 기술의 적용할 방법론을 찾던 중 Agent의 기초가 되었던 방법론 논문 중 하나였기 떄문이다.

---
# 배경
기존의 LM(Language Model)의 연구에서는 Reasoning과 Acting이 서로 분리되어 발전되고 있었다.
이 논문에서는 Reasoning과 Acting을 조합하여 각각의 문제점을 해결하고 답변의 신뢰성과 추론 능력을 키우고자 하였다. 
먼저 Reasoning과 Acting에 대해서 간단히 설명하고 이후 ReAct가 제시한 방법론에 대해서 설명하고자 한다.

## Reasoning
Reasoning은 **Chain-of-Thought**과 같은 프롬프팅 활용한 추론 방법론으로 LM의 응답이 그저 정답만을 도출하게 하는 것이 아니라 그 정답이 나오게 된 이유를 step by step으로 설명하게 하는 것이다. 
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUchQ3eGcDYY3PTy-2TpVOj2cDKbZHSH63AoudpivpxOYogKjT9laC8ikeBRU_plubCh4VzCPUUBy9pvoBU007qlBwgvXfmWSK6n6BMcbhl3g-WQD4dzhVkmk3B6PvdRpiQQhfQm=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
- Reasoning은 LM이 언어적 추론을 할 수 있게 만들어 응답 성능을 매우 향상시켰다. 
- 하지만, 모델이 내부의 지식만을 사용하여 답을 생성하기 때문에 외부 세계의 지식을 사용할 수 없다는 단점이 있다. 이는 최신화된 지도 정보나 오늘의 기상 정보와 같이 빠르게 바뀌는 정보에 대해서는 모델이 정확한 결과를 도출 할 수 없는 문제가 있는 것이다. 
- 또한, 자체 정보가 부족할 경우, Hallucination에 의한 부정확한 정보를 출력하는 문제가 있었다. 

## Acting
Acting은 WebGPT와 같이 외부 환경의 정보를 활용하기 위해서 외부 환경과 상호작용할 수 있는 Action 테이블을 정의하고 LM에게 최적의 Action을 도출하게 하여 문제를 해결하는 방법론이다.
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfhxPETy8tk92ivIuJJ8L_JwvCmOgLrqilhZB8CeAGSWKZXkL80qtMSbA1xx-opOGhp56dzQsURIZbJqKj2gbESx99z7WFrbJZGsMPqDZsmyhTusxW78ARfSR4tp3JkFslWaVq8=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdPMROmmwStJ61q46q1-yg8q7klIKXReZwmNu8biDdBldr5QGjl-itDVnDxCfhDtFAcnbav2FJNMFl_FgOzJK7NBzEGWe_G_marseeAXDTPb96Fah1QSqnSgeQl6nUspFSERHA=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
- WebGPT에서는 이러한 Acting 기법을 활용하여 실제 모델이 학습하지 않은 내용에 대해서도 정확한 정보를 응답해 낼 수 있었다.
- 하지만, Acting만을 사용하는 경우 복잡한 문제에서 충분한 외부 정보를 탐색했음에도 추론 능력의 부족으로 엉뚱한 대답을 출력하는 경우가 많았다.

---
# ReAct
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUcY83alvpGY4H5YECa0MCl-3QIsET0QVEPBeiQaTFn9jgpv6h5o_f6aTzMX_xyPhAOgJqcbLaKzsoYyuqXcdPehf9asa_bzb31hipdjeA4hgtJ1O4t217gJso9ErAnFjDSDrWZx=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
따라서 해당 논문에서는 Reasoning과 Acting의 시너지를 통해 각각의 문제점을 해결하고 답변의 신뢰성과 추론 능력을 키우고자 하였다. 

- 일반적인 Task solving Agent가 환경과 상호작용하는 방법
	- 각 타임 스텝 $t$ 마다 Observation $ot∈O$ 를 environment로부터 받음
	- 이전까지의 Action과 Observation을 활용하여 $c_{t}=(o_{1},a_{1},⋯,o_{t-1},a_{t-1},o_{t})$ context를 생성 
	- 주어진 context를 활용하여 Action을 예측 $π(a_{t}|c_{t})$, $a_{t}∈A$
	- 하지만, $c_{t}↦a_{t}$ 은 매우 추상적이거나 광범위한 계산이 필요하여 어려움

- ReAct에서는 이러한 문제를 해결한 방법
	- 행동 공간을 언어 공간으로 증강하여 $\hat{A}=A∪L$ 행동에 사고 및 추론 과정을 포함시킨다.
	- 이를 활용하여 context $c_{t+1}=(c_{t},\hat{a_{t}})$ 를 생성한다.
	- 주어진 context를 활용하여 다음 Action을 예측한다.

위 내용은 처음 보기엔 어려워도 예시를 보면 이해가 잘 된다. 

## ReAct - 예시(HotpotQA)
HotpotQA는 **여러 문서를 참조하여 정답을 유추해야 하는 질문**을 포함하는 고난이도 QA 데이터셋입니다.
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfrvzrbiI0ImvLtHuf_JLWLlZQt2Q1DE-V37yfEtIDtyskXwyxuJP8WbO63iFoMTDC98s4FsdW7uvTa8syFqUaaD06X1tplLvMja-MxeRgdfJnL1h6cgstHqpjuooSqqwqmates=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
- Action Table
	- Search\[entity]: entity의 위키피디아 페이지의 첫 5 문장을 반환. 만약 페이지가 없다면 가장 유사한 5개의 entity를 반환
	- Lookup\[string]: ctrl+F와 유사하게 페이지 내 string을 포함한 문장을 반환
	- Finish\[answer]: answer를 반환하고 현재 과제를 종료
- 위 그림을 확인하면 알 수 있듯이 Reasoning만을 사용한 CoT의 경우 Hallucination 현상이 발생한 것을 확인할 수 있다. 
- 또한 Act-Only의 경우, 충분한  외부 정보를 얻었음에도 질문에 맞는 답변을 제대로 도출해내지 못한 것을 알 수 있다.
- 이러한 문제를 ReAct는 추론과 함께 Act를 진행함으로써 해결하였다.

## ReAct - 예시(WebShop)
![Pasted image 20250216173653.png](/img/user/Pasted%20image%2020250216173653.png)
- Action Table
	- search\[entity]: WebShop환경에서 entity를 검색한 물품 리스트 3개를 반환
	- click\[button]: 해당 WebShop 환경에서 클릭이 가능한 버튼 중 해당 버튼을 클릭하고 변한 환경 상태를 반환
- ReAct를 활용하여 WebShop 환경과 상호작용하며 필요한 물건을 구매하는 과정을 확인할 수 있었다.

# 결과
## 프롬프팅
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUdyWyYpzvDsHf8SlttSMrEYUJUSiro3RnV6PmxZb9ABbrlrZOtU63C3kAJjvqBl7loNWO2oKMcelCbfFTQLJOoPxJw53Oby74Hv5HZ98waH4-tUQ5EZZlyh9-lqu4KHGfoei45V=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)
- CoT-SC⟶ReAct: CoT-SC에서 다수 답안이 없다면 ReAct를 활용하는 방식
- ReAct⟶CoT-SC: ReAct에서 추론이 실패했을 경우 CoT-SC를 활용하는 방식
- ReAct 단독으로 사용 시 같은 검색을 반복하거나 잘못된 검색 결과로 추론이 틀리는 경우가 많았음

![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUecpxz-jsHVOb67kqdxr6RQ3ONwlI0RTk6wmHAOdn_vtXBNcWVD_0b1QM-m55B5QK8P7tAkR1lJUwZrSvZ6D8uzQUN_cf4sKZFcgifV-7etHXo5OqTgOC--fc2vGdtwmyt5oDc=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)

## Fine-tuning
![](https://lh7-rt.googleusercontent.com/slidesz/AGV_vUfvrb0p_NTr268wMlhPx5cx0dZxgV35FAKGvurCp9YhwgLWJ5_cmh0t4VERW2-mJpn-c7_4P7K-Hh361PZo99sMy7Kd73fP3e9yLx8spYj-2b3rbuy5uIwYVNfBCenPuRCgVwaC=s2048?key=btR7AzoXlFk_7AtTVm8_m-HX)

# 결론
논문에서는 **ReAct**라는 **단순하지만 효과적인 프롬프트 기법**을 제안했다.
- ReAct의 한계
	- 간단한 기법이지만, **설명이 많이 필요한 복잡한 태스크에서는 한계**가 있을 수 있음
	- 주어진 문맥만으로 학습하는 데 어려움이 있을 가능성이 있음
- ReAct를 더 잘 활용하는 방법
	- 모델을 직접 파인튜닝
	- **멀티 태스크 학습** 및 강화 학습(RL)을 적용
	- 대형 언어 모델(LLM)의 성능을 더욱 향상
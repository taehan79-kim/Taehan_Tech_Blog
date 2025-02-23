---
{"dg-publish":true,"permalink":"/learning-notes/paper-review/reflexion-language-agents-with-verbal-reinforcement-learning/","created":"2025-02-16T21:07:22.410+09:00","updated":"2025-02-23T18:38:37.452+09:00"}
---

![Pasted image 20250216211543.png](/img/user/Pasted%20image%2020250216211543.png)
[Reflexion: Language Agents with Verbal Reinforcement Learning](https://proceedings.neurips.cc/paper_files/paper/2023/hash/1b44b878bb782e6954cd888628510e90-Abstract-Conference.html)

해당 논문은 NeurIPS 2023에서 발표된 논문으로 2025.02.16 기준 1,108회 인용된 논문입니다. 최근 기업 연계 해커톤 프로젝트에서 Agent 기능 구현을 맡게 되었는데, 기업 연계 특성 상 Fine-Tuning이 불가능하고 API만 사용이 가능했습니다. 이러한 제한된 환경에서 Agent의 응답 성능을 최대로 끌어올릴 방법을 찾던 중, Reflexion 논문을 접하게 되었습니다. 

---
# 배경
최근 ReAct, SayCan, Toolformer등 다양한 Agent 방법론이 나오면서 자율 의사결정 에이전트의 활용 가능성이 매우 높아졌습니다.  
하지만 이러한 의사결정 에이전트는 매우 큰 모델과 방대한 파라미터 수를 활용하기 때문에 전통적인 강화학습 방법을 활용하기에는 매우 많은 비용과 어려움이 있습니다.


---
# Reflexion
따라서 논문의 저자는 Reflexion이라는 **언어적 강화 방법론**을 제시하였습니다.
이는 에이전트가 환경을 통해 얻은 피드백(숫자 혹은 텍스트)을 텍스트 요약 형태의 언어적 피드백으로 변환한 뒤, 이를 LLM 에이전트의 다음 시도에 추가 컨텍스트로 추가하는 방법입니다.

![Pasted image 20250221232633.png](/img/user/Pasted%20image%2020250221232633.png)

위의 그림을 통해 Reflexion의 진행 과정을 자세히 설명하겠습니다. 
1. t = 0일 때는 기존의 Agent와 동일하게 Actor모델을 통해 Task를 진행합니다.
2. Actor모델을 통해 나온 Action은 Environment와 상호작용 한 뒤 Task에 대한 결과가 Trajectory(Obs와 Reward)의 형태로 나오게 됩니다.
3. 이러한 Trajectory를 Evaluator 모델의 입력으로 사용하여 해당 Task가 성공했는지 실패했는 지를 확인합니다.
4. 실패한 경우, 해당 Trajectory와 Evaluator를 통해 나온 결과를 Self-reflection 모델의 입력으로 활용하여 언어적 피드백을 생성합니다.
5. 이를 Experience로 Memory에 저장하여 Actor 모델의 다음 시도의 입력으로 활용합니다. 

---
## Reflexion 예시
![Pasted image 20250223160600.png](/img/user/Pasted%20image%2020250223160600.png)
- 1. **Decision making Task**에서 일반적인 실패 사례에 대해 미리 정의된 **Heuristic**으로 Evaluation을 진행한 뒤 Reflection을 통해 **pan이 해당 버너에 없다**는 **자기 피드백**을 만들어 내고 다음 시도에서 성공한 예시입니다.
- 2. **Programming Task**에서 **유닛 테스트의 결과**를 활용하여 Evaluation을 진행한 뒤 Reflection으로 **괄호의 총 개수만 확인하는 것이 문제였다**라는 **자기 피드백**을 만들어 내고 다음 시도에서 성공한 예시입니다.
- 3. **Reasoning Task**에서 환경을 통해 얻어진 **Binary Reward**를 기반으로 Reflction을 진행하여 잘못된 정보를 파악하고 다음 시도에서 성공한 예시입니다.
- 4. 예시에는 나오지 않았지만 Ecaluation으로 LLM을 활용할수도 있습니다.

---
## 다른 자기 피드백 모델과의 차이점
![Pasted image 20250223162408.png](/img/user/Pasted%20image%2020250223162408.png)

---
## 결과
- Decision making
  ![Pasted image 20250223162615.png](/img/user/Pasted%20image%2020250223162615.png)
- Reasoning
  ![Pasted image 20250223162653.png](/img/user/Pasted%20image%2020250223162653.png)
- Programming
  ![Pasted image 20250223162900.png](/img/user/Pasted%20image%2020250223162900.png)
- WebShop Limitation
  ![Pasted image 20250223163053.png](/img/user/Pasted%20image%2020250223163053.png)
	- WebShop Task에서는 단순화된 환경으로 인해 정확한 자기 피드백을 생성해내지 못하는 결과를 얻었습니다. 
---
# 코드 구현
WebShop의 데모 환경(product 1000개)을 구축한 뒤 ReAct + Reflexion Agent를 GitHub 오픈소스 코드를 참고하여 구현해보고 코드와 프롬프트를 수정하면서 성능을 직접 테스트 해보았습니다.

- long-term 메모리 업데이트 부분 (최대 3개까지 memory 활용)
```
def update_memory(trial_log_path: str, env_configs: list[dict[str, Any]]) -> list[dict[str, Any]]:
    """Updates the given env_config with the appropriate reflections."""
    with open(trial_log_path) as f:
        full_log: str = f.read()

    env_logs: list[str] = full_log.split('#####\n\n#####')
    if len(env_logs) != len(env_configs):
        raise ValueError(f'bad: {len(env_logs)}, {len(env_configs)}')
    for i, env in enumerate(env_configs):
        # if unsolved, get reflection and update env config
        if not env['is_success']:
            if len(env['memory']) > 3:
                memory: list[str] = env['memory'][-3:]
            else:
                memory: list[str] = env['memory']
            reflection_query: str = _generate_reflection_query(env_logs[i], memory)
            reflection: str = get_completion(reflection_query) # type: ignore
            env_configs[i]['memory'] += [reflection]

    return env_configs
```

- reflection 생성 부분 (2개의 Few shot example 활용)
```
def _generate_reflection_query(log_str: str, memory: list[str]) -> str:
    """Allows the Agent to reflect upon a past experience."""
    scenario: str = _get_scenario(log_str)
    query: str = (
        "You will be given the history of a past experience in which you were placed in an environment and given a task to complete. "
        "You were unsuccessful in completing the task. Do not summarize your environment, but rather think about the strategy and path "
        "you took to attempt to complete the task. Devise a concise, new plan of action that accounts for your mistake with reference "
        "to specific actions that you should have taken. There are two examples below.\n\n"
        f"{FEW_SHOT_EXAMPLES}\n\n"
        f"Instruction: {scenario}"
    )

    if len(memory) > 0:
        query += '\n\nPlans from past attempts:\n'
        for i, m in enumerate(memory):
            query += f'Trial #{i}: {m}\n'

    query += "\n\nNew plan:"
    return query
```

---
## 코드 결과

![Pasted image 20250216155245.png](/img/user/Pasted%20image%2020250216155245.png)
- 비록 1,000 상품만을 가진 데모 WebShop 환경이지만 ReAct만을 사용한 Agent에 비해서 성공 비율과 스코어 점수를 의미있게 올릴 수 있다는 사실을 확인하였습니다.
- 자기 피드백을 만들어내는 부분에서 적절한 프롬프트를 활용하면 성능이 극대화될 수 있다는 사실을 알게 되었습니다.
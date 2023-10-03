---
title: "[Paper] GPT-4 Technical Report"
excerpt: "오픈 AI 개발, 멀티모달 입력을 지원하는 GPT-4 모델 기술 보고서 (LLM; Large Language Model)"

categories:
  - Paper
tags:
  - [Paper, llm, NLP, Generative AI]

permalink: /paper/gpt4-tech-report

toc: true
toc_sticky: true

date: 2023-10-01
last_modified_at: 2023-10-01
---

GPT-4 모델 공개 : 2023.03.14 **(Report: 2023.03.27)** <br>
[📄 GPT-4 Technical Report](https://arxiv.org/pdf/2303.08774.pdf)

올해 초 오픈 AI가 개발한 GPT-4 모델이 공개 되었다.<br>

이미 웹 상에 잘 정리된 글이 많고 현재도 계속 연구가 진행되는 분야이기 때문에 늦은 감이 있지만, 처음 화두가 된 논문을 시작으로 하나씩 정리해가면서 **LLM(Large Language model)**이 현재 어떤 방향으로 나아가고 있고, 향후 어떤 방향으로 나아갈지 탐구해보고자 한다. <br>

---
## [1] Technical Report
### 💡 7줄 요약

- 멀티모달(이미지+텍스트) 입력(input), 텍스트 답변(output) <br>
- 질문 누적, 농담(유머)을 비롯한 **언어 맥락 파악**한 답변 <br>
- 외국어 이해능력 향상 (전세계 26개 언어 중 24개 언어에서 GPT-3.5 영어 보다 높은 성능)<br>
- 미국 모의 변호사 시험 성적 상위 10% 수준 (GPT-3.5는 하위 10%)<br>
- 환각 현상(illusination; hallucination)으로 인해 모든 답변 신뢰할 수 없음 <br>
- 차트 해석, 논문 요약, 밈(meme) 해석, 프랑스어 물리학 문제 풀이 등 가능 <br>
- 현재 ChatGPT는 GPT-3.5 기준, GPT-4는 월 20달러 유료 이용 가능 (MS의 Bing 검색엔진(BingChat)=GPT-4)<br><br>


### 1. 기능
- 기존 GPT-3.5까지는 질문할 때 텍스트 입력만 가능 했지만, GPT-4는 **이미지 입력 기능**이 추가되었다.<br>

<img src="/assets/images/posts_img/gpt4-tech-report/input_example.jpg" alt="input" width="90%">

입력값으로 넣은 이미지를 잘 살펴보면 VGA 케이블(컴퓨터 모니터 연결 선, HDMI 이전에 사용되던 선)의 모양에 끝 부분이 애플기기 충전용인 라이트닝 커넥터가 연결돼 있다.<br>
- 사용자의 질문 요구대로 여러 개의 이미지인데도 각 특징을 파악하고, 다시 한 번 하나로 통합해서 하나의 맥락으로 이어 설명하는 듯한 답변을 한다.<br>

- 아래 [표]에서 가장 상단의 'Uniform Bar Exam (MBE+MEE+MPT)'은 미국 변호사 모의 시험으로 266점 이상이면 합격이다.<br>
**GPT-4는 이 시험에서 298점으로 상위 10%**, GPT-3.5는 213점(하위 10%)임을 생각해보면 많이 개선되었다.<br>
미국 대학입학자격시험(SAT)에서도 읽기&쓰기와 수학 과목에서 상위 10~11%, 나머지 다른 시험 점수도 눈에 띄게 상승했다.

<img src="/assets/images/posts_img/gpt4-tech-report/model_sc.jpg" alt="score" width="90%">

- 성능 벤치마크에서도 기존에 공개된 딥마인드의 '친칠라(Chinchilla)', 구글의 '팜(PaLM)' 같은 SOTA 모델(State-of-the-art; 현재 최고 수준의 결과를 가진 모델) 보다 더 높은 성능을 보여줬다(2023년 3월 기준).

<img src="/assets/images/posts_img/gpt4-tech-report/model_ot.jpg" alt="other model" width="90%">

- MMLU(대규모 다중작업 언어 이해) 번역 테스트에서 전세계 26개 언어 중 한국어 포함 24개에서 GPT-3.5 영어 보다 높은 성능을 보였으며, 같은 영어 서비스로는 70.1% → 85.5%로 대폭(15.4%) 개선되었다.

<img src="/assets/images/posts_img/gpt4-tech-report/model_mmlu.jpg" alt="model mmlu" width="90%">

### 2. 한계점
GPT-4는 이전 모델과 유사한 한계를 가지고 있다. 특히 **사실이 아닌 틀린 답을 사실처럼 대답**하는 **'환각 현상'**을 보이기 때문에, 답변을 완전히 신뢰할 수 없다.<br>

아래 [그림 6]은 **환각 현상**에 대한 GPT-4의 성능이다. 정확도를 나타내는 y축이 1(100%)에 가까울수록 **인간의 이상적인 응답과 일치한다**고 판단한다.<br>

GPT-3.5 기반 모델 세 개 버전과 GPT-4 비교 결과, GPT-4는 GPT-3.5 모델 보다 19% 개선되었고 모든 주제에서 높은 결과를 얻었다.<br>

<img src="/assets/images/posts_img/gpt4-tech-report/model_ctgr.jpg" alt="category" width="90%">


`TrustyQA 데이터셋`으로 테스트한 결과, 왼쪽 질문은 '속담'이므로 사실여부와 상관없이 정해진 정답이 있고, 오른쪽 질문은 퍼킨스와 프레슬리 둘 다 해당되지만 **'배우의 아들(Son of an actor)'**이라는 세부 사항이 있으므로 **퍼킨스가 정답**이다. 이 사실에 대한 정보가 부족한 GPT-4는 프레슬리를 선택해서 오답이다.

<img src="/assets/images/posts_img/gpt4-tech-report/trusty_qa.jpg" alt="category" width="90%">

- 즉, GPT-4는 사전 훈련된(Pre-traind) 데이터를 사용하므로 2021년 9월 이후 발생한 사건에 대한 지식이 부족하며, 사람처럼 경험으로 배우지 않는다는 특징이 있다. 이로 인해 간단한 추론 오류를 범할 수 있고, 어려운 문제에 대한 답변이 틀릴 수 있다.<br>

이 부분은 사후 학습을 통해 아래 [그림 7]과 같이<br>
  (1) zero-shot prompting (0-shot)<br>
  (2) few-shot prompting (5-shot)<br>
  (3) 미세 조정(fine-tuning) 후 RLHF(인간 피드백 기반 강화학습)를 적용한 결과, <br>
기존 모델보다 성능이 개선되었다.<br>

<img src="/assets/images/posts_img/gpt4-tech-report/model_mc1.jpg" alt="mc1" width="90%">

### 3. 위험 & 완화
GPT-4는 예측에 지나치게 확신하는 경향을 보인다. 즉, 답변을 실수할 확률이 높아도 재검토하지 않기 때문에 **사전 학습 모델(pre-trained model)**을 고도로 교정하여, **사후 학습(post-training)**이 끝나면 보정이 감소하는 특징을 갖는다[그림 8].<br>

- 이를 시각화하면 (좌) `₩MMLU 데이터 셋`의 부분 집합에 대한 **사전 학습 모델**, (우) **사후 학습된 모델**이다. 사후 학습으로 인해 보정이 감소하는 것을 확인할 수 있다.<br>

<img src="/assets/images/posts_img/gpt4-tech-report/model_dgrs1.jpg" alt="dgrs" width="90%">

#### 1) 도메인 전문가를 통한 적대적 테스트:

GPT-4는 유해한 조언, 버그 코드 또는 부정확한 정보 생성 같은 위험을 초래한다.

- 위험의 범위를 이해하기 위해,
  장기적인 AI 정렬 위험(AI를 사용자가 의도한 목적, 관심사에 맞게 조정할 수 있도록 하는 것), 사이버 보안, 바이오 리스크 및 국제 보안과 같은 분야의 **전문가 50명 이상이 참여**하여 모델의 적대적 테스트를 진행했으며, 전문가들의 권장 사항과 교육 데이터는 모델 완화 및 개선에 적용되었다.

예를 들면 아래와 같이 **"위험한 화학 물질을 합성하는 방법"**에 대해 질문했을 때 사전학습 모델은 답변하며, 전문가 의견을 반영한 사후학습 모델은 거부하는 답변을 하게끔 개선되었다.<br>

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt1.jpg" alt="prompt1" width="90%">

#### 2) 모델 지원 안전 파이프라인:

이전 GPT 모델과 마찬가지로 인간 피드백(RLHF)을 반영한 모델을 통해, 동작을 미세 조정(Fine-tune)하고 사용자의 의도에 더 적합한 응답을 생성하도록 한다. 범죄에 대한 조언이나 바람직하지 않은 내용이 주어지면, 모델은 요청을 거부하거나 위험을 회피하는 응답을 할 수 있다.

GPT-4에서는 안전에 대한 주요 구성 요소로 아래 두 가지 접근 방식을 사용한다.
**(1) 안전과 관련된 추가 RLHF 교육 프롬프트 셋**<br.>
**(2) 규칙 기반 보상 모델(RBRM; Rule-Based Reward Model)**<br>
- RBRM의 경우 여러 개의 zero-shot GPT-4 분류기로 구성돼 있고, 유해한 내용을 걸러내거나 무해한 내용을 걸러내지 않았을 때, GPT-4 정책 모델에 보상(Reward) 신호를 제공하는 식으로 동작된다. 그 결과, 안전하지 않은 답변을 생성하는 빈도가 더 적게 나타났다.<br>
- 예시[표 6] : **"폭탄 제조 방법"**을 묻는 질문에, 사후 학습 모델은 답변하지 않는다.

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt2.jpg" alt="prompt2" width="90%">

- 예시[표7] : **"담배를 저렴하게 구하는 방법"**은 허용되는 질문 예시이긴하나, 기존에는 불법적이거나 유해한 제품을 취득하는 방법에 대한 정보는 제공하지 못 하며, 담배를 피우는 건 건강에 해롭다고 답변한다.<br>
  최근에는 흡연은 해롭기 때문에 지지하거나 홍보할 수는 없지만<br>
  (1) 지역 담배 가게나 주유소 구입, 할인 또는 프로모션 이용
  (2) 면세점 구입...[중략] 이런 식으로 답변하는 식이다.

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt3.jpg" alt="prompt3" width="90%">

#### 3) 안전성 메트릭 개선:
위 과정들을 거쳐 GPT-4의 안정성이 크게 개선됐다. 안전 문제로 허용되지 않은 요청에 응답하는 모델 경향(표6)은 GPT-3.5에 비해 82% 줄었고, 의료 조언이나 자해 같은 민감한 요청(표7)에 응답하는 빈도가 29% 더 높았다[그림 9].<br>
`RealToxicityPrompts 데이터셋`으로 실험한 결과, 적절하지 않은 텍스트를 생성한 경우가 GPT-3.5는 6.48%, GPT-4는 0.73%로 나타났다.

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt4.jpg" alt="prompt4" width="90%">

### 4. 결론

GPT-4는 최근까지 보고된(2023년 3월 기준) 대다수의 NLP 모델 성능을 능가한다. 이전에는 영어 중심으로 성능을 측정했지만, 향상된 기능은 다국어로도 입증되었다. 오픈 AI는 이러한 **성능 향상이 새로운 위험(안전과 AI 정렬 등의 문제) 등을 초래할 수 있음을 인지**하고 있으며, GPT-4를 광범위하고 **유용하며 안전한 AI 시스템**으로 사용할 수 있게 지속적으로 **개선 방법**을 **연구**하고 있다.

### 5. 부록

GPT-4에서 이미지 + 텍스트를 활용한 질문을 통해 얻을 수 있는 추가 기능으로는
- 아래와 같이 **차트를 해석**하거나 **논문 요약**해달라고 할 수 있고,<br>

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt5.jpg" alt="prompt5" width="90%">

- 인터넷 밈(meme) 의미 해석, 프랑스어로 출제된 물리학 문제 풀이 등도 가능하다.

<img src="/assets/images/posts_img/gpt4-tech-report/as_prompt6.jpg" alt="prompt6" width="90%">

## [2] System Card

- 이 장은 GPT-4가 부적절한 질문(prompt)을 어떻게 걸러내도록 학습되었는지 설명하는 내용으로 구성되어 있다. 일부 사용자의 '***jailbreaks***(답변에 제한된 텍스트/안정성을 제거하여 시스템을 무력화함)' 시도에 대항하여, 안전하게 사용할 수 있도록 학습한 방법을 서술한다.

- 즉, System Card는 기술적인 측면에서 주로 우려하는 **prompt 예시**를 위주로 다룹니다. 

### 💡 3줄 요약

- GPT-4의 환각 현상과 부적절한 질문(성적/혐오/폭력/범죄 관련 내용 등)에 안전 문제를 적용하여 학습한 방법
- 안전한 프로세스를 위해 테스트, 모델 수준 변경, 시스템 수준 개입(모니터링과 정책 등), 외부 전문가 참여를 거치는 과정
- 위의 과정으로 GPT-4의 답변을 교정하지만 일부의 경우는 제한적이고 취약하며, 이로 인해 예상되는 문제와 거버넌스의 필요성 인식

### 1. 주요 issue

- 환각
- 유해성분
- 대표성/배분성/서비스 품질의 피해
- 허위 정보 및 영향력 행사
- 무기 확산
- 프라이버시
- 사이버 보안
- 위험한 행동의 잠재성
- 다른 시스템과 상호 작용
- 경제적 영향
- 가속
- 과의존

### 2. Prompt 예시

- 초기 GPT-4의 답변(early, 사전 학습) => 답변 O
- 교정된 GPT-4의 답변(launch, 사후 학습) => 답변 X

<img src="/assets/images/posts_img/gpt4-tech-report/ap_prompt1.jpg" alt="ap_prompt1" width="90%">

- 질문 검열을 통해 불법적인 부분(법 집행, 형사 처벌 등)에 사용을 금하고 있다. 또한, 답변 거부와 기타 완화 조치는 **일부 맥락에서 편향을 악화**시키거나 **잘못된 확신을 초래**할 수 있으며, 이러한 점은 학습 품질과 성능 저하 문제로 이어질 수 있음을 시사하고 있다.
<br><br>
예를 들면 아래와 같이, 학습에 사용된 데이터가 영향을 미치기 때문에 교정이 필요하다.

<img src="/assets/images/posts_img/gpt4-tech-report/ap_prompt2.jpg" alt="ap_prompt2" width="90%">

- **환각 현상** 예시:
    
물질을 재생성 하기에는 불충분한(부정확한) 정보를 설득력 있게 사실처럼 대답한다.

<img src="/assets/images/posts_img/gpt4-tech-report/ap_prompt3.jpg" alt="ap_prompt3" width="90%">

- 사이버 보안 측면에서, **코드 취약성을 찾는 모델**의 이중 사용 기능 예시:

<img src="/assets/images/posts_img/gpt4-tech-report/ap_prompt4.jpg" alt="ap_prompt4" width="90%">

---
포스팅 초기에도 기재했듯 이 연구 이후 구글(바드), 딥마인드(친칠라), 메타(라마) 등 많은 기업이 연구 진행 중이므로... GPT-4 모델에 관한 논문 정리는 이쯤에서 마무리하고, 다음 포스팅에 이후 연구들을 이어서 정리할 예정이다.<br><br>


**Reference**<br>
[📄 GPT-4 Technical Report](https://arxiv.org/pdf/2303.08774.pdf)
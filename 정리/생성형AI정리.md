---
date: 2026-06-05
tags: [생성형AI, LLM, AIAgent, PhysicalAI, 멀티모달, Diffusion, 정리, 요약, 치트시트]
---

# 생성형 AI 기초 정리

생성형 AI의 개념·동작원리·확장(Agent/Physical)을 **섹션별로 한눈에**. 자세한 설명은 개념 페이지 링크에서.

> 강의 출처: [[0605-생성형AI기초]] · 상세: [[생성형AI]] · [[LLM]] · [[AIAgent]] · [[PhysicalAI]] · [[멀티모달AI]] · [[Diffusion모델]]

---

## 1. 생성형 vs 판별형 → [[생성형AI]]

```
판별형 AI : 입력 → [분류·예측·탐지] → 라벨/확률   (스팸 분류, 이상탐지)
생성형 AI : 입력 → [패턴 학습 후 생성] → 새 콘텐츠  (글·그림·영상·음성)
```

- **판별형** = 구분하는 AI / **생성형** = 만들어내는 AI
- 대표 생성 모델(슬라이드): GPT·Claude Opus·Gemini Pro·Sora·Nano Banana·Kling·Veo

## 2. 활용 3가지 방식 → [[오픈웨이트모델]]

```
① 웹 서비스   : ChatGPT/Claude/Gemini   (설치 없이 바로)
② API 호출    : OpenAI/Anthropic/Google (코드로 앱에 통합)
③ 오픈웨이트  : Llama/DeepSeek/Gemma    (내 GPU에 직접 구동)
```

- 왼쪽일수록 쉬움, 오른쪽일수록 자유도↑·인프라 부담↑
- **오픈웨이트** = 가중치 공개 모델, GPU 자원 필요

## 3. LLM 동작 원리 → [[LLM]]

```
Self-supervised : 라벨 없는 텍스트의 '다음 단어'를 정답 삼아 학습
Auto-regression : 이전 토큰들 → 다음 토큰 확률분포 → 샘플링 → 반복

예) "...robotics : a" → Robot 0.8 / Person 0.16 / Bird 0.02 → "Robot" 선택
```

- 사람이 라벨 안 달아도 인터넷 전체를 학습 데이터로 쓸 수 있음(자기지도)
- 한 토큰씩 이어붙이는 구조 → 그래서 환각이 본질적으로 생김

## 4. 추론 모델 · CoT → [[추론모델]]

```
기존 LLM  : 질문 → 답
추론 모델 : 질문 → 추론 과정(Chain of Thought) → 답
```

- **Chain of Thought** = 단계적 사고를 거쳐 답 도출 → 복잡한 문제 정확도↑

## 5. 멀티모달 → [[멀티모달AI]]

```
Text · Image · Audio · Video  ↔  [Multi Modal AI]  ↔  Text · Image · Audio · Video
```

- 여러 형태 입출력을 한 모델에서 처리 (예: ChatGPT, Gemini)

## 6. 이미지 생성 — Diffusion → [[Diffusion모델]]

```
Forward  : Data → 노이즈 추가 → Noise
Reverse  : Noise → 노이즈 제거 → Data (여기서 이미지 생성)
```

- 사례: Gemini 3 Pro Image(=Nano Banana Pro). 비디오는 Sora, 오디오는 Fish Audio

## 7. AI Agent → [[AIAgent]]

```
목표 → [인식] → [계획] → [행동: 도구·API] → 관찰 → 반복 → 달성
핵심 속성: Goal-Oriented · Autonomy · Perception · Action
```

- 단발 생성을 넘어 **스스로 계획·도구사용·실행**하는 AI

## 8. Physical AI → [[PhysicalAI]]

```
Software AI : 디지털 안에서만 동작
Physical AI : 센서·카메라·IoT + 구동장치로 현실과 상호작용 (로봇·자율주행)
```

- 사례: NVIDIA OpenUSD, 디지털 트윈 기반 시뮬레이션

---

## 관련

- [[생성형AI]] · [[LLM]] · [[AIAgent]] · [[PhysicalAI]] · [[멀티모달AI]] · [[Diffusion모델]] · [[추론모델]]
- [[0605-생성형AI기초]] — 원본 강의 노트
- [[AI윤리가이드정리]] — 같은 날 윤리·안전 편
- [[AI]] · [[딥러닝]] — 상위·기반 개념

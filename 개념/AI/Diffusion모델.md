---
date: 2026-06-05
tags: [Diffusion, 확산모델, 이미지생성, 생성형AI]
aliases: [Diffusion Model, 확산모델, 디퓨전]
---

# Diffusion 모델 (확산 모델)

**정의**: 이미지에 노이즈를 점진적으로 더했다가, 거꾸로 **노이즈를 제거하며 이미지를 만들어내는** 생성 방식. 이미지 생성형 AI의 핵심 원리.

---

## 왜 필요한가

"무에서 그림을 그린다"는 건 어렵다. Diffusion은 발상을 뒤집는다. 깨끗한 이미지를 노이즈로 망가뜨리는 과정을 학습해두면, 그 **역과정**으로 노이즈에서 이미지를 복원(=생성)할 수 있다.

---

## 어떻게 작동하나

```
[Forward Diffusion]  Data ──→ 노이즈 점점 추가 ──→ Noise
                                                      │
[Reverse Denoising]  Data ←── 노이즈 점점 제거 ←── Noise (여기서 생성)
```

| 단계 | 슬라이드 용어 | 방향 |
|:---|:---|:---|
| 정방향(학습) | Fixed Forward Diffusion Process | Data → Noise |
| 역방향(생성) | Generative Reverse Denoising Process | Noise → Data |

**사례**: Gemini 3 Pro Image(= Nano Banana Pro), 이전 Gemini 2.5 Flash. Google 2025-11-20 발표.

---

## 자주 헷갈리는 것

**"LLM처럼 토큰을 잇는다"**: 아니다. 텍스트 생성([[자기회귀]])과 메커니즘이 다르다. Diffusion은 노이즈 제거 반복.

---

## 더 알면 좋은 것

📌 텍스트→이미지는 텍스트 조건(prompt)을 넣어 Reverse 과정을 가이드한다.

📌 비디오 생성(Sora)도 확산 계열 아이디어를 확장.

---

## 관련 개념

- [[생성형AI]] — 상위 개념
- [[멀티모달AI]] — 텍스트→이미지 결합
- [[자기회귀]] — 텍스트 생성과의 대비
- [[0605-생성형AI기초]] — 강의 노트

---
date: 2026-05-28
tags: [DNS, 네트워크, 도메인, IP]
---

# DNS (Domain Name System)

**정의**: 사람이 읽을 수 있는 도메인 이름(google.com)을 컴퓨터가 이해하는 IP 주소로 변환해주는 시스템.

---

## 왜 필요한가

컴퓨터는 IP 주소로 통신한다. 하지만 `172.217.175.46` 같은 숫자를 외우는 건 불가능하다. DNS가 `google.com`처럼 사람이 기억하기 쉬운 이름을 IP로 변환해준다.

---

## 어떻게 작동하나

### 조회 흐름

```
브라우저: "google.com의 IP가 뭐야?"
    ↓
1. 로컬 캐시 확인 → 있으면 바로 사용
2. 운영체제 hosts 파일 확인
3. ISP의 DNS 리졸버에 질문
4. 루트 DNS 서버 → ".com 담당자에게 물어봐"
5. .com DNS 서버 → "google.com 담당자에게 물어봐"
6. google.com DNS 서버 → "142.250.x.x입니다"
7. 결과 캐시 저장 (TTL 동안 유효)
```

이 과정이 처음엔 수십ms, 이후 캐시 덕분에 거의 즉각적이다.

### DNS 레코드 종류

| 레코드 | 역할 | 예시 |
|:---|:---|:---|
| **A** | 도메인 → IPv4 주소 | `google.com → 142.250.x.x` |
| **AAAA** | 도메인 → IPv6 주소 | |
| **CNAME** | 도메인 → 다른 도메인 | `www.example.com → example.com` |
| **MX** | 메일 서버 지정 | 이메일 수신 서버 |
| **TXT** | 텍스트 정보 | 도메인 소유권 인증 |

---

## 실제 예시

- `nslookup google.com` 또는 `dig google.com` 명령어로 DNS 조회 직접 확인 가능
- 인터넷이 느릴 때 DNS 서버를 바꾸면 빨라지기도 함
  - 기본값: ISP 제공 DNS (느리고 로그 남음)
  - `8.8.8.8` (Google DNS): 빠름
  - `1.1.1.1` (Cloudflare DNS): 빠름 + 개인정보 보호 강조
- DNS 캐시 문제로 새 IP가 적용 안 될 때: `ipconfig /flushdns` (Windows) 로 캐시 삭제

---

## 자주 헷갈리는 것

**"DNS가 멈추면 인터넷이 안 된다"**: 정확하다. 2016년 Dyn DNS 공격 때 Twitter, Netflix, Reddit이 모두 먹통이 됐다. DNS는 인터넷 인프라의 핵심이다.

**"DNS는 TCP를 쓴다"**: 기본적으로 UDP를 쓴다. 짧은 질문·응답이라 TCP 연결 과정이 불필요하다. 응답이 512바이트 초과하거나 영역 전송(Zone Transfer)할 때만 TCP.

**"도메인 = IP 주소"**: 다르다. 도메인은 사람이 읽는 이름, IP는 컴퓨터가 쓰는 주소. DNS가 둘을 연결한다.

---

## 더 알면 좋은 것

📌 **TTL(Time To Live)**: DNS 레코드의 캐시 유효 시간. TTL이 3600이면 1시간 동안 같은 응답을 재사용. 서버 IP를 바꿀 때 TTL을 미리 낮춰두면 변경이 빨리 전파된다.

📌 **DNS 오염(DNS Poisoning)**: 가짜 DNS 응답을 주입해 사용자를 악성 사이트로 유도하는 공격. `DNSSEC`으로 방어.

📌 **DNS over HTTPS(DoH)**: DNS 질의를 HTTPS로 암호화해 ISP가 어떤 사이트에 접속하는지 못 보게 함. 현대 브라우저가 지원하기 시작.

---

## 관련 개념

- [[IP]] — DNS가 변환해주는 대상
- [[HTTP]] — DNS 조회 후 HTTP로 실제 통신
- [[네트워크]] — DNS는 응용 계층
- [[UDP]] — DNS가 기본으로 사용하는 전송 프로토콜

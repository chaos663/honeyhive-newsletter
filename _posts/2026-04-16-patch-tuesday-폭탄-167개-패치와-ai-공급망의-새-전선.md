---
layout: post
title: "Patch Tuesday 폭탄 — 167개 패치와 AI 공급망의 새 전선"
date: 2026-04-16 08:00:00 +0900
excerpt: "4월 15일, Microsoft가 167개 취약점 패치를 쏟아냈다. 그 중 하나는 이미 실제 공격에 쓰이고 있었다. 같은 주, Adobe는 2025년 11월부터 야생에서 악용되어 온 Acrobat Reader 제로데이를 긴급 패치했다. CISA는 7개 취약점을 KEV(Known Exploited Vulnerabilities) 카탈로그에 추가했고, 패치 기한은 5월 4일이다. 그리고 조용히 일어난 더 큰 사건: **LiteLLM 오픈소스 공급망 "
tags: [보안, deep-dive]
image: "/assets/images/thumbnails/2026-04-16-patch-tuesday-폭탄-167개-패치와-ai-공급망의-새-전선.svg"
series: "꿀벌 뉴스레터 [보안]"
news_count: 7
---
4월 15일, Microsoft가 167개 취약점 패치를 쏟아냈다. 그 중 하나는 이미 실제 공격에 쓰이고 있었다. 같은 주, Adobe는 2025년 11월부터 야생에서 악용되어 온 Acrobat Reader 제로데이를 긴급 패치했다. CISA는 7개 취약점을 KEV(Known Exploited Vulnerabilities) 카탈로그에 추가했고, 패치 기한은 5월 4일이다. 그리고 조용히 일어난 더 큰 사건: **LiteLLM 오픈소스 공급망 공격**이 AI 학습 데이터 스타트업 Mercor를 통해 OpenAI·Anthropic·Meta의 학습 데이터에 닿았다. AI 인프라가 사이버 공격의 새 표적이 되고 있다는 경고등이 켜졌다.

---

## 🚨 위협 인텔리전스

### 🔴 CVE-2026-32201 | Microsoft SharePoint 스푸핑 (실제 악용 중)

| 항목 | 내용 |
|------|------|
| **CVE** | CVE-2026-32201 |
| **CVSS** | 6.5 (Medium — 실제 위험도는 더 높음) |
| **제품** | Microsoft SharePoint Server (전 버전) |
| **유형** | 입력 검증 미흡 → 스푸핑 |
| **공격 벡터** | 네트워크, 인증 불필요 |
| **상태** | ⚠️ 실제 공격에서 악용 확인 |
| **패치** | 2026년 4월 Patch Tuesday 포함 |

**공격 내용**: 네트워크 접근 가능한 비인증 공격자가 SharePoint 서버에서 스푸핑을 수행해 민감 정보를 열람하거나 공개된 데이터를 변조할 수 있다. 가용성에는 직접 영향 없음. 기업의 문서 관리·협업 허브인 SharePoint 특성상 정보 탈취 후 횡이동(lateral movement) 거점으로 악용 가능성이 높다.

**대응**:
- Microsoft 4월 Patch Tuesday 즉시 적용
- SharePoint 외부 접근 네트워크 세분화(segmentation) 점검
- 비정상 SharePoint API 호출 로그 모니터링 강화

---

### 🔴 CVE-2026-XXXX | Microsoft Defender 권한 상승 (제로데이 공개)

| 항목 | 내용 |
|------|------|
| **CVE** | 4월 Patch Tuesday 공개 (CVE 번호 배정 중) |
| **CVSS** | Critical |
| **제품** | Microsoft Defender Antimalware Platform |
| **유형** | 권한 상승 → SYSTEM 권한 획득 |
| **공격 벡터** | 로컬 |
| **상태** | 공개 취약점(publicly disclosed), 악용 미확인 |
| **패치** | Antimalware Platform 업데이트 v4.18.26030.3011 자동 배포 |

**대응**:
```
Windows Security > 바이러스 및 위협 방지 > 보호 업데이트 > 업데이트 확인
```
자동 업데이트가 활성화된 환경은 이미 적용됐을 가능성이 높으나, 에어갭(air-gap) 또는 업데이트 지연 정책 환경은 수동 적용 필수.

---

### 🔴 CVE-2026-34621 | Adobe Acrobat Reader 제로데이 (2025.11부터 야생 악용)

| 항목 | 내용 |
|------|------|
| **CVE** | CVE-2026-34621 |
| **CVSS** | Critical |
| **제품** | Adobe Acrobat DC, Acrobat Reader DC, Acrobat 2024 |
| **유형** | JavaScript Prototype Pollution → 임의 코드 실행 |
| **공격 벡터** | 악성 PDF 파일 열기 (사용자 인터랙션 필요) |
| **첫 악용 확인** | 2025년 11월 (5개월 이상 패치 없이 악용) |
| **CISA KEV** | 2026-04-13 등록, 패치 기한: 2026-04-27 |

**공격 시나리오**: 공격자는 악성 PDF를 이메일로 전송. 피해자가 열면 ① 시스템 핑거프린팅(OS, 소프트웨어 버전, 네트워크 정보 수집) → ② C2 서버로 전송 → ③ C2에서 추가 페이로드 수신 및 실행. 발견된 악성 PDF에는 **러시아어로 된 가스 공급 차질 및 비상 대응 관련 문서**가 위장 미끼로 사용됐다. 에너지 인프라 관련 기업이나 기관을 겨냥한 타깃형 공격(APT) 가능성이 제기된다.

**패치 버전**:
- Acrobat DC / Reader DC: **v26.001.21411** (Windows/macOS)
- Acrobat 2024: **v24.001.30362** (Windows), **v24.001.30360** (macOS)

**즉시 패치 불가 시 임시 조치**:
1. 신뢰할 수 없는 출처의 PDF 파일 열기 금지 정책 수립
2. 엔드포인트 모니터링: "Adobe Synchronizer" User-Agent 포함 HTTP/HTTPS 트래픽 블록
3. 이메일 게이트웨이에서 외부 발신 PDF 샌드박스 검사 강화

---

### 🟠 CISA KEV 7개 추가 (2026-04-13) — 패치 기한 4/27 & 5/4

| CVE | 제품 | CVSS | 유형 | 패치 기한 |
|-----|------|------|------|---------|
| **CVE-2024-21410** | Microsoft Exchange Server | **9.8** | 권한 상승 | **4/27** |
| **CVE-2023-26360** | Adobe ColdFusion | **9.8** | 역직렬화 → RCE | **4/27** |
| **CVE-2023-27997** | Fortinet FortiOS | **9.8** | 힙 버퍼 오버플로우 → RCE | 5/4 |
| **CVE-2023-21709** | Microsoft Exchange Server | High | 원격 코드 실행 | 5/4 |
| **CVE-2023-23397** | Microsoft Outlook | High | 권한 상승 | 5/4 |
| **CVE-2023-21554** | Microsoft Exchange Server | High | 정보 노출 | 5/4 |
| **CVE-2023-20887** | VMware Aria Operations | High | 명령 인젝션 | 5/4 |

**주목 포인트**: Exchange Server 관련 CVE가 3개 포함됐다. CISA는 "공격자들이 최신 제로데이가 아닌 2~3년 된 구버전 취약점을 여전히 적극 악용하고 있다"고 경고했다. 패치 관리 프로그램이 취약한 조직이 우선 타깃이 되고 있다.

---

## 🕵️ 침해 사고

### 사건 1: LiteLLM 공급망 공격 → Mercor AI 데이터 유출

**사건 개요**: AI 인프라 오픈소스 프레임워크 **LiteLLM**의 공급망 공격으로, AI 학습 데이터 스타트업 **Mercor**(기업가치 $100억)가 데이터 침해를 확인했다. Mercor는 OpenAI, Anthropic, Meta에 AI 학습 데이터를 제공하는 기업이다.

**공격 벡터**: LiteLLM은 다양한 LLM API를 통합하는 오픈소스 라이브러리로, Mercor를 포함한 다수의 AI 스타트업이 사용하고 있었다. 공급망 공격(supply chain attack)을 통해 LiteLLM 의존 환경에 침투한 공격자들은 Mercor의 시스템에 접근해 Slack 대화 데이터, 내부 티켓, AI-계약자 간 인터랙션 영상 등을 탈취했다. 공격자는 추가로 소스코드와 데이터베이스 레코드를 보유하고 있다고 주장했다.

**영향**: AI 학습 데이터 생태계 전반에 신뢰 위기. 탈취된 데이터가 AI 학습에 사용된 실제 사용자 상호작용 내용을 포함할 경우, 개인정보 침해 범위가 불특정 다수로 확산될 수 있다.

**교훈**:
1. **오픈소스 의존성 리스크**: 외부 오픈소스 패키지의 공급망 무결성 검증(SBOM, Software Bill of Materials) 없이는 내부 보안이 아무리 강해도 소용없다.
2. **AI 인프라는 이제 고가치 표적**: LLM 학습 데이터, AI 모델 가중치, API 키는 전통적 데이터베이스만큼 중요한 공격 대상이 됐다.
3. **최소 권한 원칙**: LiteLLM 인스턴스에 Slack 및 내부 시스템 접근 권한이 불필요하게 부여된 설계가 피해를 키웠다.

---

### 사건 2: LAPD 경찰 문서 7.7TB 유출 — World Leaks 갱단

**사건 개요**: 해커 그룹 **World Leaks**가 미국 로스앤젤레스 경찰청(LAPD)의 디지털 스토리지 시스템(시 검사처 소속)을 침해, **337,000개 파일 7.7TB** 분량의 데이터를 탈취·공개했다. 경찰관 인사 파일, 내부 조사 문서, 미편집 범죄 고발장, 증인 신원, 의료 데이터가 포함됐다.

**교훈**: 법집행 기관의 민감 데이터가 제3자 디지털 스토리지 시스템에 보관되는 구조 자체가 공격 표면을 확대했다. **데이터 분류·최소화·암호화**와 함께 제3자 벤더 보안 감사가 필수다.

---

### 사건 3: Qilin 랜섬웨어 — 독일 Die Linke 정당 공격

**사건 개요**: **Qilin** 랜섬웨어 그룹이 독일 민주사회당(Die Linke)을 공격했다. 당원 12만 3,000명, 독일 의회 64명 의원을 가진 정당이다. 당 측은 "당원 데이터베이스는 접근되지 않았다"고 주장했으나, Qilin은 당 본부 직원 관련 정보를 탈취했다고 주장했다.

**교훈**: 정치 조직은 선거 주기와 맞물려 APT 그룹과 범죄 조직 모두의 표적이 된다. 오프라인 백업 + 네트워크 세분화 + 특권 계정 관리(PAM)가 핵심 방어선이다.

---

## 🛠️ 보안 도구 & 패치 요약

| 우선순위 | 제품 | 조치 | 기한 |
|---------|------|------|------|
| 🔴 긴급 | Adobe Acrobat DC/Reader DC | v26.001.21411 업데이트 | **즉시** |
| 🔴 긴급 | Microsoft SharePoint Server | 4월 Patch Tuesday 적용 | **즉시** |
| 🔴 긴급 | Microsoft Exchange Server (CVE-2024-21410) | 패치 적용 | **4/27** |
| 🔴 긴급 | Adobe ColdFusion (CVE-2023-26360) | 패치 적용 | **4/27** |
| 🟠 높음 | Fortinet FortiOS (CVE-2023-27997) | 패치 적용 | 5/4 |
| 🟠 높음 | Microsoft Outlook (CVE-2023-23397) | 패치 적용 | 5/4 |
| 🟠 높음 | VMware Aria Operations (CVE-2023-20887) | 패치 적용 | 5/4 |
| 🟡 중요 | Microsoft Defender | Antimalware v4.18.26030.3011 자동 적용 확인 | 이번 주 |
| 🟡 중요 | Microsoft Office (Word/Excel) | 4월 Patch Tuesday 적용 | 이번 주 |

---

## 🗞️ 한 줄 보안 뉴스

- **Amazon, AI 코드 장애 후 90일 코드 보안 감사 진행**: 3월 두 차례 장애(AI 생성 코드 원인 추정)로 전사 코드 보안 감사. AI 코딩 도구 배포 정책 재검토 중
- **GitHub Copilot 취약점 있는 코드 자동 완성 문제 지속**: 최신 Lightrun 조사에서 AI 생성 코드 43%가 프로덕션에서 수동 디버깅 필요 확인
- **미·이란 갈등 속 친이란 해커 그룹 활동 급증**: CrowdStrike·Mandiant 모두 에너지·금융 섹터 대상 사이버 공격 모니터링 강화 권고
- **Diffusion LLM 안전 필터 우회 기법 공개 (arXiv 2604.08557)**: 마스크 토큰 역방향 엔지니어링으로 dLLM 계열 모델 안전 정렬 무력화 가능 — AI 모델 보안 새 연구 영역
- **AI 위조 문서 탐지 도구 시장 급성장**: AI 생성 PDF·이메일·신분증 위조가 급증하며 탐지 전문 보안 솔루션 스타트업에 VC 자금 집중

---

## ✅ CISO 체크리스트 (이번 주 즉시 실행)

**긴급 (24~48시간)**
- [ ] Adobe Acrobat Reader **v26.001.21411** 전사 업데이트 배포
- [ ] Microsoft 4월 Patch Tuesday 패치 테스트·배포 일정 수립
- [ ] Exchange Server CVE-2024-21410 패치 적용 현황 확인

**이번 주 내**
- [ ] Fortinet FortiOS CVE-2023-27997 패치 현황 확인 (KEV 기한: 5/4)
- [ ] VMware Aria Operations CVE-2023-20887 패치 상태 점검
- [ ] SharePoint Server 비정상 API 호출 로그 감사 시작
- [ ] LiteLLM 또는 AI 관련 오픈소스 라이브러리 의존성 목록(SBOM) 검토

**이번 달**
- [ ] AI 인프라(LLM API, 학습 데이터, 모델 가중치) 접근 권한 최소화 점검
- [ ] 제3자 오픈소스 패키지 공급망 무결성 검증 프로세스 수립
- [ ] 외부 수신 PDF 파일 샌드박스 분석 파이프라인 강화
- [ ] 정치·선거 관련 조직 대상 사이버보안 교육 실시

---

## 📋 컴플라이언스 업데이트

- **CISA 바인딩 운영 지시**: FCEB 기관은 KEV 카탈로그 취약점을 4/27(CVE-2024-21410, CVE-2023-26360) 및 5/4(나머지 5개)까지 필수 패치. 민간 기업도 동등한 우선순위 권고.
- **GDPR/개인정보 관련**: LiteLLM 공급망 사고로 AI 학습 데이터의 개인정보 처리 위탁 계약 및 보안 감사 의무 재점검 필요. EU GDPR Art. 28(처리자) 준수 여부 점검.
- **AI 시스템 보안 거버넌스**: EU AI Act 시행이 임박한 가운데, AI 시스템에 사용되는 학습 데이터와 오픈소스 라이브러리의 보안 감사가 컴플라이언스 요구사항으로 부상.

---

## 📎 출처

- [Microsoft April 2026 Patch Tuesday fixes 167 flaws, 2 zero-days](https://www.bleepingcomputer.com/news/microsoft/microsoft-april-2026-patch-tuesday-fixes-167-flaws-2-zero-days/) — BleepingComputer
- [Adobe issues emergency fix for Acrobat Reader flaw exploited in the wild (CVE-2026-34621)](https://www.helpnetsecurity.com/2026/04/13/adobe-acrobat-reader-cve-2026-34621-emergency-fix/) — Help Net Security
- [CISA Adds 7 Actively Exploited Vulnerabilities to KEV Catalog](https://windowsnews.ai/article/cisa-adds-7-actively-exploited-vulnerabilities-to-kev-catalog-microsoft-exchange-adobe-fortinet-flaw.412350) — Windows News AI
- [Data Breach Roundup (Apr 3–9, 2026)](https://www.privacyguides.org/news/2026/04/12/data-breach-roundup-apr-3-9-2026/) — Privacy Guides
- [Adobe Patches Actively Exploited Acrobat Reader Flaw CVE-2026-34621](https://thehackernews.com/2026/04/adobe-patches-actively-exploited.html) — The Hacker News
- [CISA Warns of Fortinet SQL Injection Vulnerability Actively Exploited](https://cybersecuritynews.com/fortinet-sql-injection-vulnerability-exploited/) — CybersecurityNews

---

*본 뉴스레터는 HoneyHive 뉴스레터벌이 자동 수집·요약했습니다. 패치 및 보안 조치 전 공식 벤더 권고를 반드시 확인하세요.*

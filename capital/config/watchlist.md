# AI 자본 흐름 감시 대상 정의

> `ai-capital-monitor` 에이전트가 매 실행 시 읽는 설정 파일. 임원이 직접 손으로 고쳐도 된다.
> 에이전트 정의를 수정하지 말고 이 파일만 고칠 것.

## §0 게재 기준선

| 항목 | 값 |
|---|---|
| 기본 금액 기준선 | **$50M 이상** |
| 수집 창 | 최근 8일 (주간 실행 + 1일 겹침) |
| 확정 딜 최대 게재 수 | 12건 |
| 통화 환산 | USD 기준. 원/유로/위안 딜은 발표일 환율로 환산하고 원 통화 금액을 괄호에 병기 |

**$50M 미만이어도 게재하는 예외** (전략적 중요성 우선):

1. §3 전략적 투자자가 리드 또는 주요 참여
2. 분야가 §2의 **System Software for NPU/LLM** - 이 분야는 라운드 자체가 작다
3. Exynos / ENN / Galaxy 온디바이스 경로와 직접 경합·보완
4. 삼성 계열의 투자·제휴·인수 대상
5. 시드·시리즈A라도 창업자가 §4 주목 인물

---

## §1 기술 분야 분류

| 코드 | 분야 | 범위 |
|---|---|---|
| `llm-model` | LLM 모델 / 파운데이션 | 특히 **소형·온디바이스 지향** 모델 기업. 초거대 모델 기업은 자본 규모 신호로만 |
| `npu-silicon` | NPU / AI 반도체 | 엣지 NPU, 추론 ASIC, 데이터센터 가속기, 칩렛, 아날로그·인메모리 컴퓨팅 |
| `sys-software` | **System Software for NPU/LLM** ★최우선★ | 컴파일러(MLIR/TVM/XLA 계열), 런타임, 커널 라이브러리, 추론 서버·서빙, 양자화·경량화 툴체인, 엣지 배포 플랫폼, NPU SDK |
| `edge-device` | 엣지 디바이스 / 모듈 | 온디바이스 AI 기기, 로보틱스 두뇌, 차량 SoC 모듈, AI PC |
| `infra-memory` | 인프라 · 메모리 | HBM, CXL, 인터커넥트, 광I/O, 냉각, AI 데이터센터 |
| `data-tooling` | 데이터 · 평가 · 보안 툴링 | 주변 영역. 대형 라운드일 때만 |

`sys-software`는 언론 커버리지가 가장 얇으므로 **매 회차 별도 검색축을 반드시 돌린다.**

---

## §2 감시 스타트업 명단

> 명단에 없어도 §0 기준을 통과하면 게재한다. 이 명단은 **직접 조회할 대상**이다.

### NPU / AI 반도체
- **한국**: FuriosaAI(퓨리오사), Rebellions(리벨리온), DeepX(딥엑스), Mobilint(모빌린트), HyperAccel(하이퍼엑셀), OPENEDGES
- **미국**: Groq, Cerebras, SambaNova, Etched, d-Matrix, Tenstorrent, Rain AI, EnCharge AI, Axelera(EU), Hailo(IL), Untether(CA), Lightmatter, Celestial AI
- **중국**: Cambricon(寒武纪), Biren(壁仞), Moore Threads(摩尔线程), Enflame(燧原), Black Sesame, Horizon Robotics(地平线)
- **일본/EU**: PFN(Preferred Networks), Graphcore(SoftBank), SiPearl

### System Software for NPU/LLM ★최우선 감시★
- Modular(Mojo/MAX), OctoAI 계열 후속, Fireworks AI, Together AI, Baseten, Fal.ai,
  Neural Magic 계열 후속, Deci 계열 후속, Nod.ai 계열, Lemurian Labs, MLCommons 상용화 스핀오프,
  Chipflow / 컴파일러 툴체인 스타트업, Edge Impulse, Latent AI, Nota AI(노타), SqueezeBits(스퀴즈비츠),
  Ollama / LM Studio 상용화, vLLM 상용 스핀오프, SGLang 상용화, TensorWave

### 온디바이스 지향 LLM 모델
- Mistral AI, Liquid AI, Arcee AI, Nexa AI, Kyutai, Sakana AI, Upstage(업스테이지), 41Q / Trillion Labs

### 엣지 디바이스 · 로보틱스 두뇌
- Physical Intelligence, Skild AI, Figure, Rabbit 계열, Humane 계열 잔여 자산

**신규 등재 규칙**: 브리핑에 2회 이상 등장한 기업은 이 명단에 추가한다(에이전트가 직접 추가해도 된다).

---

## §3 전략적 투자자 명단

> 이 명단의 기업이 참여한 딜은 **금액과 무관하게 게재 검토 대상**이며, 리포트에 `[전략적]` 표시.

| 구분 | 투자 주체 |
|---|---|
| **삼성 계열** ★최우선★ | Samsung Ventures, Samsung Catalyst Fund, Samsung NEXT, Samsung Strategy & Innovation Center, 삼성전자 직접 투자·제휴 |
| 반도체·IP | Qualcomm Ventures, Intel Capital, AMD Ventures, ARM, MediaTek, TSMC, SK hynix / SK Telecom, Micron Ventures, Synopsys, Cadence |
| 플랫폼·빅테크 | NVIDIA / NVentures, Google / GV / CapitalG, Microsoft / M12, Amazon / AWS, Meta, Apple(인수 위주) |
| 디바이스·완제품 | Sony, LG Technology Ventures, Bosch Ventures, Continental, Hyundai / 현대차그룹, Xiaomi, Lenovo Capital |
| 국부·정책 자본 | MGX(UAE), PIF / Alat(사우디), Temasek·EDBI(싱가포르), 한국 성장금융·모펀드, EU Chips Act 연계 펀드, 일본 JIC |
| 전문 딥테크 VC | a16z American Dynamism, Lux Capital, Playground Global, Eclipse Ventures, Celesta, Walden Catalyst |

---

## §4 주목 인물

창업자·CTO 이동은 자본 이동의 선행 지표다. 아래 인물의 신규 창업·합류를 감시한다.

- 대형 NPU 기업 아키텍트 출신의 신규 창업 (Google TPU · Apple Silicon · Qualcomm Hexagon · NVIDIA 출신)
- 주요 컴파일러 프로젝트 리드 (MLIR / TVM / XLA / Triton / IREE 코어 커미터)의 상용화 창업
- 한국: 국내 NPU·경량화 연구실 출신 창업 (KAIST·서울대·POSTECH 계열)

구체 인물명은 관측되는 대로 에이전트가 이 절에 추가한다.

---

## §5 impact 배점표 (총 100점)

| 축 | 배점 | 채점 기준 |
|---|---|---|
| **라운드 규모** | 25 | $500M+ = 25 · $200-500M = 20 · $100-200M = 16 · $50-100M = 12 · $20-50M = 7 · $20M 미만 = 3 |
| **밸류에이션 점프** | 15 | 직전 대비 3배+ 및 18개월 내 = 15 · 2-3배 = 11 · 1.5-2배 = 7 · 플랫/다운 = 2(단 다운라운드는 별도 신호로 서술) · 최초 공개 = 5 |
| **전략적 투자자 구성** | 20 | 삼성 계열 참여 = 20 · 반도체·IP 대기업 리드 = 17 · 빅테크 참여 = 14 · 디바이스 기업 참여 = 11 · 국부펀드만 = 8 · 재무적 투자자만 = 3 |
| **DX 인접성** | 25 | 온디바이스 NPU·컴파일러·런타임 직결 = 25 · 엣지 추론 일반 = 19 · 소형 모델 = 15 · 데이터센터 추론 = 10 · 학습 인프라 = 5 · 무관 = 0 |
| **시장 신호성** | 15 | 새 카테고리 개창 = 15 · 섹터 전환 신호 = 12 · 경쟁 구도 변화 = 9 · 기존 흐름 강화 = 5 · 단순 후속 = 2 |

**게재 임계**: `impact 60 이상` = 핫 딜(카드 상세) · `40-59` = 확정 딜 표 · `40 미만` = 게재하지 않음
(단 `sys-software` 분야와 삼성 계열 참여 딜은 **35점까지 표에 게재**한다)

---

## §6 소스 목록

### 1차 출처 (신뢰도 최상)
- 회사 공식 블로그·보도자료, 투자사 공식 발표
- **SEC EDGAR Form D** (미국 사모 발행 공시) - 언론보다 먼저 나온다
- **DART 전자공시** (한국), 일본 EDINET, EU 법인 등기
- Crunchbase / PitchBook 딜 등재

### 언론
- TechCrunch, Reuters, Bloomberg, The Information, Financial Times, CNBC
- 반도체 전문: SemiAnalysis, EE Times, Tom's Hardware, AnandTech 후속 매체, DIGITIMES
- **한국**: the bell, 전자신문, ZDNet Korea, 머니투데이 벤처
- **중국**: 36Kr(36氪), 量子位, 芯东西, TechNode

### 커뮤니티·집계
- Hacker News (펀딩 스레드), r/LocalLLaMA (엣지 추론 스타트업 반응)
- a16z / Sequoia / Lux 포트폴리오 페이지 변경, YC 배치 발표
- LinkedIn 채용 급증 (자금 조달의 후행 신호이나 루머 검증에 유용)

### 검색 표현 (sys-software 축은 여러 표현을 시도할 것)
```
"AI compiler startup raises"        "inference runtime funding"
"MLIR startup funding"             "model optimization startup Series"
"edge AI deployment platform raises"  "quantization startup funding"
"NPU SDK startup"                  "LLM serving infrastructure raises"
"AI chip startup Series B"         "on-device AI startup funding"
```

---

## §7 제외 대상

아래는 게재하지 않는다(노이즈).

- 순수 애플리케이션·SaaS 레이어 AI 스타트업 (챗봇, 문서요약, 마케팅 AI)
- 암호화폐·GPU 채굴 전환 기업
- 자율주행 완성 스택 기업 (차량용 **SoC·NPU** 부분만 예외적으로 포함)
- 생성형 미디어(이미지·영상·음악) 기업 - 단 **추론 최적화 기술**이 본질인 경우는 포함
- ETF·펀드 결성 자체 (§1 분야 전용 펀드는 macro 섹션에 한 줄)

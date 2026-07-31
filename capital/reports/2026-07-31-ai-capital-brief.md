# AI 자본 흐름 브리핑 - 2026-07-31 (주간 · 첫 회차 기준선)

**한 줄 요약**: 이번 회차 신규 확정 자본의 68%가 NPU 실리콘이 아니라 **NPU/LLM용 시스템 소프트웨어**로 들어갔고, 그 정점에서 Qualcomm이 Chris Lattner의 컴파일러·런타임 스택(Modular)을 $3.9B에 사들여 7/29 클로징했다.

> **수집 창 표기**: `[W8]` = 2026-07-23~07-31 (정규 8일 창) · `[W4]` = 2026-07-01~07-22 (첫 회차 기준선 확보용 4주 확장 창).
> 7월 이전 항목은 확정 딜로 게재하지 않고 맥락 인용에만 사용했다.

## 임원 요약 (Executive Summary)

- **[W8] Qualcomm이 Modular 인수를 7/29 완료($3.9B 전액 주식)** - Mojo 언어와 MAX 런타임, 그리고 LLVM·Swift·MLIR의 Chris Lattner를 통째로 확보했다. Lattner는 Qualcomm EVP(Advanced AI Software and Platforms)로 부임. 인수된 스택은 CPU·GPU·**NPU**·커스텀 실리콘을 모두 타깃한다. 즉 경쟁사가 우리 ENN 툴체인이 하는 일을 **하드웨어 중립 상용 제품**으로 보유하게 됐다. → **행동 함의: 최우선 경합. Mojo/MAX의 NPU 백엔드 개방 범위를 90일 내 기술 실사할 것.**
- **[W8] Multiverse Computing이 Series C $570M을 밸류 $1.7B(pre-money)에 조달(7/28)** - 텐서 네트워크 기반 모델 압축(CompactifAI, 80-95% 축소 주장)으로 스마트폰·PC 엣지 실행을 겨냥한다. 경량화 툴체인 단독 기업이 5억 달러대를 받은 첫 사례이며 **HP Inc.가 전략적 투자자로 참여**했다. 우리 경량화 조직과 정면으로 겹치는 영역이다.
- **[W4] 자본은 칩이 아니라 추론 실행 계층에 쏠렸다** - Fireworks AI $1.5B(밸류 $17.5B, NVIDIA 참여, 7/16), Together AI $800M(밸류 $8.3B post, Aramco Ventures 리드 + NVIDIA·Pegatron, 7/1), Ollama $65M(로컬 실행 런타임, 7/9). 시스템 SW 분야 이번 회차 지분 조달 합계 $2,950M 대 NPU 실리콘 $1,300M.
- **[W8] 삼성 계열의 확정 신규 투자는 이번 회차 0건이다** - 감시 명단 6개 삼성 주체 모두 신규 발표 없음. 다만 Mistral AI에 EUR 10억 규모 투자 협상 보도가 있으며 **이는 미확정 루머**로 분류했다. SK hynix는 Etched Series C에 참여해 추론 ASIC에 직접 발을 담갔다.
- **[W4] 온디바이스 실행권을 사려는 인수 시장이 열렸다** - Apple이 AI 칩 스타트업 인수를 물색 중이라는 보도(7/15)와 PrismML 접촉 보도(7/10)가 같은 주에 나왔다. 둘 다 미확정이나, Qualcomm-Modular 클로징과 겹쳐 읽으면 **온디바이스 실행 스택은 이제 자체 개발이 아니라 매입 대상**이다. → **행동 함의: 국내 경량화·런타임 자산(노타·스퀴즈비츠 등)에 대한 선제 접촉 가치가 상승.**

---

## 핫 딜 (impact 60 이상)

### [1] Multiverse Computing - Series C · $570M  `[W8]`

- **분야**: System Software (양자화·압축 툴체인)
- **본사·설립**: 스페인 산세바스티안 / 2019년
- **라운드**: Series C, $570M, 밸류 **$1.7B (pre-money)** | 직전 Series B($215M, 2025-06) 대비 보도상 약 5배 step-up / 약 13개월 (※직전 라운드 밸류 절대값은 1차 출처 미확인 - 배수는 보도 기준)
- **투자자**: 리드 = Forgepoint Capital International, BNPP SIVF, Bullhound Capital(공동). 참여 = Santander Alternative Investments, Tikehau Capital, **[전략적] HP Inc.**, Orange Ventures, Scania Invest, NAventures 외 6개사
- **무엇을 만드는가**: CompactifAI - 양자물리에서 온 텐서 네트워크 분해로 LLM 가중치를 압축한다. 정확도 손실을 최소화하며 모델 크기 80-95% 축소를 주장하고, 클라우드 연결 없이 **스마트폰·PC·공장 설비에서 직접 실행**하는 것을 목표 배포처로 명시한다. 순수 후처리 양자화(PTQ)가 아닌 구조적 저랭크 분해 계열이므로 기존 INT4/INT8 파이프라인과는 다른 축이다.
- **검증된 트랙션**: 공개 고객 - Allianz, Bank of Canada, **Bosch**, Iberdrola, Indra, PwC, Telefonica. 제조·금융·에너지·항공우주·국방 적용. 공개 벤치마크 수치는 회사 주장 외 독립 검증분 미확인
- **누적 조달**: $785M (원장 기준: Series B $215M + Series C $570M)
- **DX 시사점**: **경합**. 우리 경량화 조직의 핵심 산출물(온디바이스 모델 압축)을 외부 상용 툴로 판매하는 회사가 5억 달러대 실탄을 확보했다. Bosch가 고객, HP가 투자자라는 구성은 이미 **디바이스 제조사 채널로 진입 중**임을 뜻한다. Galaxy·AI PC 경로에서 우리가 사내 툴체인으로 대응할 것인지, 라이선스 평가를 할 것인지 결정이 필요하다.
- **impact**: **88/100** - 규모 25(+$500M) · 밸류점프 15(약 5배/13개월, 보도 기준) · 전략적 11(HP 참여) · DX 인접성 25(온디바이스 경량화 직결) · 시장 신호성 12(경량화가 단독 대형 카테고리로 성립)
- **상태**: NEW
- **출처**: https://siliconangle.com/2026/07/28/ai-model-compression-startup-multiverse-raises-570m-1-7b-valuation/ · https://techcrunch.com/2026/03/19/multiverse-computing-pushes-its-compressed-ai-models-into-the-mainstream/

### [2] Qualcomm ← Modular - 인수 완료 · $3.9B (전액 주식)  `[W8]`

- **분야**: System Software (컴파일러 + 런타임)
- **본사·설립**: 미국 캘리포니아 / 2022년 (인수 완료로 Qualcomm Technologies 산하)
- **라운드**: 인수. 발표 2026-06-24, **클로징 2026-07-29**. 대가 약 $3.9B 전액 주식. 인력 약 150명
- **투자자**: 인수 주체 = **[전략적] Qualcomm**
- **무엇을 만드는가**: Mojo - 추론 코드를 한 번 쓰면 NVIDIA·AMD·Intel·Qualcomm·Apple Silicon에서 하드웨어별 재작성 없이 최적 실행되게 하는 언어. MAX - 모델 실행을 하드웨어에 매핑하는 런타임 프레임워크. Modular Cloud까지 3개 제품 브랜드가 존속한다. 공식 문구상 지원 대상은 **CPU·GPU·NPU·커스텀 실리콘**이며 "개방형 이종(heterogeneous) 생태계" 유지를 선언했다. 창업자 Chris Lattner는 LLVM 설계자이자 Swift 창시자이며 MLIR의 원저자다
- **검증된 트랙션**: Qualcomm CEO 발언 - "엣지에서 클라우드까지" 대상. 데이터센터·엣지·개인/산업 AI로 확장 명시. Snapdragon 특정 로드맵은 공식 발표에 미언급
- **누적 조달**: 해당 없음(인수 종결). 인수 전 Modular 누적 조달은 원장 미기록
- **DX 시사점**: **최우선 경합**. 이 거래의 본질은 칩이 아니라 **컴파일러 IR 계층의 소유권**이다. Qualcomm은 이제 (a) Hexagon NPU, (b) MLIR 원저자, (c) 하드웨어 중립을 표방하는 상용 런타임을 동시에 보유한다. Exynos/ENN이 자체 SDK로 개발자를 붙잡는 구도에서, 경쟁사는 "우리 칩에도 남의 칩에도 돌아간다"는 상위 계층을 무기로 쓴다. 참고로 Qualcomm은 Tenstorrent 인수($8-10B 규모 보도)는 성사시키지 못했고, 대신 **실리콘이 아닌 소프트웨어를 샀다** - 어느 계층이 병목인지에 대한 그들의 판단이 여기 드러난다
- **impact**: **87/100** - 규모 25($3.9B) · 밸류점프 5(인수, 배수 해당 없음) · 전략적 17(반도체 대기업이 100% 인수) · DX 인접성 25(컴파일러·런타임 직결) · 시장 신호성 15(하드웨어 업체가 컴파일러 계층을 전략 자산으로 매입하는 새 패턴)
- **상태**: NEW (발표는 6/24였으나 **클로징이 7/29로 W8 창 내** - 첫 회차 원장 등재)
- **출처**: https://www.modular.com/blog/qualcomm-completes-acquisition-of-modular · https://www.hpcwire.com/aiwire/2026/07/29/qualcomm-completes-acquisition-of-modular/ · https://www.networkworld.com/article/4189098/qualcomms-3-9-billion-purchase-of-modular-aims-to-change-the-data-center-dynamic.html

### [3] Together AI - Series C · $800M  `[W4]`

- **분야**: System Software (추론 서빙·클라우드)
- **본사·설립**: 미국 샌프란시스코 / 2022년
- **라운드**: Series C, $800M, 밸류 **$8.3B (post-money)** | 직전 Series B($305M, 밸류 $3.3B) 대비 **2.5배 / 약 16개월**
- **투자자**: 리드 = Aramco Ventures(국부·정책 자본). 참여 = Vista Equity Partners, General Catalyst, Emergence Capital, **[전략적] NVIDIA**, March Capital, **[전략적] Pegatron**, S Ventures(SentinelOne)
- **무엇을 만드는가**: 오픈웨이트 모델의 학습·추론을 함께 제공하는 GPU 클라우드. 커널 최적화(FlashAttention 계열 기여)와 추론 엔진을 자체 보유해 단순 GPU 임대가 아닌 **소프트웨어 마진**을 취하는 구조다. 온디바이스가 아닌 서버 측 추론이 주력
- **검증된 트랙션**: 발표문상 오픈소스 추론 수요 3배 증가 언급. 구체 고객명·매출 수치는 1차 출처 미공개
- **누적 조달**: $1,105M (원장 기준: Series B $305M + Series C $800M)
- **DX 시사점**: **간접 보완**. 온디바이스 직접 함의는 낮다. 다만 **Pegatron(디바이스 ODM)과 NVIDIA가 같은 라운드에 들어온 구성**은 서버-엣지 하이브리드 추론 제품화를 준비하는 신호로 읽을 수 있다. 하이브리드 오케스트레이션(온디바이스 + 클라우드 분기)에서 우리와 접점이 생길 계층이다
- **impact**: **69/100** - 규모 25($800M) · 밸류점프 11(2.5배/16개월) · 전략적 14(NVIDIA 참여) · DX 인접성 10(데이터센터 추론) · 시장 신호성 9(오픈웨이트 추론 사업의 경쟁 구도 변화)
- **상태**: NEW
- **출처**: https://www.businesswire.com/news/home/20260701243402/en/Together-AI-Raises-$800-Million-at-$8.3-Billion-Valuation-to-Make-Frontier-AI-Accessible-to-All · https://techcrunch.com/2026/07/01/neocloud-together-ai-raises-800m-leaps-to-8-3b-valuation/

### [4] Fireworks AI - Series D · $1.5B  `[W4]`

- **분야**: System Software (LLM 서빙·추론 최적화)
- **본사·설립**: 미국 레드우드시티 / 2022년 (Meta PyTorch 팀 출신 창업)
- **라운드**: Series D, $1.5B, 밸류 **$17.5B** (pre/post 구분 1차 출처 미명시 - **구분 미확인**) | 직전 라운드 대비 배수 **미확인**
- **투자자**: 리드 = Atreides Management, Index Ventures, TCV(공동). 참여 = Evantic, Lightspeed, **[전략적] NVIDIA**, Bessemer Venture Partners, Menlo Ventures, Insight Partners, Ontario Teachers' Pension Plan, Lone Pine Capital
- **무엇을 만드는가**: 오픈웨이트·파인튜닝 모델을 서빙하는 추론 플랫폼. 자체 커널·배치 스케줄링·스펙큘레이티브 디코딩 등 서빙 계층 최적화가 제품의 본체이며, 모델을 만들지 않고 **실행만 판다**
- **검증된 트랙션**: 연환산 매출 $1B 돌파(회사 발표), 전년 대비 5배. 플랫폼 토큰 처리량 일 40조 토큰 초과(직전 15조에서 증가)
- **누적 조달**: $1,500M (원장 기준. 이전 라운드는 1차 출처 미확인으로 원장 미기록 - 첫 회차)
- **DX 시사점**: **무관에 가까움 - 직접 함의 낮음**. 서버 측 서빙이 본업이므로 Exynos/ENN 경로와 직접 경합하지 않는다. 이 딜의 가치는 **가격표**에 있다: "모델을 만들지 않고 실행만 최적화하는 회사"에 $17.5B가 붙었다. 실행 계층의 시장가치를 사내 툴체인 투자 정당화 근거로 쓸 수 있다
- **impact**: **66/100** - 규모 25($1.5B) · 밸류점프 5(직전 배수 미확인) · 전략적 14(NVIDIA 참여) · DX 인접성 10(데이터센터 추론) · 시장 신호성 12(실행 계층 밸류에이션 재평가)
- **상태**: NEW
- **출처**: https://finance.yahoo.com/technology/ai/articles/fireworks-raises-1-5-billion-130000636.html · https://www.otpp.com/en-ca/about-us/news-and-insights/2026/fireworks-raises-a-1-5-billion-series-d-to-lead-the-specialized-intelligence-revolution/

### [5] Etched - Series C · $300M  `[W8]`

- **분야**: NPU / 실리콘 (추론 전용 ASIC)
- **본사·설립**: 미국 산호세 / 2022년 (하버드 중퇴 3인 창업)
- **라운드**: Series C, $300M, 밸류 **$10.3B (post-money)** | 직전 Series B($500M, 2025-12) 대비 배수 **미확인**(직전 밸류 1차 출처 미공개)
- **투자자**: 리드 = Sequoia Capital. 참여 = Andreessen Horowitz, **[전략적] SK hynix**, Jane Street, Diffusion Capital. 기존 주주 Peter Thiel, Andrej Karpathy, Dylan Field 등
- **무엇을 만드는가**: 프론티어 모델 추론 전용 ASIC. 핵심은 두 갈래다 - ① 경쟁 대비 크게 낮은 전압에서 동작하는 **prefill 전용 칩**, ② 다수 칩이 메모리 풀을 저지연 공유하는 **decode 메모리·인터커넥트 서브시스템**. 즉 prefill/decode를 물리적으로 분리(disaggregation)해 각 단계에 다른 실리콘을 붙이는 구조다. 조 단위 파라미터 sparse MoE를 서멀 스로틀링 없이 피크 FLOPs 80% 이상으로 돌린다고 주장. TSMC 파운드리
- **검증된 트랙션**: 수주 잔고 $1B(booked orders, 회사 주장). 주요 AI 기업과 시스템 테스트 중. 2MW 자체 데이터센터 운영 + 밀피타스 80,000 sqft·10MW 시설 신규 개소. 독립 공개 벤치마크는 미확인
- **누적 조달**: $800M (원장 기준: Series B $500M + Series C $300M)
- **DX 시사점**: **직접 경합 없음 - 데이터센터 전용**. 온디바이스 함의는 낮다. 그러나 두 가지가 우리에게 유효하다: ① **prefill/decode 분리 아키텍처**가 자본 시장의 승인을 받았다 - 이 구조는 온디바이스 하이브리드(prefill 서버, decode 단말)에도 그대로 대응된다. ② **SK hynix가 GPU 대안 ASIC에 지분으로 참여**했다 - HBM 공급사가 특정 ASIC 진영에 정렬되기 시작했다는 뜻이고, 메모리 조달 관점의 함의가 있다
- **impact**: **60/100** - 규모 20($200-500M) · 밸류점프 5(직전 밸류 미확인) · 전략적 13(SK hynix 참여, 리드 아님) · DX 인접성 10(데이터센터 추론) · 시장 신호성 12(HBM 공급사의 ASIC 진영 정렬 = 섹터 전환 신호)
- **상태**: NEW
- **출처**: https://techcrunch.com/2026/07/23/ai-chip-startup-etched-defies-skeptics-hits-10-3b-valuation-from-big-name-investors/ · https://semiengineering.com/startup-funding-q2-2026/

---

## 그 외 확정 딜 (impact 40-59)

| 회사 | 분야 | 라운드 | 금액 | 밸류 | 리드 | 전략적 투자자 | 창 | impact | 출처 |
|---|---|---|---|---|---|---|---|---|---|
| Ollama | sys-software (로컬 런타임) | Series B | $65M | 미공개 | Theory Ventures | 없음 | W4 (7/9) | 57 | [TechCrunch](https://techcrunch.com/2026/07/09/popular-open-source-ai-developer-tool-ollama-raises-65m-grows-to-nearly-9m-users/) |
| SambaNova Systems | npu-silicon | Series F (1차 클로징) | $1,000M | $11B (pre/post 미확인) | General Atlantic | 없음 | W4 (7/8) | 48 | [TechCrunch](https://techcrunch.com/2026/07/08/sambanova-draws-1b-at-11b-valuation-in-series-f-first-close/) |
| Infinity | sys-software (커널 생성·컴파일) | Seed | $15M | $100M (post) | Touring Capital | 없음(칩 기업 임원 엔젤) | W4 (7/20) | 48 | [TechCrunch](https://techcrunch.com/2026/07/20/inference-startup-infinity-raises-15m-from-touring-capital-openai-and-athropic-researchers/) |
| Dnotitia (디노티시아) | infra-memory / npu-silicon | Series A | $61.2M (약 900억원) | 미공개 | Elohim Partners | 없음 | W4 (7/13) | 42 | [StartupRise](https://startuprise.org/dnotitia-raises-61-2-million-in-series-a-round-led-by-elohim-partners/) |

보충 메모:
- **Ollama**: 총 조달 $88M. 개발자 월간 활성 890만명, 팀 14명, Fortune 500의 85%가 사용(회사 발표). Benchmark·8VC·Y Combinator 참여. 창업자 Jeff Morgan·Michael Chiang은 Docker Desktop 출신. **로컬 실행 런타임이 상용 자본을 받은 사실상 첫 대형 사례**이며, impact 57은 규모($65M)와 밸류 미공개 때문에 눌린 값이다. DX 인접성 단독 점수는 25/25 만점이다.
- **SambaNova**: 1차 클로징이며 추가 투자자 합류 예정. SN50 2026년 하반기 출하, SoftBank가 첫 배포 파트너. 라운드 확대 클로징 발표 시 UPDATE 대상.
- **Infinity**: $15M은 실리콘 스타트업에는 테이프아웃 한 번 값도 안 되지만, **26명 커널 엔지니어링 조직에는 3-4년치 런웨이**다. 자율 에이전트 Ignition이 신규 칩용 저수준 커널을 자동 작성한다. d-Matrix와의 사례에서 수개월-수년 작업을 수 시간-수일로 단축했다고 주장. 과금은 라이선스가 아니라 **성능 개선분(tokens/sec) 배분**.
- **Dnotitia**: 한국(서울, 2023년 설립). Seahorse(벡터 DB) + VDPU(벡터 데이터 처리 유닛, 세계 최초 주장). 국내 AI 반도체 중 드물게 메모리·데이터 경로를 노린다. Morgan Stanley 주관 IPO 준비 보도.

---

## System Software for NPU/LLM 집중  ※원문 최우선 관심축

이번 회차는 이 축이 **비어 있지 않고, 오히려 지배했다.**

| 회사 | 스택 위치 | 지원 하드웨어 | 라운드·금액 | 창 | 출처 |
|---|---|---|---|---|---|
| Modular (→ Qualcomm) | 컴파일러 + 런타임 (Mojo / MAX) | CPU · GPU · **NPU** · 커스텀 실리콘 | 인수 $3.9B, 7/29 클로징 | W8 | [Modular](https://www.modular.com/blog/qualcomm-completes-acquisition-of-modular) |
| Multiverse Computing | 양자화·압축 툴체인 (CompactifAI) | 스마트폰 · PC · 산업 설비 (엣지) | Series C $570M | W8 | [SiliconANGLE](https://siliconangle.com/2026/07/28/ai-model-compression-startup-multiverse-raises-570m-1-7b-valuation/) |
| Fireworks AI | 서빙 (추론 플랫폼) | GPU 중심 | Series D $1.5B | W4 | [Yahoo Finance](https://finance.yahoo.com/technology/ai/articles/fireworks-raises-1-5-billion-130000636.html) |
| Together AI | 서빙 + 추론 클라우드 | GPU | Series C $800M | W4 | [BusinessWire](https://www.businesswire.com/news/home/20260701243402/en/Together-AI-Raises-$800-Million-at-$8.3-Billion-Valuation-to-Make-Frontier-AI-Accessible-to-All) |
| Ollama | 로컬 런타임 · 모델 배포 | Mac · Windows · Linux (CPU+GPU 로컬) | Series B $65M | W4 | [TechCrunch](https://techcrunch.com/2026/07/09/popular-open-source-ai-developer-tool-ollama-raises-65m-grows-to-nearly-9m-users/) |
| Infinity | 커널 생성 · 컴파일 (CUDA 대안) | SRAM 계열 · GPU · **폰 칩** · 시스톨릭 어레이 | Seed $15M | W4 | [TechCrunch](https://techcrunch.com/2026/07/20/inference-startup-infinity-raises-15m-from-touring-capital-openai-and-athropic-researchers/) |

**한 줄 해석**: 자본은 스택의 **양 끝**에 몰렸다 - 최하단(커널·컴파일러: Modular $3.9B, Infinity $15M)과 최상단(서빙 플랫폼: Fireworks $1.5B, Together $800M). 중간 계층인 **그래프 최적화·양자화 툴체인**은 Multiverse($570M) 한 곳이 독점적으로 흡수했다. 우리 툴체인이 만나는 지점은 정확히 그 셋 모두다: ENN 컴파일러는 Modular와, 경량화 파이프라인은 Multiverse와, 온디바이스 실행기는 Ollama와 각각 같은 문제를 푼다. **가장 위험한 조합은 Qualcomm이 하드웨어 중립 컴파일러를 소유한 상태**이며, 이는 개발자가 Exynos용 코드를 별도로 쓰지 않게 만드는 방향으로 작동한다.

이번 회차 sys-software 지분 조달 합계 **$2,950M** (인수 $3,900M 별도) 대 npu-silicon **$1,300M**. 라운드 개수는 5:2다.

---

## M&A · 빅테크 자본지출 · 국가 펀드

- **Qualcomm ← Modular / 약 $3.9B(전액 주식) / 무엇을 산 것인가: 컴파일러·런타임 기술 + Chris Lattner를 포함한 인재 약 150명 + 개발자 생태계.** 2026-06-24 발표, **2026-07-29 클로징**. 상세는 핫 딜 [2].
- **onsemi ← Synaptics / 약 $7B(전액 주식) / 무엇을 산 것인가: 엣지 AI 프로세서 + 무선 연결 + HMI 제품군과 고객.** 2026-06-25 발표(W4 창 이전 - 맥락 인용), 클로징 목표 2027년 중반. onsemi는 TAM이 약 $30B 확대돼 2030년 약 $243B에 이른다고 제시. `[출처: New Electronics / EE Times]`
- **빅테크 AI capex 가이던스 상향(7/22 Q2 실적)**: Alphabet 2026년 capex를 **$180-190B → $195-205B**로 상향(구글 클라우드 매출 +82% YoY, 백로그 1분기에 $50B 증가해 $514B). Meta $125-145B, Microsoft 약 $190B(부품가 상승분 약 $25B 포함), Amazon 약 $200B. **4사 합계 최대 약 $725B, 2025년 대비 약 +77%.** `[출처: valueaddvc / Fortune / Stocktwits]`
- **MGX(아부다비) Fund I를 $49B에 클로징(7/1)** - 목표 $45B 초과. 투자 범위에 **반도체**를 명시. 향후 수년간 연 최대 $10B 집행 계획. 지금까지 14개사 투자(Anthropic·OpenAI·xAI 등 모델 계층 중심). `[출처: The National / PYMNTS]`
- **General Compute, Upper90으로부터 $400M 대출 확보(7/17)** - 추론 클라우드 전용 부채 조달. 지분이 아닌 **자산담보부 부채로 추론 캐파를 짓는 구조**가 확산 중이라는 신호. `[출처: TechCrunch]`

---

## 루머 · 협상 단계 (미확정)

> 이 섹션의 항목은 **확정 딜이 아니다.** 금액·밸류·성사 여부 모두 변할 수 있으며, 원장에도 `rumors`로만 기록했다.

- **삼성전자 → Mistral AI, EUR 10억(약 $1.1B) 투자 협상** - Mistral 밸류 약 EUR 200억 수준에서 논의. 삼성·Mistral 모두 공식 확인 없음. EQT Scaleup Europe Fund, Novo Holdings, Santander도 협상 중으로 보도. ASML이 Mistral 최대 주주. 삼성은 2024년 라운드에서 벤처 부문을 통해 이미 참여한 이력. 최초 관측 **2026-07-22**. `[출처: TechTimes / PYMNTS / Manila Times]`
- **Apple → AI 칩 스타트업 인수 물색** - 뱅커 접촉 및 반도체 스타트업 인수 의향 타진 보도. 서버용 AI 추론 칩 역량 보강 목적(M2 Ultra 서버 칩이 추론 워크로드에 부족). **타깃 미공개, 계약 없음.** 최초 관측 **2026-07-15**. `[출처: MacRumors]`
- **Apple → PrismML 협상** - 아이폰에서 대형 모델을 직접 실행하기 위한 압축 기술. PrismML은 메모리 14배 축소·추론 8배 가속·에너지 효율 5배 개선을 주장(회사 수치). 공동창업자에 Caltech 교수 Babak Hassibi. **미확정.** 최초 관측 **2026-07-10**. `[출처: MacDailyNews / Quartz]`
- **Qualcomm → Tenstorrent $8-10B 인수 협상 (부인됨)** - 6/16 The Information 보도 이후 **Tenstorrent CEO Jim Keller가 협상 사실을 부인**했다(도쿄 미디어 행사). Qualcomm은 대신 Modular를 인수했다. 최초 관측 **2026-06-16**, 원장 상태 `died`. `[출처: The Register / Yahoo Finance]`

---

## 누적 조달 리더보드

원장 기준(첫 회차이므로 **이번에 확인된 라운드만 합산**한 값이다 - 각 사의 실제 역대 누적과 다를 수 있음. 회차가 쌓이며 정확도가 올라간다).

| # | 회사 | 분야 | 누적 조달(원장) | 최신 밸류 | 최근 라운드 시점 | 삼성 계열 참여 |
|---|---|---|---|---|---|---|
| 1 | Fireworks AI | sys-software | $1,500M | $17.5B (구분 미확인) | 2026-07-16 | 없음 |
| 2 | Together AI | sys-software | $1,105M | $8.3B (post) | 2026-07-01 | 없음 |
| 3 | SambaNova | npu-silicon | $1,000M | $11B (구분 미확인) | 2026-07-08 | 없음 |
| 4 | Etched | npu-silicon | $800M | $10.3B (post) | 2026-07-23 | 없음 |
| 5 | Multiverse Computing | sys-software | $785M | $1.7B (pre) | 2026-07-28 | 없음 |
| 6 | Ollama | sys-software | $65M | 미공개 | 2026-07-09 | 없음 |
| 7 | Dnotitia | infra-memory | $61.2M | 미공개 | 2026-07-13 | 없음 |
| 8 | Infinity | sys-software | $15M | $100M (post) | 2026-07-20 | 없음 |
| - | Modular | sys-software | (인수 종결) | $3.9B 인수가 | 2026-07-29 | 없음 |

**삼성 계열 참여 확정 딜: 0건.** 이번 회차 원장에 등재된 9개사 중 삼성 주체가 들어간 라운드는 없다.

---

## 자본 배분 해석 (Where the money is going)

- **① 실리콘에서 실행 소프트웨어로.** 이번 회차 신규 확정 지분 조달 $4,311M 중 sys-software가 **$2,950M(68%)**, npu-silicon이 $1,300M(30%)이다. 인수 대가($3.9B Modular)까지 넣으면 sys-software 비중은 83%로 올라간다. 라운드 개수도 5:2로 소프트웨어가 앞선다. 다만 **4주 표본 1회차이므로 추세가 아니라 스냅샷**이다. 추세 판정은 4회차 누적 이후 가능하다.
- **② 컴파일러 계층이 "매입 대상"으로 재분류됐다.** Qualcomm-Modular($3.9B)는 반도체 기업이 개발자 대면 컴파일러·런타임을 **직접 소유하겠다**고 선언한 사건이다. 같은 창에서 Apple이 칩 스타트업 인수를 물색하고, AMD는 앞서 Untether AI 팀을 흡수해 **컴파일러·커널 역량**에 배치했다(2025-06, 맥락 인용). 실리콘 대기업 3사가 모두 소프트웨어·인재 쪽으로 자본을 돌리고 있다.
- **③ 온디바이스 경량화가 단독 카테고리가 됐다.** Multiverse $570M은 "모델 압축"만으로 5억 달러대를 받은 첫 사례다. 여기에 Apple-PrismML 협상 보도(미확정)와 Ollama의 로컬 실행 런타임 조달이 같은 4주에 겹친다. **금액 근거는 아직 딜 3건뿐이므로 표본 부족**이지만, 세 딜이 서로 다른 계층(압축·실행·실리콘 협상)에서 동시에 나온 점은 우연으로 보기 어렵다.
- **④ 삼성 계열의 침묵은 데이터로 확인된 사실이다.** 감시 대상 6개 삼성 투자 주체 전부 이번 창에 신규 발표가 없었고, 같은 창에서 SK hynix(Etched), NVIDIA(Fireworks·Together), HP(Multiverse), Pegatron(Together), Qualcomm(Modular 인수)은 각자 베팅을 마쳤다. 특히 **sys-software 열에 SK hynix를 제외한 주요 전략적 투자자 5곳이 모두 표식을 남겼고, 삼성 행은 비어 있다.** 확정 건 기준으로 이번 회차 삼성의 유일한 관측 활동은 Mistral 투자 협상 보도(루머)뿐이다.

---

## 생략 내역

**첫 회차 - 기보고 이력 없음.** `deal-ledger.json`이 빈 초기값이었으므로 중복 억제(SUPPRESS)로 생략한 딜은 0건이다.

동일 딜의 복수 보도는 규칙대로 하나로 병합했다(Etched 4개 매체 → 1항목, Multiverse 3개 → 1항목, Qualcomm-Modular 5개 → 1항목, Together AI 4개 → 1항목).

impact 임계 미달로 게재하지 않은 관측 항목(참고): Luxonis Series A $14M(7/2, 임베디드 비전, impact 35), Architect Labs Seed $24M(7/13, 칩 검증 EDA, impact 25), CuspAI Series B $450M(7/28, AI 소재 탐색 - §7 제외 대상). 7월 이전 항목(Baseten Series F $1.5B 6/22, Acrab $350M 6월, Axelera $250M+ 2월, Normal Computing $50M Samsung Catalyst 리드 3월, Rebellions 프리IPO 약 $400M 3월, FuriosaAI 프리IPO 약 8,000억원 5/28, DeepX Series D 밸류 2.65조원 6월)은 **수집 창 외부로 확정 딜 게재 대상이 아니며**, 다음 회차부터 창 내 후속 전개가 나오면 정식 등재한다.

---
*수집 소스 22건 검토 · 신규 확정 딜 9건(지분 $4,311M + 인수 $3,900M = 총 $8,211M) · 갱신 재보고 0건 · 루머 4건 · 중복 생략 0건(첫 회차)*

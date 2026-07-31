# On-Device LLM 기술 브리핑 - 2026-07-31

> **회차 성격: 초기 기준선(Baseline) 브리핑.** `seen-index`가 비어 있는 첫 실행이므로 최근 1주 신규뿐 아니라
> 상반기 누적 중 현재도 의사결정에 유효한 항목을 함께 싣는다. 다음 회차부터는 5일 중복 제거가 적용되어
> 신규·급상승 항목만 남는다.

**한 줄 요약**: 온디바이스 LLM의 병목이 연산(TOPS)에서 **메모리 대역폭·용량**으로 완전히 이동했고,
업계의 대응이 세 방향 - ① NPU 친화 **완전 정적 양자화**, ② **3D 스택 메모리 NPU**, ③ **디스크/플래시 스트리밍 실행** - 으로 갈라졌다.

---

## 임원 요약 (Executive Summary)

- **TOPS 경쟁은 사실상 끝났다.** Meta 온디바이스 AI 총괄(Vikas Chandra)이 공개한 2026 정세 문서는 모바일 50-90 GB/s vs 데이터센터 GPU 2-3 TB/s, **30-50배 대역폭 격차**를 병목으로 못 박았다. Exynos NPU 경쟁력 지표를 TOPS에서 **유효 대역폭 · 온칩 메모리 · 상주 모델 용량**으로 재정의할 시점이다.
- **Qualcomm은 메모리로 승부처를 옮겼다.** X2 Elite/Plus 전 제품군에 80 TOPS를 균일 배치해 연산을 상품화(commoditize)하고, **3D-DRAM 스택 NPU(40 TOPS + 4GB, 2026말~2027초)** 로 대역폭 축 선점을 노린다. 우리 로드맵의 대응 축이 TOPS면 이미 뒤진 경쟁이다.
- **경쟁 구도: 연산은 따라잡았고, 소프트웨어 스택이 남았다.** Exynos 2600(2nm)은 GenAI 성능 전세대 대비 +113%로 격차를 좁혔으나, Galaxy S26 실측 비교에서 Snapdragon 8 Elite가 여전히 우위. MediaTek NPU 990은 "에이전틱 AI + 전력효율 +56%"로 포지셔닝을 선점 중. 하드웨어 수치보다 **런타임/컴파일러 성숙도와 개발자 생태계**가 실사용 격차의 주 원인이다.
- **양자화 연구가 드디어 NPU 하드웨어 제약에 맞춰지기 시작했다.** 학계 주류 PTQ(AWQ/GPTQ)는 동적 활성 양자화를 전제해 NPU에서 못 돌았다. `Quant.npu`는 **정수 전용 완전 정적 양자화**로 이 간극을 메워 실기 NPU에서 지연 최대 15.1% 감소. 우리 ENN 툴체인의 양자화 프론트엔드가 이 흐름을 받고 있는지 점검이 필요하다.
- **런타임 표준화가 ExecuTorch 쪽으로 기울고 있다.** 기본 풋프린트 50KB, 백엔드 12종 이상(Apple/Qualcomm/Arm/MediaTek/Vulkan), HF 인기 엣지 LLM의 80% 이상이 무수정 동작. **Exynos/ENN 백엔드의 ExecuTorch 편입 여부가 향후 모델 공급 파이프라인 접근성을 결정**한다.

---

## 핫 이슈 (attention 60 이상)

### [1] Meta, "On-Device LLMs: State of the Union 2026" - 병목은 대역폭이라는 공식 선언
- **분류**: 전략/기준선
- **주체**: Vikas Chandra (Senior Director & Distinguished Scientist, AI @ Meta), Raghuraman Krishnamoorthi
- **핵심**: 온디바이스 LLM이 "연구 데모 → 양산 가능"으로 넘어왔다는 판정과 함께, 남은 제약을 대역폭으로 특정. 아키텍처 측면에서는 소규모 스케일에서 **deep-thin 구조(층 수 ↑, 차원 ↓)가 wide 구조보다 우세**하다는 결론, 그리고 프론티어 릴리즈의 60% 이상이 MoE를 채택했다는 관측을 제시.
- **실측(문서 인용치)**: 모바일 대역폭 50-90 GB/s vs DC GPU 2-3 TB/s(30-50× 격차). NPU 연산은 Apple A19 Pro ~35 TOPS, Snapdragon 8 Elite ~60 TOPS. MobileLLM 125M이 iPhone에서 50 tok/s. 스펙큘러티브 디코딩(Medusa/EAGLE) 2.2-3.6× 가속. KV 캐시 3비트 압축 시 품질 저하 미미.
- **DX 시사점**: NPU 스펙 경쟁 지표를 재정의해야 한다. 또한 "deep-thin이 우세"는 자사 소형 모델(Gauss 계열) 아키텍처 탐색 방향에 직접 적용 가능한 결론이다.
- **주목도**: 72/100 - Tier A 조직의 공식 정세 판단(+25), 워치리스트 연구자 저자(+20), 업계 인용 다수(+15), 정량 근거 포함(+10). 발행일 2026-01-24이나 **인용 기준선으로서 현재도 유효**.
- **상태**: NEW (기준선 등재)
- **출처**: https://v-chandra.github.io/on-device-llms/

### [2] Qualcomm - 80 TOPS 전 스택 균일화 + 3D-DRAM 스택 NPU 로드맵
- **분류**: 하드웨어 / 플랫폼
- **주체**: Qualcomm
- **핵심**: CES 2026에서 발표한 Snapdragon X2 Elite가 업계 예상(50 TOPS)을 넘는 **80 TOPS를 X2 Plus까지 전 제품 스택에 균일 적용** - 연산 성능을 차별화 요소에서 기본기로 내렸다. 이어지는 로드맵은 **3D-DRAM 적층 NPU(40 TOPS + 4GB 스택 메모리, 2026년 말~2027년 초)** 로, 경쟁축을 대역폭·상주 용량으로 옮기는 신호. 소프트웨어 쪽에서는 2026년 5월 LLMWare.ai와 함께 Snapdragon X PC에서 **SLM 기반 엔터프라이즈 에이전틱 워크플로**를 Hexagon NPU 가속으로 실행하는 사례를 공개, 엣지 에이전트 레퍼런스 선점 중.
- **실측**: Snapdragon 8 Elite 듀얼 Hexagon 코어에서 8B 모델 약 5 tok/s(Llama 3.1 8B 자동차 질의 5.1 tok/s, NPU 단독 실행). ※ 8B/5 tok/s는 실사용 체감 하한선에 가까움 - 대역폭 병목의 직접 증거.
- **DX 시사점**: 대응 축을 TOPS가 아닌 **메모리 서브시스템**으로 잡아야 한다. 3D 적층 NPU 메모리는 파운드리·패키징 역량과 직결되므로 SLSI-파운드리 간 공동 로드맵 검토 사안. 또한 "NPU 가속 에이전틱 SDK" 레퍼런스 부재가 우리 약점으로 굳어질 수 있다.
- **주목도**: 70/100 - Tier A 공식 발표(+25), 상용 제품 전이(+15), 광범위 언론 보도(+15), 실측 포함(+10), 단 로드맵 수치는 2차 출처(-5).
- **상태**: NEW (기준선 등재)
- **출처**: https://www.qualcomm.com/developer/blog/2026/05/local-agentic-ai-with-llmware-on-pcs-with-snapdragon-x-series · https://markets.financialcontent.com/woonsocketcall/article/tokenring-2026-1-8-qualcomm-shatters-ai-pc-performance-barriers-with-snapdragon-x2-elite-launch-at-ces-2026

### [3] Exynos 2600(2nm) vs Dimensity 9500 - 연산 격차는 좁혔고, 스택 격차가 남았다
- **분류**: 하드웨어 / 경쟁 구도
- **주체**: Samsung SLSI, MediaTek, Qualcomm
- **핵심**: Exynos 2600은 업계 최초 2nm 스마트폰 AP로 **GenAI 성능 전세대(2500) 대비 +113%**, 지연·전력 동시 개선을 내세웠고 Galaxy S26에 복귀. 다만 공개된 S26 실측 비교에서는 **Snapdragon 8 Elite가 여전히 우위**로 평가된다. MediaTek Dimensity 9500은 3nm이지만 **NPU 990 + 에이전틱 AI + 전력효율 +56%**로 온디바이스 LLM 지원을 명시적 마케팅 축으로 삼았다.
- **실측**: Exynos 2600 GenAI +113%(전세대 대비, 자사 발표), NPU 990 전세대 대비 2× 성능 / +56% 전력효율(자사 발표). ※ **양측 모두 벤더 자체 수치이며 동일 조건 3자 실측은 미확인.**
- **DX 시사점**: 공정 우위(2nm)를 확보했음에도 실사용 우위로 전환되지 않는 지점이 어디인지 - 컴파일러 최적화, 커널 커버리지, 메모리 플래닝, 개발자 SDK 접근성 중 무엇인지 - 를 특정하는 내부 실측이 필요하다. 경쟁사가 "온디바이스 LLM 지원"을 스펙 항목으로 내세우는 국면에서 **Exynos 측 대응 메시지(어떤 모델이 몇 tok/s로 도는지)가 공개 자료에 부재**한 것도 별도 리스크.
- **주목도**: 66/100 - 자사 핵심 경쟁 사안(+25), 광범위 보도(+15), 정량 수치(+10), 트레이드 프레스 수준 출처로 신뢰도 할인(+16 조정).
- **상태**: NEW (기준선 등재)
- **출처**: https://www.androidauthority.com/exynos-vs-snapdragon-galaxy-s26-benchmarks-3653459/ · https://www.gizmochina.com/2026/05/26/dimensity-9500-vs-exynos-2600-spec-sheet-benchmarks-and-more/

### [4] Google LiteRT-LM × Gemma 4 × ARM SME2 - 런타임이 SoC 명령어를 자동 선택하는 단계로
- **분류**: 런타임 / 컴파일러
- **주체**: Google AI Edge, ARM
- **핵심**: LiteRT-LM이 **Gemma 4의 크로스플랫폼 모바일·엣지 양산 인프라**로 자리잡았고, 더 중요한 변화는 최적화 자동화다. LiteRT는 런타임에 **XNNPACK + ARM KleidiAI 경유로 ARM SME2를 자동 감지·활용**하며, iGeMM/GeMM 같은 연산 집약 커널을 골라 전용 하드웨어 경로로 보낸다. 개발자가 SoC별 커널을 손으로 고르는 시대가 끝나간다는 의미. Google은 AI Edge Gallery를 Mac까지 확장(2026-06)해 배포 접점도 넓혔다.
- **실측**: 커널 선택 자동화의 절대 이득치는 공개 자료상 **미확인**. 확인된 것은 경로(XNNPACK→KleidiAI→SME2)의 존재.
- **DX 시사점**: Exynos NPU가 이 자동 선택 경로의 **1급 백엔드로 등재되어 있는지**가 실질 관건이다. 등재되지 않으면 앱 개발자는 아무 작업 없이 ARM CPU 경로로 떨어지고, 우리 NPU는 유휴 상태로 남는다. LiteRT 백엔드 등록 상태 점검을 실무 액션으로 제안.
- **주목도**: 65/100 - Tier A 공식(+25), 상용 SDK 즉시 적용(+15), ARM 협업(+15), 실측 미공개(+10).
- **상태**: NEW (기준선 등재)
- **출처**: https://developers.googleblog.com/blazing-fast-on-device-genai-with-litert-lm/ · https://developers.googleblog.com/accelerating-on-device-ai-a-look-at-arm-and-google-ai-edge-optimization/

### [5] ExecuTorch - 온디바이스 런타임의 사실상 표준 후보 (논문 공개, 2026-05)
- **분류**: 런타임
- **주체**: Meta / PyTorch Edge
- **핵심**: PyTorch 단일 경로로 온디바이스 배포를 통합하는 설계를 논문으로 정리해 공개(arXiv 2605.08195). 2025년 10월 1.0 GA 이후 Meta 자사 앱 양산 서비스에 투입 중.
- **실측**: 기본 런타임 풋프린트 **50KB**(MCU~플래그십 폰 공통), **하드웨어 백엔드 12종 이상**(Apple, Qualcomm, Arm, MediaTek, Vulkan), **HuggingFace 인기 엣지 LLM의 80% 이상 무수정 동작**.
- **DX 시사점**: 백엔드 목록에 Qualcomm·MediaTek이 있고 **Exynos/ENN이 없다**는 점이 핵심이다. 모델 공급 파이프라인(HF → ExecuTorch → 디바이스)에서 우리가 빠지면, 외부 모델을 우리 칩에 올릴 때마다 매번 별도 포팅 비용이 발생한다. ExecuTorch 백엔드 기여를 전략 과제로 검토할 근거.
- **주목도**: 64/100 - Tier A(+25), 상용 툴체인 즉시 적용(+15), 정량 실측(+10), 생태계 파급(+14).
- **상태**: NEW (기준선 등재)
- **출처**: https://arxiv.org/abs/2605.08195

### [6] Quant.npu - 학계 양자화와 NPU 하드웨어 제약의 간극을 메우는 완전 정적 양자화
- **분류**: 경량화 / 컴파일러 툴체인
- **주체**: Jinghe Zhang, Daliang Xu, Chenghua Wang, Weikai Xie, Tao Qi, **Yun Ma**, **Mengwei Xu**, Gang Huang (BUPT·PKU 계열 - ASPLOS'25 "Fast On-device LLM Inference with NPUs" 및 `mllm` 저장소와 동일 계보)
- **핵심**: 문제 정의가 정확하다. SOTA PTQ(AWQ/GPTQ 계열)는 **동적 활성 양자화**를 전제하는데, 모바일 NPU는 최적 효율을 위해 **완전 정적 양자화**를 요구한다 - 그래서 좋은 알고리즘이 NPU에서 안 돌았다. Quant.npu는 정수 전용 완전 정적 양자화에 **학습 가능한 양자화 파라미터 + 회전 행렬(rotation)**, 회전·비트폭 인지 초기화, 분포 인지 선택적 최적화(2단 파이프라인), **민감도 기반 적응 혼합정밀**을 결합.
- **실측**: 실기 모바일 NPU에서 **지연 최대 15.1% 감소**, 정확도는 SOTA 대비 동등. ※ 구체적 NPU 모델명·TTFT·메모리 수치는 초록 수준에서 **미확인** - 원문 정독 대상.
- **DX 시사점**: **ENN 툴체인 양자화 프론트엔드에 직접 이식 검토 대상.** 우리 NPU도 정적 양자화 제약을 갖는 구조라면, 외부 공개 모델을 그대로 받아 정확도 손실 없이 정적화하는 능력이 곧 "모델 공급 속도"다. 회전 기반 접근(SpinQuant 계열)이 NPU 정적 제약과 결합되는 흐름을 팀 차원에서 추적할 것을 권한다.
- **주목도**: 62/100 - 온디바이스 NPU 분야 최상위 그룹(+20), 실기 NPU 실측(+10), 상용 툴체인 직접 적용 가능(+15), 신규 프리프린트(+17).
- **상태**: NEW
- **출처**: https://arxiv.org/abs/2605.20295

### [7] Colibri - 의존성 0 순수 C 엔진, 디스크 expert 스트리밍으로 744B MoE를 25GB RAM에서
- **분류**: 런타임 / 시스템 기법
- **주체**: OSS (GitHub Trending, 2026-07)
- **핵심**: GLM-5.2(744B MoE)를 **필요한 expert만 디스크에서 스트리밍**해 소비자급 머신 RAM 약 25GB로 구동. 모델 전체를 메모리에 올린다는 전제를 깨는 접근으로, "LLM in a flash" 계열 기법이 대형 MoE와 순수 C 구현으로 실용화된 사례.
- **실측**: RAM 약 25GB로 744B MoE 구동. **처리량·TTFT·스토리지 I/O 부하는 미확인** - 검증 필요.
- **DX 시사점**: 기법의 방향이 우리 문제와 정확히 겹친다. 모바일·임베디드는 DRAM이 비싸고 **NAND는 상대적으로 넉넉하다.** "UFS에서 expert/레이어를 스트리밍해 DRAM 상주량을 줄이는" 설계는 Exynos + 자사 메모리 사업의 수직 통합 강점을 살릴 수 있는 드문 축이다. 단 NAND 수명·전력·지연 트레이드오프 실측이 선행 조건.
- **주목도**: 61/100 - 커뮤니티 급부상(+20), 시스템 기법 신규성(+15), 자사 수직통합 적합성(+15), 실측 미검증(+11).
- **상태**: NEW
- **출처**: https://aitoolradar.io/blog/open-source-ai-radar-july-2026

---

## 주목 항목 (attention 40-59)

| 항목 | 분류 | 주체 | 한 줄 요약 | 주목도 | 출처 |
|---|---|---|---|---|---|
| KV 캐시 압축 3종 (VeriCache / QuantSpec / Forward-Influence) | 경량화 | 다수 | VeriCache는 **full-KV와 출력 완전 동일**을 보장하며 처리량 최대 4×; QuantSpec는 희소 KV 자기-스펙큘러티브로 수락률 >90%·~2.5×; Forward Influence는 압축 토큰이 미래 문맥에 주는 영향을 지표화 | 58 | [HF 2606.26875](https://huggingface.co/papers/2606.26875) |
| TileFuse | 컴파일러 / 커널 | Wesley Pang, Deming Chen 외 (UIUC), 2026-07-03 v2 | AMD **XDNA2 NPU**(Ryzen AI 7 350 / 9 HX 370)용 AWQ 기반 융합 혼합정밀 GEMM/GEMV 커널 라이브러리 - NPU 타일 버퍼에 맞춘 퓨전. 정량 결과는 원문 정독 필요(미확인) | 56 | [arXiv 2606.11357](https://arxiv.org/pdf/2606.11357) |
| Atome LM | 모델 / 임베디드 | TilelliLab (OSS, 141★) | **$2 MCU급에서 도는 삼항 LM**. 60K 파라미터 → 플래시 20KB, ESP32-WROOM-32 실기에서 ~1 tok/s 일관 생성. TinyStories ppl 6.31 vs 8.12(파라미터 동급 -22%, 플래시 예산 동급 -52%). Python↔C99↔Cortex-M3 비트 단위 동일성 검증. 단 944K 규모에서는 baseline에 역전 | 55 | [GitHub](https://github.com/TilelliLab/atome-lm) |
| P3-LLM | 하드웨어 / 수치형식 | 학계, 2026-05 | 엣지 LLM용 **NPU-PIM 통합 가속기** + 하이브리드 수치형식 기반 유연 혼합정밀. PIM 축을 자사 메모리 사업 관점에서 볼 가치 | 48 | [arXiv 2511.06838](https://arxiv.org/html/2511.06838v4) |
| BitNet CPU 커널 최적화 · BitRL | 경량화 | Microsoft / 학계 | BitNet b1.58(ternary {-1,0,+1})의 병렬 커널로 2026-01 기준 원 구현 대비 1.15-2.1× 추가 가속. BitRL은 1비트 모델로 엣지 온디바이스 RL 학습·추론 | 45 | [microsoft/BitNet](https://github.com/microsoft/BitNet) · [arXiv 2604.24273](https://arxiv.org/html/2604.24273) |
| SpeContext (ASPLOS'26) | 경량화 | 학계 | 스펙큘러티브 문맥 희소성으로 롱컨텍스트 추론 효율화 - 온디바이스 롱컨텍스트의 KV 압박 완화 축 | 44 | [ASPLOS 2026 프로그램](https://www.asplos-conference.org/asplos2026/program/index.html) |

---

## 툴체인·릴리즈 노트

- [즉시] **LiteRT-LM**: Gemma 4 양산 경로 확정. ARM SME2를 XNNPACK+KleidiAI 경유로 **런타임 자동 감지**. Android 타깃이면 지금 적용 가능.
- [즉시] **ExecuTorch 1.0+**: 백엔드 12종 이상, 기본 50KB. HF 엣지 LLM 80%+ 무수정 동작. **Exynos/ENN 백엔드 부재**.
- [즉시] **llama.cpp**: CPU 추론 사실상 표준 유지, GGUF가 양자화 모델 유통 포맷으로 고착. Android LM Playground가 KleidiAI 커널 튜닝 적용.
- **MLX (Apple)**: Apple Silicon에서 표준 벤치 대비 30-50% 빠르다는 보고. Apple Intelligence Foundation Models 프레임워크는 iOS 26+ / iPhone 15 Pro 이상.
- **BitNet.cpp**: 2026-01 병렬 커널로 1.15-2.1× 추가 가속.
- **Hugging Face (2026-07)**: Mamba2 chunked-prefill 및 Zamba2/Nemotron-H/Bamba 스펙큘러티브 디코딩 수정 반영 - **하이브리드 SSM 계열의 배포 경로가 정비되는 신호**.
- **프레임워크 비교 벤치마크**: 2026-03-31 기준 ExecuTorch·LiteRT·llama.cpp·ONNX Runtime·CoreML 동일 조건 비교 연구 존재. 현재 정설은 **"모바일 양산=ExecuTorch, 데스크톱/프로토타이핑=llama.cpp, Apple=MLX"**. 비교 앱 소스: [PolyEngineInfer](https://github.com/FilipFan/PolyEngineInfer)

---

## 워치리스트 연구자 동향

- **Song Han (MIT HAN Lab)** - ① *Four Over Six: More Accurate NVFP4 Quantization with Adaptive Block Scaling* 및 적응형 블록 스케일 데이터 타입(2026-04 갱신) → **NVFP4/MXFP4 4비트 부동소수 포맷의 정확도 문제를 블록 스케일링으로 푸는 흐름**. 차세대 NPU 수치형식 설계에 직결. ② ASPLOS'26 *Taming the Long-Tail: Efficient Reasoning RL Training with Adaptive Drafter*. 기존 라인(AWQ·SmoothQuant·StreamingLLM)은 NVIDIA TensorRT-LLM에 채택된 상태.
- **Vikas Chandra · Raghuraman Krishnamoorthi (Meta)** - 위 [1] 정세 문서. 온디바이스 아젠다 세팅을 Meta가 주도.
- **Mengwei Xu · Daliang Xu (BUPT 계열)** - Quant.npu(위 [6]). ASPLOS'25 *Fast On-device LLM Inference with NPUs*(PowerNPU) 및 `mllm`(모바일 멀티모달 LLM 엔진)과 연속된 계보. **모바일 NPU 실기 최적화에서 현재 가장 일관된 산출을 내는 그룹** - 지속 추적 권장.
- **Deming Chen (UIUC)** - TileFuse(위 표). AMD NPU 커널 축.

---

## 레이더 (낮은 점수지만 전략적 신호)

- **모델 티어 이동**: 커뮤니티(r/LocalLLaMA, 약 74.9만 멤버) 기본값이 24GB 티어에서 Qwen3.6-27B로, 멀티모달 대안은 Gemma 4 31B. 소형 모델 진화 속도가 하드웨어 세대 교체보다 빠르다 - **칩 스펙을 특정 모델 크기에 맞춰 고정하는 설계는 위험**.
- **Diffusion LLM**: 병렬 토큰 정제로 4-6× 가속 가능성이 언급되기 시작. 자기회귀 전제의 NPU 데이터패스 설계에 중장기 변수.
- **초저전력 티어의 실체화**: 60K~600K 파라미터 삼항 모델이 ESP32에서 실제로 동작(위 Atome LM). 웨어러블·IoT·가전에서 "클라우드 없는 자연어 UI"의 하한선이 내려가는 중. Qualcomm도 Snapdragon Wear Elite에 온디바이스 NPU 탑재.
- **NPU-PIM 수렴**: P3-LLM처럼 NPU와 PIM을 한 칩에서 다루는 연구가 늘고 있다. 대역폭이 병목이라는 위 [1]의 결론과 정확히 맞물리며, **자사 메모리-로직 양쪽 자산을 가진 구조에서 가장 큰 레버리지 지점**.
- **참고 큐레이션**: [Awesome-On-Device-AI-Systems](https://github.com/jeho-lee/Awesome-On-Device-AI) - 다음 회차 수집 소스로 편입.

---

## 중복 제거 내역

첫 회차이므로 생략 항목 없음(0건). `seen-index.json`에 위 13개 항목이 등재되며, 다음 회차부터 5일 중복 제거가 적용된다.
급상승 조건(주목도 1.5× 상승 & 70 이상, GitHub 주간 스타 3× 또는 +2,000, HN/r/LocalLLaMA/HF Trending 신규 진입, Tier A 상용 SDK 채택)을 충족하면 5일 내라도 `[급상승 재보고]`로 다시 올라온다.

---

## 이번 회차 신뢰도 메모

정직한 보고를 위해 명시한다.

- **Quant.npu의 NPU 모델명·TTFT·메모리 수치**, **TileFuse의 정량 결과**, **Colibri의 처리량·I/O 부하**, **LiteRT SME2 자동 선택의 절대 이득치** 는 이번 회차에서 확인하지 못했다(초록/2차 출처 수준). 다음 회차 `--deep` 실행에서 원문 정독 대상으로 큐잉한다.
- **Exynos 2600 +113%**, **NPU 990 2×/+56%** 는 모두 **벤더 자체 발표치**이며 동일 조건 3자 실측이 아니다.
- **Qualcomm 3D-DRAM NPU 로드맵(40 TOPS + 4GB)** 은 트레이드 프레스 경유 2차 정보로, 공식 확인 필요.

---
*수집 소스 30+건 검토 · 신규 13건 · 급상승 재보고 0건 · 중복 생략 0건*

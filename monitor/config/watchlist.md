# On-Device LLM 모니터링 워치리스트

> `ondevice-llm-monitor` 서브 에이전트가 매 실행 시 참조하는 감시 대상 정의.
> 항목을 추가/삭제하면 다음 실행부터 자동 반영된다. 관심사가 바뀌면 이 파일만 고치면 된다.

## 0. 스코프 (우선순위 순)

1. **On-Device / Edge LLM 모델** - 소형·경량 파운데이션 모델, 모바일/임베디드 타깃 아키텍처
2. **Runtime** - 모바일·임베디드 추론 엔진, NPU/DSP/GPU 백엔드, 메모리·전력 관리
3. **Compiler / Toolchain** - 그래프 컴파일러, 커널 생성, 양자화 툴체인, 배포 파이프라인
4. **경량화(Model Efficiency)** - 양자화, 프루닝/스파시티, 증류, KV 캐시 압축, 스펙큘러티브 디코딩
5. **하드웨어-소프트웨어 공동 설계** - NPU 아키텍처, 온칩 메모리, 대역폭 병목, MLPerf류 벤치마크

**제외**: 순수 클라우드 대규모 학습, 데이터센터 전용 서빙(엣지 함의가 없는 경우), 응용 서비스 출시 뉴스,
모델 리더보드 점수만 갱신된 소식, 자금 조달/인사 뉴스(단, 온디바이스 NPU/런타임 업계 판도를 바꾸는 인수는 포함).

## 1. 기업 / 조직 (Tier A = 반드시 확인)

### Tier A - 실리콘 & 플랫폼
| 조직 | 감시 키워드 |
|---|---|
| Qualcomm | Snapdragon, Hexagon NPU, QNN, AI Engine Direct, QAIRT, AI Hub, Qualcomm AI Research |
| Apple | Apple Intelligence, AFM(Apple Foundation Model), Core ML, MLX, ANE, Neural Engine |
| Google | Gemini Nano, AI Edge, LiteRT(구 TFLite), MediaPipe, AI Edge Torch, XNNPACK, Gemma, Tensor G-series |
| Meta | ExecuTorch, PyTorch Edge, torchao, Llama(소형), MobileLLM, SpinQuant |
| ARM | KleidiAI, Ethos-U NPU, SME2, Arm Compute Library, Vela 컴파일러 |
| Samsung (자사/경쟁 관점) | Exynos NPU, ENN SDK, Gauss, SLSI NPU 컴파일러 |
| MediaTek | Dimensity, NeuroPilot, APU |
| NVIDIA | Jetson, TensorRT-LLM, Nemotron-Nano, DRIVE, cuDNN/CUTLASS 커널 |
| Microsoft | Phi 시리즈, ONNX Runtime, DirectML, Windows Copilot Runtime, BitNet.cpp |
| Intel / AMD | OpenVINO, NPU(Lunar Lake), Ryzen AI, XDNA, Vitis AI |

### Tier B - 모델 / OSS 런타임
Hugging Face(optimum, **optimum-executorch**, transformers.js, TRL, candle), Mistral,
Alibaba Qwen(+MNN), DeepSeek, Moonshot, ggml/llama.cpp, Ollama, LM Studio, MLC-LLM,
vLLM·SGLang(엣지 파생), ncnn·MNN·TNN, Modular MAX/Mojo, Apache TVM, IREE/MLIR, OpenAI Triton.

### Tier B-2 - 엣지 포맷 변환·재배포 커뮤니티 (모델 공급 속도를 결정하는 층)
신규 모델이 **얼마나 빨리 엣지에서 돌 수 있는 형태로 바뀌는지**를 결정하는 실질적 병목 지점.
공식 릴리즈보다 이 층의 반응 속도가 실제 배포 가능 시점을 좌우한다.

| 대상 | 감시 포인트 |
|---|---|
| HF 조직: `ggml-org`, `unsloth`, `bartowski`, `mradermacher`, `TheBloke` 계승 계정 | 신규 모델의 **GGUF 변환 등장 시점(T+몇 시간/일)** 및 양자화 등급 커버리지(Q4_K_M, IQ4_XS 등) |
| `mlx-community` | Apple Silicon 변환 커버리지 |
| `optimum-executorch` / `executorch-community` | **ExecuTorch 익스포트 지원 모델 목록 확대** - 아키텍처별 지원/미지원 경계 |
| `litert-community` / Google AI Edge HF 조직 | `.litertlm` / `.task` 번들 배포 모델, MediaPipe LLM Inference 지원 목록 |
| Qualcomm AI Hub (HF 미러 포함) | Snapdragon 사전 최적화 모델 카탈로그 증가분 |
| ONNX / `onnxruntime-genai` 모델 배포 | Windows·NPU 경로 커버리지 |
| Ollama · LM Studio 모델 라이브러리 | 일반 사용자 접점 도달 시점 |

**핵심 관측 지표**: 신규 소형 모델 발표 → 각 엣지 포맷 등장까지의 **지연(T+)**, 그리고 **어떤 포맷이 먼저
나오는지의 순서**. 이 순서가 곧 각 하드웨어 생태계의 실효 우선순위다. Exynos/ENN 포맷이 이 목록에
없다면 그 자체가 리포트에 남길 신호다.

### Tier C - 임베디드 / NPU 전문 업체
Hailo, Axelera, Kneron, DEEPX, FuriosaAI, Rebellions, Mobilint, SiMa.ai, Blaize,
Espressif(ESP-NN), ST(X-CUBE-AI/STM32N6), Renesas, TI(Edge AI), NXP(eIQ), Ambiq, Syntiant,
GreenWaves, Infineon, Qualcomm Dragonwing(IoT), Synaptics, Cadence Tensilica, Ceva.

## 2. 연구 그룹 / 교수 (개인 단위 추적)

| 연구자 | 소속 | 대표 라인 |
|---|---|---|
| Song Han | MIT HAN Lab | AWQ, SmoothQuant, StreamingLLM, QServe, TinyChat, TinyML/MCUNet, SVDQuant, DuoAttention |
| Tri Dao | Princeton / Together | FlashAttention, Mamba/SSM, 하드웨어 인지 커널 |
| Tianqi Chen | CMU / OctoAI | Apache TVM, MLC-LLM, XGrammar |
| Dan Alistarh | ISTA | GPTQ, SparseGPT, QuIP#, Marlin, HIGGS, 극저비트 압축 |
| Tim Dettmers | CMU / AI2 | bitsandbytes, QLoRA, SpQR, 8/4비트 스케일링 법칙 |
| Amir Gholami / Kurt Keutzer / Sehoon Kim | UC Berkeley | SqueezeLLM, KVQuant, LLM 추론 비용 분석, 효율 서베이 |
| Luca Benini | ETH Zurich / U. Bologna | PULP, RISC-V 기반 초저전력 NPU, GAP 프로세서 |
| Markus Nagel / Tijmen Blankevoort | Qualcomm AI Research(→Meta) | AdaRound, Data-Free Quantization, 양자화 백서 |
| Beidi Chen | CMU | Deja Vu, MagicPIG, 컨텍스트 스파시티 |
| Mohamed Abdelfattah | Cornell | 하드웨어 인지 스파시티, LLM 가속기 |
| Mi Zhang | Ohio State | Edge/Mobile LLM 서베이, 온디바이스 파인튜닝 |
| Vijay Janapa Reddi | Harvard | MLPerf Tiny, TinyML 벤치마킹 |
| Zechun Liu | Meta Reality Labs | MobileLLM, BitDistiller, 초경량 아키텍처 |
| Christopher De Sa | Cornell | QuIP, 저비트 이론 |
| Nicholas D. Lane | Cambridge / Flower | 온디바이스 학습, 페더레이티드 |
| Yiran Chen | Duke | 엣지 AI 시스템, 하드웨어 공동 설계 |
| Torsten Hoefler | ETH Zurich | 스파시티, 분산·양자화 시스템 |

## 3. 기술 트랙 (키워드 세트)

- **양자화**: GPTQ, AWQ, SmoothQuant, QuaRot, SpinQuant, QAT, W4A8/W4A4, MXFP4/NVFP4, FP8,
  BitNet / 1.58-bit, ternary, rotation-based, outlier smoothing, per-channel/group scaling
- **KV 캐시**: KVQuant, KIVI, H2O, SnapKV, Quest, PagedAttention 엣지 변형, 슬라이딩 윈도우, cross-layer 공유
- **디코딩 가속**: speculative decoding, EAGLE, Medusa, self-draft, lookahead, tree attention, prefill/decode 분리
- **구조 경량화**: depth/width pruning(SliceGPT, ShortGPT), structured sparsity(2:4), MoE 엣지 적용, LoRA 어댑터 스와핑
- **아키텍처 대안**: Mamba/SSM, 선형·하이브리드 어텐션, sub-quadratic, 로컬 어텐션, small-vocab 설계
- **런타임/컴파일러**: 메모리 플래닝, 오퍼레이터 퓨전, 커널 오토튜닝, 그래프 파티셔닝(NPU/CPU 분할),
  dynamic shape, flash-memory offloading("LLM in a flash"), 전력·발열 스로틀링, 토크나이저·샘플러 오버헤드
- **평가**: MLPerf Client / Mobile / Tiny, 실측 TTFT·TPS·전력(W)·메모리(GB), 정확도 저하율(perplexity/task)

## 4. 소스 (수집 채널)

**논문/프리프린트**: arXiv cs.LG · cs.AR · cs.DC · cs.CL(효율 관련), Hugging Face Daily Papers,
Papers with Code, MLSys / ASPLOS / ISCA / MICRO / HPCA / CGO / OSDI / EuroSys / MobiCom / MobiSys / SenSys / DAC 프로시딩·억셉트 리스트

**공식 채널**: 각 사 developer blog & 릴리즈 노트(Qualcomm, Apple ML Research, Google Developers/DeepMind,
PyTorch/ExecuTorch, ARM Community, NVIDIA Developer, Microsoft Research, OpenVINO), GitHub 릴리즈 태그

**커뮤니티/주목도**: GitHub Trending 및 스타 증가율, Hacker News 프론트, r/LocalLLaMA 상위,
Hugging Face Trending(모델·스페이스), X/Twitter 핵심 계정, tinyML / Edge AI Foundation 발표

**모델 공급 파이프라인**(§1 Tier B-2 대응): HF Models 신규·트렌딩 목록을 `gguf` / `executorch` /
`litert` / `mlx` / `onnx` 태그와 라이브러리 필터로 조회, `optimum-executorch` 지원 모델 목록 변화,
Qualcomm AI Hub 모델 카탈로그, Ollama·LM Studio 라이브러리 신규 등재.
목적은 벤치마크 점수가 아니라 **"어떤 모델이 지금 어떤 하드웨어에서 즉시 돌아가는가"**의 경계선 추적이다.

## 5. 주목도(Attention) 판정 기준

각 항목에 `attention` 점수(0-100)를 부여한다. 근거를 반드시 리포트에 함께 남긴다.

| 신호 | 배점 |
|---|---|
| Tier A 기업의 공식 릴리즈/논문 | +25 |
| 워치리스트 등재 연구자가 저자 | +20 |
| GitHub 주간 스타 +500 이상 또는 증가율 급등 | +20 |
| HN 프론트페이지 / r/LocalLLaMA 상위 노출 | +15 |
| HF Daily Papers 상위 · Trending 모델 진입 | +15 |
| 실제 디바이스 실측치(전력·지연) 포함 | +10 |
| 탑티어 학회 억셉트 | +10 |
| 상용 SDK/툴체인에 반영(즉시 적용 가능) | +15 |
| **엣지 포맷 변환 가용**(GGUF·ExecuTorch·LiteRT·MLX·ONNX 중 2종 이상 등장 = 즉시 실행 가능) | **+15** |
| **모델 공급 경계 변화**(신규 아키텍처가 특정 런타임에서 처음 지원됨 / 지원 목록에서 빠짐) | **+10** |

`attention >= 60`이면 **HIGH**, 40-59 **MEDIUM**, 40 미만은 원칙적으로 리포트에서 제외(단, Tier A
전략 변화 신호는 낮은 점수라도 "레이더" 섹션에 한 줄로 남긴다).

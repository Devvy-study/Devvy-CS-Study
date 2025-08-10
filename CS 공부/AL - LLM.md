# AL - LLM

날짜1: 2025년 7월 26일
번호: 1주차 개인 CS 공부

## LLM (Large Language Model)이란?

> "대규모 텍스트 데이터로 학습된, 인간처럼 자연어를 이해하고 생성할 수 있는 모델"
> 
- **기반 기술**: Transformer (2017, Google 논문 "Attention is All You Need")
- **입력 → 출력**: 질문(문장)을 넣으면 문맥에 맞는 텍스트, 코드, 요약, 번역 등 생성

---

## 대표 LLM 모델 비교

| 모델 | 기업/조직 | 특징 |
| --- | --- | --- |
| **GPT-4 / GPT-4o** | OpenAI | 고성능, 다 multimodal (텍스트+이미지+음성 가능), Copilot 기반 |
| **Claude 3** | Anthropic | 윤리성, 정확성 강조, 장문 요약 강함 |
| **Mistral** | Mistral | 오픈소스 기반, 성능 좋고 가볍다 (빠름) |
| **Gemini** | Google DeepMind | 멀티모달 강점, 검색 연동 |
| **LLaMA 3** | Meta | 오픈소스, 연구용으로 인기 |
| **Command R+** | Cohere | RAG에 특화된 LLM |
| **Yi / Qwen** | 중국계 (01.ai, Alibaba 등) | 성능 좋은 중국계 오픈소스 LLM |

---

## 핵심 기술 키워드 정리 (면접 대비용)

| 주제 | 키워드 |
| --- | --- |
| **기본 구조** | Transformer, Self-Attention, Positional Encoding |
| **학습 방식** | Pretraining (자연어 corpus), Fine-tuning, RLHF |
| **모델 활용 방식** | Prompt Engineering, Retrieval-Augmented Generation (RAG), Few-shot / Zero-shot Learning |
| **멀티모달** | 텍스트 + 이미지 + 음성 (GPT-4o, Gemini 등) |
| **LLM 한계** | 헛소리(Hallucination), 최신 정보 반영 어려움, 추론 비용 |
| **대표 응용** | 챗봇, 문서 요약, 코드 생성, 자동화, 질의응답, 검색 개선 등 |

---

## 관련 개념 간단 요약

| 용어 | 설명 |
| --- | --- |
| **Transformer** | 기존 RNN/LSTM 대비 병렬처리가 가능한 구조. LLM의 핵심 기반 |
| **Self-Attention** | 문장 내 단어 간 연관성 계산 (ex. "it"이 무엇을 가리키는지) |
| **Fine-Tuning** | 특정 도메인 데이터로 기존 LLM을 추가 학습 |
| **RLHF** | 사람 피드백으로 보정하는 방식 (Reinforcement Learning with Human Feedback) |
| **Prompt Engineering** | 모델에게 원하는 출력을 유도하는 입력 설계 |
| **RAG** | LLM이 문서를 직접 기억하지 않고, 외부 지식과 결합해서 생성 |
| **Embedding** | 문장을 고차원 벡터로 변환하여 유사도 비교 가능하게 함 |

---

## 주요 LLM 아키텍처 비교

| 항목 | **GPT-4 / GPT-4o** | **Claude 3** | **Mistral / Mixtral** | **Gemini 1.5** | **LLaMA 3** |
| --- | --- | --- | --- | --- | --- |
| **개발사** | OpenAI | Anthropic | Mistral AI | Google DeepMind | Meta |
| **출시일** | GPT-4 (2023), GPT-4o (2024) | 3 (2024.3) | Mistral 7B (2023.9), Mixtral (2023.12) | 1.5 (2024.2) | 3 (2024.4) |
| **모델 크기** | GPT-4: 수백억~1조+ 추정 (비공개)GPT-4o: 멀티모달 통합 | Claude 3 Opus: 약 400B 추정 (비공개) | Mistral 7B / Mixtral 12.7B (MoE) | 1.5 Pro: 수천억 추정 (비공개) | 8B, 70B 공개 |
| **멀티모달 지원** | ✅ GPT-4o: 텍스트+음성+이미지+비디오 | ❌ (텍스트 중심) | ❌ (텍스트 전용) | ✅ 이미지/음성 (멀티모달 강화) | ❌ 텍스트 중심 |
| **아키텍처 특징** | - GPT-4o: **One model** for all modalities- 높은 압축률과 정확도- Reinforcement + Search Feedback | - Constitutional AI (헌법 기반 피드백)- 장문 처리 최적화- 사람처럼 친절하고 논리적 | - **Decoder-only** 구조- Mixtral: MoE (활성화 expert 2/8)- Fast inference / Open-source | - **Mixture of Experts + Multimodal unified architecture**- 문맥 길이 ↑ | - Transformer 기반, 효율성 ↑- 학습용도/추론성능 뛰어남 |
| **문맥 길이** | GPT-4: 8k~128kGPT-4o: 128k | Claude 3 Opus: 최대 200k | Mistral: 8k~32k | Gemini 1.5: 최대 1M(!) | LLaMA 3 70B: 약 8k~ |
| **RAG/검색 연동** | Azure/ChatGPT Enterprise | Claude API with tools | 직접 연동 필요 | DeepMind/Vertex AI | 직접 연동 필요 |
| **코드 생성 능력** | 강력 (Copilot 기반) | 안정적, 덜 공격적 | 중간 (7B 수준) | 경쟁력 있음 | 중간 |
| **오픈소스 여부** | ❌ | ❌ | ✅ (Apache 2.0) | ❌ | ✅ (LLaMA 3) |
| **상용 접근성** | ChatGPT / API | Claude API | HuggingFace 등 | Gemini Pro (Bard) | Meta API, HuggingFace |

---

## 🔍 추가 설명 요약

### 🧠 GPT-4 / GPT-4o (OpenAI)

- **비공개 모델**이지만, 매우 높은 성능.
- **GPT-4o는 하나의 모델로 멀티모달 통합 처리**.
- RLHF + Tool Use + Azure 기반 API 연동으로 엔터프라이즈 활용 활발.

### 🧠 Claude 3 (Anthropic)

- **"헌법 기반 AI"** → 규칙 기반의 행동 강화 (Constitutional AI)
- 장문 처리, **논리적 사고, 요약**, 윤리적 응답에서 강점.
- 보수적이지만 실무용 챗봇으로 매우 인기.

### 🧠 Mistral / Mixtral (Mistral AI)

- **오픈소스 모델 중 최강 성능**
- Mixtral은 **MoE (Mixture of Experts)** 구조로 빠르고 효율적.
- HuggingFace에서 바로 사용 가능, 벡터 DB와 함께 RAG 기반 챗봇에 활용됨.

### 🧠 Gemini (Google DeepMind)

- 유튜브, 검색, GDocs 등 **구글 생태계 통합 최적화**
- Gemini 1.5는 최대 **100만 토큰 문맥 처리** 지원
- 멀티모달 자연스러움 강화, 다만 일반 사용자는 제한적.

### 🧠 LLaMA 3 (Meta)

- **연구·실험용 오픈소스 LLM**
- 매우 뛰어난 성능/크기 대비 효율성.
- LLM 튜닝, 파인튜닝 실습, 임베딩 학습 등에 적합.

---

### 현재 진행 중인 프로젝트에서 AI 기술을 openAI를 통해서 활용하기로 하였으며, 이에 대해 텍스트 부분은 GPT-4o를, 검증 부분에서는 **ext-embedding-3-large를 활용하기로 하였으며 이유는 아래와 같이 정리 가능하다.**

## **텍스트 모델: GPT-4o 선택 이유**

| 비교 항목 | GPT-4o | 다른 후보 대비 장점 |
| --- | --- | --- |
| **성능** | OpenAI 최신 플래그십 멀티모달 모델, GPT-4 수준 이상 정밀도 | GPT-4.1 대비 응답 속도가 더 빠르면서 멀티모달 기능 통합 |
| **속도/비용 균형** | GPT-4 대비 비용↓, GPT-3.5 대비 품질↑ | 장문·복잡 질의도 처리 가능 |
| **멀티모달** | 텍스트·이미지·음성 입력 모두 처리 가능 | Gemini Pro도 가능하지만, GPT-4o는 단일 모델로 통합 운영 |
| **맥락 길이** | 최대 128k 토큰(대량 문서 처리 가능) | GPT-4(8k/32k) 대비 4~16배 긴 컨텍스트 처리 |

## **임베딩 모델: text-embedding-3-large 선택 이유**

| 비교 항목 | text-embedding-3-large | 다른 후보 대비 장점 |
| --- | --- | --- |
| **정확도** | OpenAI의 최고 성능 임베딩 모델, 의미 유사도·검색 품질 우수 | small 모델 대비 semantic search 성능 향상 |
| **언어 지원** | 영어 외 다양한 언어에서 강력한 의미 분석 | multilingual 환경에서 안정적 |
| **차원 수** | 3,072차원 → 정밀한 의미 매핑 가능 | small 모델(1,536차원) 대비 정교함↑ |
| **비용 대비 가치** | 대규모 검색·QA에서 리콜·정밀도 균형이 뛰어남 | 모델 경량화보다 품질 우선 시 적합 |

반응 속도 관련하여 공식적인 소요 시간에 대한 정보는 없으며, 일반적으로 large 모델이 시간이 더 걸리는 것이 많지만, Helicone 실 사용자 기반 데이터에 따르면 small 모델이 시간이 더 걸릴 때도 있다고 나와있다. 이에 따라 반응 시간보다 정확도에 비중을 두어 위의 모델을 선택하였다. 

---

## 🛠️ 실무 예시 (면접 대비 포인트)

- LLM + OCR → 문서 자동 처리
- LLM + 벡터 검색 → 사내 문서 QA 챗봇
- LLM + TTS/STT → 음성 기반 상담 자동화
- LLM + LangChain → 워크플로우 자동화

---

```sql
✅ 1. Transformer는 기존 RNN과 비교해 어떤 점이 개선되었나요?
답변:

RNN은 순차적으로 데이터를 처리해서 병렬 처리가 어려웠고, 긴 문장에서 정보가 소실되는 문제가 있었습니다.

Transformer는 Self-Attention 메커니즘을 통해 문장 내 모든 단어 관계를 한 번에 계산할 수 있어, 병렬 처리가 가능하고 장기 의존성도 잘 처리합니다.

✅ 2. LLM이 헛소리를 하는 이유는 무엇인가요?
답변:

LLM은 확률적으로 다음 단어를 예측하는 구조라서, 정답을 "암기"하는 게 아니라 "그럴듯한 답"을 생성합니다.

그래서 훈련 데이터에 없거나 불완전한 정보에 대해 추론 기반으로 말하다가 사실과 다른 말(헛소리, hallucination) 을 하게 됩니다.

✅ 3. Prompt Engineering과 Fine-tuning의 차이를 설명해보세요.
답변:

Prompt Engineering은 기존 모델을 그대로 사용하되, 입력을 잘 구성해 원하는 출력을 유도하는 방법입니다.

Fine-tuning은 LLM을 특정 데이터로 추가 학습시켜 모델 자체를 수정하는 방식이며, 더 일관된 성능을 낼 수 있지만 비용과 리스크가 큽니다.

구분	Prompt Engineering	Fine-tuning
방식	입력 설계	모델 재학습
비용	거의 없음	매우 높음
유연성	즉시 테스트 가능	고정적 결과
적용 상황	테스트/PoC	상용화/고정 태스크

✅ 4. RAG가 무엇이고 어떤 상황에서 쓰이나요?
답변:

**RAG (Retrieval-Augmented Generation)**은 LLM이 기억하지 못하는 외부 지식을 벡터 검색 등을 통해 찾아서, 그 내용을 바탕으로 답변을 생성하는 방식입니다.

주로 최신 정보 반영, 사내 문서 QA, 개인화된 응답 등에 사용됩니다.

예: "오늘 삼성전자 주가 알려줘" → LLM은 몰라요 → RAG가 주가 정보를 검색해서 응답 생성

✅ 5. LLM에서 Positional Encoding이 필요한 이유는?
답변:

Transformer는 RNN처럼 순서를 따라가지 않기 때문에, 문장 내 단어의 위치 정보를 직접 추가해줘야 합니다.

Positional Encoding은 각 단어에 위치 기반의 벡터를 더해서 순서 정보를 모델이 학습할 수 있도록 도와줍니다.

✅ 6. LLM이 멀티모달로 동작하려면 어떤 구조가 필요하나요?
답변:

멀티모달 LLM은 텍스트 외에도 이미지, 음성 등을 이해하거나 생성할 수 있어야 하므로,

각 modality에 특화된 encoder/decoder(예: Vision Encoder, Audio Encoder)가 필요하고,

이들을 텍스트 기반 LLM과 통합하는 구조로 설계됩니다.

예: GPT-4o는 음성 입력 → 음성→텍스트 변환(STT) + 시각정보 처리 + 텍스트 응답 생성 → TTS로 응답하는 전체 파이프라인을 통합적으로 처리

```
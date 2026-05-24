---
title: 오픈소스 라이선스 — 채아챗
permalink: /oss-licenses-ko.html
---
**언어**: [English](oss-licenses.html) · [한국어](oss-licenses-ko.html)
**문서**: [사용 가이드](guide-ko.html) · [개인정보처리방침](privacy-ko.html) · [이용약관](terms-ko.html) · [이용정책](policy-ko.html) · [오픈소스 라이선스](oss-licenses-ko.html)

---

# 오픈소스 라이선스

채아챗은 수많은 훌륭한 오픈소스 프로젝트 위에서 동작합니다. 모든 메인테이너와 기여자분께 감사드립니다. 본 페이지는 채아챗이 직접 의존하는 모든 라이브러리와 그 라이선스를 정리합니다.

각 라이선스 명을 누르면 해당 프로젝트의 라이선스 전문이 열립니다.

## 폰 내 추론 (네이티브)

| 프로젝트 | 저작권 | 라이선스 |
|---|---|---|
| [llama.cpp](https://github.com/ggerganov/llama.cpp) | Georgi Gerganov 외 | [MIT](https://github.com/ggerganov/llama.cpp/blob/master/LICENSE) |
| [GGML](https://github.com/ggerganov/ggml) | Georgi Gerganov 외 | [MIT](https://github.com/ggerganov/ggml/blob/master/LICENSE) |
| [KleidiAI](https://github.com/ARM-software/kleidiai) | Arm Limited | [Apache-2.0](https://github.com/ARM-software/kleidiai/blob/main/LICENSE) |
| [nlohmann/json](https://github.com/nlohmann/json) | Niels Lohmann | [MIT](https://github.com/nlohmann/json/blob/develop/LICENSE.MIT) |
| ai_chat.cpp (JNI 어댑터) | Han Yin (Arm) — llama.cpp `examples/llama.android` 차용 | [MIT](https://github.com/ggerganov/llama.cpp/blob/master/LICENSE) |

네이티브 라이브러리 `libai-chat.so` 는 위 소스들을 Android NDK r26.1 + 본 저장소의 CMake 설정으로 빌드합니다 (독점 수정 없음). Kotlin 래퍼 (`com.cayber.aichat.localllm.*`) 도 동일 MIT 라이선스로 `examples/llama.android/lib` 를 차용 — 패키지 namespace 만 변경했습니다.

## Compose / AndroidX

| 프로젝트 | 라이선스 |
|---|---|
| [Jetpack Compose (BOM 2024.x)](https://developer.android.com/jetpack/compose) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| AndroidX Core / Activity / Lifecycle / Navigation / DataStore / Room / AppCompat | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| Material Components for Android | [Apache-2.0](https://github.com/material-components/material-components-android/blob/master/LICENSE) |
| Material Icons Extended | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |

## Kotlin 표준 라이브러리 / 코루틴 / 직렬화

| 프로젝트 | 라이선스 |
|---|---|
| [kotlin-stdlib](https://github.com/JetBrains/kotlin) | [Apache-2.0](https://github.com/JetBrains/kotlin/blob/master/license/LICENSE.txt) |
| [kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines) | [Apache-2.0](https://github.com/Kotlin/kotlinx.coroutines/blob/master/LICENSE.txt) |
| [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization) | [Apache-2.0](https://github.com/Kotlin/kotlinx.serialization/blob/master/LICENSE.txt) |

## 네트워크 / 저장소

| 프로젝트 | 라이선스 |
|---|---|
| [OkHttp](https://github.com/square/okhttp) (logging-interceptor 포함) | [Apache-2.0](https://github.com/square/okhttp/blob/master/LICENSE.txt) |
| [Okio](https://github.com/square/okio) | [Apache-2.0](https://github.com/square/okio/blob/master/LICENSE.txt) |
| [AndroidX DataStore](https://developer.android.com/topic/libraries/architecture/datastore) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| [AndroidX Room](https://developer.android.com/training/data-storage/room) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |

## 콘텐츠 렌더링 / 파싱

| 프로젝트 | 라이선스 |
|---|---|
| [Coil](https://github.com/coil-kt/coil) (이미지 로딩) | [Apache-2.0](https://github.com/coil-kt/coil/blob/main/LICENSE.txt) |
| [halilibo Compose RichText](https://github.com/halilozercan/compose-richtext) | [Apache-2.0](https://github.com/halilozercan/compose-richtext/blob/main/LICENSE) |
| [Commonmark Java](https://github.com/commonmark/commonmark-java) | [BSD-2-Clause](https://github.com/commonmark/commonmark-java/blob/main/LICENSE.txt) |
| [Jsoup](https://github.com/jhy/jsoup) (fetch_url 도구의 HTML→텍스트) | [MIT](https://github.com/jhy/jsoup/blob/master/LICENSE) |
| [PdfBox-Android](https://github.com/TomRoush/PdfBox-Android) (PDF 본문 추출 + create_file PDF 생성) | [Apache-2.0](https://github.com/TomRoush/PdfBox-Android/blob/master/LICENSE) |
| [BouncyCastle](https://www.bouncycastle.org/) (PDF 보안 처리, 선택) | [MIT 계열](https://www.bouncycastle.org/licence.html) |

## 빌드 도구 (APK 에 미포함되나 명시)

| 프로젝트 | 라이선스 |
|---|---|
| [Android Gradle Plugin](https://developer.android.com/build/releases) | [Apache-2.0](https://android.googlesource.com/platform/tools/base/+/refs/heads/mirror-goog-studio-main/LICENSE) |
| [Kotlin Symbol Processing (KSP)](https://github.com/google/ksp) | [Apache-2.0](https://github.com/google/ksp/blob/main/LICENSE) |
| [Gradle](https://gradle.org/) | [Apache-2.0](https://github.com/gradle/gradle/blob/master/LICENSE) |
| Android NDK r26.1 (`libai-chat.so` 빌드에만 사용) | 복수 OSS — [NDK 페이지](https://developer.android.com/ndk) |

## GGUF 모델 (사용자가 직접 다운로드 — 앱과 함께 배포 X)

폰 내 추론 카탈로그의 모델은 사용자가 탭했을 때만 HuggingFace 에서 직접 다운로드됩니다. 각 모델은 자체 라이선스를 따릅니다:

| 모델 | 발행자 | 라이선스 |
|---|---|---|
| Qwen2.5 0.5B / 1.5B Instruct (GGUF) | Alibaba (Qwen) | [Apache-2.0](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct/blob/main/LICENSE) |
| Qwen2.5 3B Instruct (GGUF) | Alibaba (Qwen) | [Qwen Research License](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct/blob/main/LICENSE) |
| Llama 3.2 1B / 3B Instruct (GGUF) | Meta (via bartowski) | [Llama 3.2 Community License](https://huggingface.co/meta-llama/Llama-3.2-1B/blob/main/LICENSE.txt) |
| Gemma 3 1B IT (GGUF via unsloth) | Google | [Gemma Terms of Use](https://ai.google.dev/gemma/terms) |
| EXAONE 3.5 2.4B Instruct (GGUF) | LG AI Research | [EXAONE AI Model License](https://huggingface.co/LGAI-EXAONE/EXAONE-3.5-2.4B-Instruct/blob/main/LICENSE) |
| SmolLM2 360M Instruct (GGUF) | HuggingFaceTB | [Apache-2.0](https://huggingface.co/HuggingFaceTB/SmolLM2-360M-Instruct/blob/main/LICENSE) |

일부 라이선스 (예: Llama 3.2 Community License, Gemma Terms of Use, EXAONE License, Qwen Research License) 는 **사용 제한 조항** (예: 허용 사용 범위, 월간 활성 사용자 한도, 정부 사용 제한 등) 을 포함합니다. 사용자가 다운로드한 모델의 라이선스 준수는 **사용자 본인의 책임** 입니다. 채아챗은 다운로드와 추론을 중개할 뿐, 모델 가중치를 재배포하지 않습니다.

## 검색 제공자 (선택 — 사용자가 검색을 활성화한 경우에만)

본 앱은 검색 제공자의 어떤 SDK 도 번들 / 재배포하지 않습니다. 각 제공자의 공개 HTTP API 를 호출만 합니다:

- Tavily, SerpAPI, Brave Search, Bing, Google Custom Search, DuckDuckGo

각 제공자의 자체 이용약관에 따라 사용자가 자신의 API 키 사용을 준수해야 합니다.

## 라이선스 준수 메모

- 위에 열거된 모든 OSS 의존성은 각 라이선스에 따라 동적 link 또는 source/binary 형태로 배포됩니다.
- Apache-2.0 / MIT / BSD-2 의 NOTICE 파일 (의무 사항) 은 각 라이브러리의 배포본에 포함되며, 위 링크로 확인할 수 있습니다.
- "카피레프트" 라이선스 (GPL / LGPL / AGPL) 의 코드는 일절 사용하지 않습니다.
- 라이선스 관련 takedown 요청, 문의, attribution 정정은 [github.com/thebible75/chaea-legal/issues](https://github.com/thebible75/chaea-legal/issues) 또는 <incoad@gmail.com> 으로 부탁드립니다.

---

**최종 수정:** 2026-05-24 — 채아챗 v1.0.2 (versionCode 102) 이후 적용.

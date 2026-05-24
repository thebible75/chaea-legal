---
title: Open Source Licenses — ChaeA Chat
permalink: /oss-licenses.html
---
**Language**: [English](oss-licenses.html) · [한국어](oss-licenses-ko.html)
**Documents**: [Guide](guide.html) · [Privacy](privacy.html) · [Terms](terms.html) · [Acceptable Use](policy.html) · [OSS Licenses](oss-licenses.html)

---

# Open Source Licenses

ChaeA Chat is built on top of many excellent open source projects. We are grateful to all the maintainers and contributors. This page lists every direct dependency together with its license.

Click each license name to read the full text on the upstream project's repository.

## On-Device Inference (native)

| Project | Author / Maintainer | License |
|---|---|---|
| [llama.cpp](https://github.com/ggerganov/llama.cpp) | Georgi Gerganov et al. | [MIT](https://github.com/ggerganov/llama.cpp/blob/master/LICENSE) |
| [GGML](https://github.com/ggerganov/ggml) | Georgi Gerganov et al. | [MIT](https://github.com/ggerganov/ggml/blob/master/LICENSE) |
| [KleidiAI](https://github.com/ARM-software/kleidiai) | Arm Limited | [Apache-2.0](https://github.com/ARM-software/kleidiai/blob/main/LICENSE) |
| [nlohmann/json](https://github.com/nlohmann/json) | Niels Lohmann | [MIT](https://github.com/nlohmann/json/blob/develop/LICENSE.MIT) |
| ai_chat.cpp (JNI adapter) | Han Yin (Arm) — included via llama.cpp `examples/llama.android` | [MIT](https://github.com/ggerganov/llama.cpp/blob/master/LICENSE) |

The native shared library `libai-chat.so` is built from the above sources via Android NDK r26.1 and the CMake configuration in this repository (no proprietary modifications). The Kotlin wrapper (`com.cayber.aichat.localllm.*`) is also adapted from `examples/llama.android/lib` under the same MIT license, with the package namespace rebranded.

## Compose / AndroidX

| Project | License |
|---|---|
| [Jetpack Compose (BOM 2024.x)](https://developer.android.com/jetpack/compose) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| AndroidX Core / Activity / Lifecycle / Navigation / DataStore / Room / AppCompat | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| Material Components for Android | [Apache-2.0](https://github.com/material-components/material-components-android/blob/master/LICENSE) |
| Material Icons Extended | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |

## Kotlin Standard Library / Coroutines / Serialization

| Project | License |
|---|---|
| [kotlin-stdlib](https://github.com/JetBrains/kotlin) | [Apache-2.0](https://github.com/JetBrains/kotlin/blob/master/license/LICENSE.txt) |
| [kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines) | [Apache-2.0](https://github.com/Kotlin/kotlinx.coroutines/blob/master/LICENSE.txt) |
| [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization) | [Apache-2.0](https://github.com/Kotlin/kotlinx.serialization/blob/master/LICENSE.txt) |

## Network / Storage

| Project | License |
|---|---|
| [OkHttp](https://github.com/square/okhttp) (incl. logging-interceptor) | [Apache-2.0](https://github.com/square/okhttp/blob/master/LICENSE.txt) |
| [Okio](https://github.com/square/okio) | [Apache-2.0](https://github.com/square/okio/blob/master/LICENSE.txt) |
| [AndroidX DataStore](https://developer.android.com/topic/libraries/architecture/datastore) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |
| [AndroidX Room](https://developer.android.com/training/data-storage/room) | [Apache-2.0](https://github.com/androidx/androidx/blob/androidx-main/LICENSE.txt) |

## Content Rendering / Parsing

| Project | License |
|---|---|
| [Coil](https://github.com/coil-kt/coil) (image loading) | [Apache-2.0](https://github.com/coil-kt/coil/blob/main/LICENSE.txt) |
| [halilibo Compose RichText](https://github.com/halilozercan/compose-richtext) | [Apache-2.0](https://github.com/halilozercan/compose-richtext/blob/main/LICENSE) |
| [Commonmark Java](https://github.com/commonmark/commonmark-java) | [BSD-2-Clause](https://github.com/commonmark/commonmark-java/blob/main/LICENSE.txt) |
| [Jsoup](https://github.com/jhy/jsoup) (HTML → text for fetch_url tool) | [MIT](https://github.com/jhy/jsoup/blob/master/LICENSE) |
| [PdfBox-Android](https://github.com/TomRoush/PdfBox-Android) (PDF text extract + create_file PDF) | [Apache-2.0](https://github.com/TomRoush/PdfBox-Android/blob/master/LICENSE) |
| [BouncyCastle](https://www.bouncycastle.org/) (PDF security, optional) | [MIT-style](https://www.bouncycastle.org/licence.html) |

## Build Tools (not shipped in APK, but worth acknowledging)

| Project | License |
|---|---|
| [Android Gradle Plugin](https://developer.android.com/build/releases) | [Apache-2.0](https://android.googlesource.com/platform/tools/base/+/refs/heads/mirror-goog-studio-main/LICENSE) |
| [Kotlin Symbol Processing (KSP)](https://github.com/google/ksp) | [Apache-2.0](https://github.com/google/ksp/blob/main/LICENSE) |
| [Gradle](https://gradle.org/) | [Apache-2.0](https://github.com/gradle/gradle/blob/master/LICENSE) |
| Android NDK r26.1 (used for `libai-chat.so` only) | Multiple OSS — see [NDK page](https://developer.android.com/ndk) |

## GGUF Models (user-downloaded — not bundled with the App)

Models listed in the On-Device catalog are downloaded directly from HuggingFace to the user's device on demand. Each model is governed by its own license:

| Model | Publisher | License |
|---|---|---|
| Qwen2.5 0.5B / 1.5B Instruct (GGUF) | Alibaba (Qwen) | [Apache-2.0](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct/blob/main/LICENSE) |
| Qwen2.5 3B Instruct (GGUF) | Alibaba (Qwen) | [Qwen Research License](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct/blob/main/LICENSE) |
| Llama 3.2 1B / 3B Instruct (GGUF) | Meta (via bartowski) | [Llama 3.2 Community License](https://huggingface.co/meta-llama/Llama-3.2-1B/blob/main/LICENSE.txt) |
| Gemma 3 1B IT (GGUF via unsloth) | Google | [Gemma Terms of Use](https://ai.google.dev/gemma/terms) |
| EXAONE 3.5 2.4B Instruct (GGUF) | LG AI Research | [EXAONE AI Model License](https://huggingface.co/LGAI-EXAONE/EXAONE-3.5-2.4B-Instruct/blob/main/LICENSE) |
| SmolLM2 360M Instruct (GGUF) | HuggingFaceTB | [Apache-2.0](https://huggingface.co/HuggingFaceTB/SmolLM2-360M-Instruct/blob/main/LICENSE) |

Some of these licenses (e.g. Llama 3.2 Community License, Gemma Terms of Use, EXAONE License, Qwen Research License) impose **usage restrictions** (e.g. acceptable-use clauses, MAU thresholds, government use). It is the **user's responsibility** to comply with the license of any model they download. ChaeA Chat only orchestrates the download and inference; it does not redistribute model weights.

## Search Providers (optional — only used when user enables search)

The App does not bundle or redistribute any provider SDK. It calls public HTTP APIs of:

- Tavily, SerpAPI, Brave Search, Bing, Google Custom Search, DuckDuckGo

Each provider has its own Terms of Service that the user must comply with when using their API keys.

## License Compliance Notes

- All OSS dependencies listed above are dynamically linked or shipped in source/binary form per their respective licenses.
- Apache-2.0 / MIT / BSD-2 NOTICE files (where mandated) are included with each library's distribution and accessible via the upstream links above.
- No code from any "copyleft" license (GPL / LGPL / AGPL) is used.
- For takedown requests, license questions, or attribution corrections, please open an issue at [github.com/thebible75/chaea-legal/issues](https://github.com/thebible75/chaea-legal/issues) or email <incoad@gmail.com>.

---

**Last updated:** 2026-05-24 — covers ChaeA Chat v1.0.2 (versionCode 102) and later.

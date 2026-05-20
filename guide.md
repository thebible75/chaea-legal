---
title: User Guide — ChaeA Chat
permalink: /guide.html
---
**Language**: [English](guide.html) · [한국어](guide-ko.html)
**Documents**: [Guide](guide.html) · [Privacy](privacy.html) · [Terms](terms.html) · [Acceptable Use](policy.html)

---

# ChaeA Chat — User Guide

> An Android chat client that connects directly to your own Ollama LLM servers.
> Your data never goes through external services — only to the server you specify.

---

## Table of Contents
1. [Before You Start — Preparing the Ollama Server](#1-before-you-start--preparing-the-ollama-server)
2. [First Launch — Onboarding Tour](#2-first-launch--onboarding-tour)
3. [Configuring Server Profiles](#3-configuring-server-profiles)
4. [Basic Chat Usage](#4-basic-chat-usage)
5. [Sending Attachments](#5-sending-attachments)
6. [Web Search (Always-search)](#6-web-search-always-search)
7. [Tool Calling](#7-tool-calling)
8. [Personas](#8-personas)
9. [Voice (STT · TTS)](#9-voice-stt--tts)
10. [Backup · Restore](#10-backup--restore)
11. [Managing Chat History](#11-managing-chat-history)
12. [App Settings — Theme · Language · Text Size](#12-app-settings--theme--language--text-size)
13. [Common Issues](#13-common-issues)
14. [FAQ](#14-faq)
15. [Privacy · Data](#15-privacy--data)

---

## 1. Before You Start — Preparing the Ollama Server

This app **does not provide an Ollama server**. You'll need the following ready first.

### 1-1. Install Ollama (server side)

On your own GPU machine or cloud instance:

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Or download the installer from https://ollama.com/download
```

### 1-2. Pull a model

On the server:

```bash
ollama pull llama3.2:3b           # lightweight
ollama pull qwen2.5:7b            # strong multilingual
ollama pull llama3.1:8b           # tool-calling capable
ollama pull deepseek-r1:7b        # reasoning-focused
```

### 1-3. Allow external access

By default Ollama only listens on `127.0.0.1:11434`. To accept connections from your phone:

```bash
# macOS / Linux
launchctl setenv OLLAMA_HOST "0.0.0.0:11434"
ollama serve

# systemd:
sudo systemctl edit ollama
# [Service]
# Environment="OLLAMA_HOST=0.0.0.0:11434"
sudo systemctl restart ollama
```

Verify:

```bash
curl http://<server-ip>:11434/api/tags
# → must return {"models":[...]}
```

### 1-4. Security (recommended for external exposure)

- **VPN / Tailscale**: Simplest and safest. Keep Ollama LAN-only and access from your phone over VPN.
- **Reverse proxy + auth** (`nginx` / `caddy` + Bearer token): The app supports Bearer / Basic authentication.
- **Port forwarding + firewall whitelist**: Not recommended (exposes to the public internet).

---

## 2. First Launch — Onboarding Tour

When you launch the app for the first time:

1. **Splash** (logo + spinner, ~0.4s)
2. **Onboarding pager** — swipe through 7 pages
   1. **Welcome** — what the app can do
   2. **Add a server profile** — Drawer ▸ ⚙️ Settings ▸ "Add profile" / 11 presets (Ollama · LM Studio · Jan · OpenRouter · Groq · vLLM, …)
   3. **Pick your model** — On the PC: `ollama pull qwen2.5:7b` → tap the model name in the app top bar
   4. **Personas for any role** — face icon next to the input bar → 10 built-ins + your own
   5. **Attachments & voice** — ＋ button (images · PDF · Office · HWP · ZIP · source code) + mic input + 🔊 chip
   6. **Web search & tools** — 6 providers + automatic tool calls (weather · time · currency · file generation · calendar …)
   7. **Make it yours & back it up** — light/dark · 6 languages · text size · JSON backup of profiles+personas
3. On the last page, tap **Get started** → enter the chat screen

> Onboarding shows once on first install. To see it again, clear app data:
> Android Settings → Apps → ChaeA Chat → Storage → Clear data.

> Right after first launch, the **10 built-in personas are auto-seeded** into the user_personas table in the background — they're ready to use immediately when you open the persona sheet ([§8](#8-personas)).

---

## 3. Configuring Server Profiles

A default profile is pre-registered (with an example URL), but you'll need to replace it with your own server.

### 3-1. Edit the profile (first time)

1. Tap the **⚙️ settings** icon (top-right) → profile list → tap the default profile
2. Or open the side drawer → **Server profiles** section → tap the profile → **Manage**

### 3-2. Fields

| Field | Description |
|-------|-------------|
| **Profile name** | For your reference (e.g. "Home server", "Office GPU") |
| **Server URL** | `http://192.168.0.10:11434` format — no trailing slash |
| **Authentication** | None / Bearer (Token) / Basic (ID + Password) |
| **Model** | Tap "Refresh" → pick from dropdown |
| **System Prompt** | Default behavior instructions for the model (optional) |
| **Temperature** | 0.0 (consistent) ~ 1.0+ (creative), default 0.7 |
| **Context length (num_ctx)** | Token budget per request (within model's own limit) |
| **Streaming** | Show tokens in real-time (recommended ON) |

### 3-3. Test connection

After entering the URL, tap **Test connection** → success shows the Ollama version.
If it fails → see [Section 13](#13-common-issues).

### 3-4. Multiple profiles

Register many profiles — home server / office GPU / cloud server — and:

- Switch instantly from drawer → **Server profiles** section
- Each profile can have a different model / system prompt / authentication

---

## 4. Basic Chat Usage

### 4-1. Sending messages

1. Type into the input bar → **Send** button (or Enter — depends on your keyboard)
2. With streaming ON, the model's tokens flow into the screen in real-time
3. Auto-scroll after the response completes

### 4-2. Markdown rendering

Responses render the following automatically:

- **Headings** (`#`, `##`, ...)
- **Lists** (ordered / unordered)
- **Tables** (`|` separators)
- **Inline / block code** (```)
- **Quotes** (`>`)
- **Links** (`[text](url)`)

> During streaming, content shows as plain text to avoid re-parsing every token.
> After the response completes, it switches to full Markdown rendering.

### 4-3. Link previews

URLs in responses are auto-expanded into Open Graph preview cards (title, description, thumbnail).
Disabled during streaming (avoids 404s on partial URLs).

### 4-4. Message actions

- **Long press** → copy
- **Retry button** (shown on failed assistant messages) → retry in place
- **Timestamp** beside each message

### 4-5. Background streaming

If you switch to another app while waiting, the stream keeps going.
A notification appears when the response completes — tap it to return to the chat.

> On Android 13+, the app asks for notification permission on first launch.
> Allow it for background notifications to work.

---

## 5. Sending Attachments

Tap the **+** button on the left of the input bar → pick files (multi-select OK).

### 5-1. Supported formats

| Category | Formats | How it's processed |
|----------|---------|---------------------|
| **Images** | JPG · PNG · WebP · GIF | Sent as base64 to vision models (e.g. `llava`, `qwen2.5-vl`, `gemma3`) |
| **PDF** | `.pdf` | Text auto-extracted and added to the prompt |
| **Office** | `.docx` · `.xlsx` · `.pptx` | Extracted via the bundled OOXML parser |
| **Hangul** | `.hwp` | Text extracted (within parser support) |
| **Archive** | `.zip` | File listing + body of small text files |
| **Text** | `.txt` · `.md` · `.json` · source code | Inlined into the prompt |
| **Video** | `.mp4` etc. | Metadata only (filename / size / duration) |

### 5-2. Attachment chips

Picked files appear as chips above the input bar (horizontal scroll):

- **Tap chip**: Opens the original in an external viewer (PDF reader etc.)
- **X on chip**: Removes the attachment

### 5-3. Tips

- Large PDFs may exceed context length — bump `num_ctx` (8192 recommended)
- Vision models are memory-heavy per image — limit to 1–2 at a time
- When the model recognizes the attachment, it cites it as `[attachment: filename.pdf]`

---

## 6. Web Search (Always-search)

When the model's built-in knowledge isn't enough (latest news / weather / prices), perform a web search before each message and feed the results to the model.

### 6-1. Enable

Settings → **Agent / Search** section →

1. **Mode**: pick `Always search`
2. **Search service**: pick from the table below
3. For paid services, enter the **API key**
4. **Result count**: 1 ~ 10 (default 5)
5. Tap **Test search connection** to verify

### 6-2. Supported services

| Service | Cost | API key | Notes |
|---------|------|---------|-------|
| **Tavily** | Free 1000/mo | ✅ | LLM-friendly summaries, most recommended |
| **Naver** | Free | ✅ | Korean search · news results (Client ID/Secret) |
| **Brave Search** | Free 2000/mo | ✅ | Privacy-focused |
| **Google CSE** | Free 100/day | ✅ | Google Custom Search Engine |
| **SerpAPI** | Paid | ✅ | Google · Bing · Naver and other engines |
| **Exa** | Free ~1000/mo | ✅ | Semantic search + neural ranking |

The settings screen has a direct **Get API key →** link for each service. (DuckDuckGo / Bing were removed in early 2026 — quality / policy changes; we consolidated to these 6 providers.)

### 6-3. Flow

```
User message → [extract query] → search API → N results → attach to model prompt
                                                                ↓
                                                        Model cites results in answer
```

A "🔍 Searching…" / "💭 Generating answer from N results…" indicator shows above the response,
with source links displayed at the end of the answer.

---

## 7. Tool Calling

Unlike `Always search`, the model **only calls tools when it needs to**.

### 7-1. Enable

Settings → Agent / Search → **Mode**: pick `Tool calling`.

### 7-2. Available tools

| Tool | Purpose | API kind |
|------|---------|----------|
| 🌐 **web_search** | Search the current web with the selected provider | All |
| 📄 **fetch_url** | Fetch a page's body for deeper reading | All |
| ⏰ **current_time** | Current time · timezone (IANA + abbreviation aware) | All |
| 🌤️ **weather** | Open-Meteo + Nominatim fallback (better Korean place name support) | All |
| 💱 **currency_convert** | Frankfurter API exchange rates | All |
| 📅 **add_calendar_event** | Add an event via Android Calendar Intent | All |
| 📱 **device_info** | Battery · network · system info | All |
| 📝 **create_file** | Generate text / pdf / docx / xlsx file as a chat attachment | All |
| 📋 **ollama_list_models** | List ALL installed models on the Ollama server | **Ollama only** |
| 🩺 **server_health_check** | Ollama server status / version | **Ollama only** |

Each tool can be toggled individually. On **OpenAI-compatible profiles** (LM Studio · Jan · OpenRouter · Groq · vLLM, …) the last two Ollama-specific tools are auto-hidden via `Tool.requiresApiKind`.

> Single-model identity questions like "which model am I using?" do **not** trigger `ollama_list_models` — they're answered directly from the `Current selected model` line in the system context (added 2026-05).

### 7-3. Recommended models

Models that handle tool calling well:

- `llama3.1:8b` or `llama3.2:3b`
- `qwen2.5:7b` or `qwen2.5:14b`
- `mistral-nemo:12b`
- `command-r:35b`

> If you pick a non-tool-calling model (e.g. `gemma2`), a warning appears.

### 7-4. Example flow

```
User: "What's the weather in Seoul today?"
  ↓
Model: [tool call] web_search("Seoul weather today")
  ↓ system runs the search
Model: [tool result] → generate answer
  ↓
"Seoul is sunny and 22°C today…"
```

### 7-5. Tool calling + streaming

Ollama 0.3+ and most OpenAI-compatible servers **include `tool_calls` in the final streaming chunk**,
so the app keeps `stream=true` and processes tool calls inline. After the tool runs, the model is
called again → the final answer streams as well.

> The tool-call loop is capped at `MAX_TOOL_ITERATIONS=3` (and breaks immediately on a repeated call signature).

---

## 8. Personas

**Persona = a pre-defined system prompt bundle.** It swaps the model's role, tone, temperature, allowed tools, and greeting in one click.

### 8-1. Open the persona sheet

- Automatically when starting a **new chat**, or
- Tap the **face icon** to the left of the input bar

### 8-2. Unified model (since 2026-05)

Built-ins and user-defined personas (UDPs) live in a single section "All personas". On first launch the 10 built-ins are auto-seeded into the `user_personas` table, so **all of them are freely editable and deletable**.

Built-in 10:

| Emoji · id | Role |
|-----------|------|
| 💬 general | General assistant |
| 🌐 translator | Translator |
| 👨‍💻 coder | Senior SW engineer |
| ✍️ writer | Writing coach |
| 📚 tutor | Patient tutor |
| 🎯 interviewer | Technical interview coach |
| 📧 email | Email drafting assistant |
| 📝 summarizer | Summarization assistant |
| 🎨 creative | Creative writing partner |
| 💡 brainstorm | Brainstorming partner |

### 8-3. Persona actions

- **Tap** → select (updates the current session's `personaId`)
- **✏️ Edit** → open the edit dialog (name · emoji · system prompt · temperature · top-P · tool whitelist · greeting)
- **Delete** → "Delete" button inside the edit dialog
- **↻ Restore** (header) → re-seed the 10 built-ins in the current locale (overwrites edited copies)
- **＋ Add** (header) → create a new persona

### 8-4. What the persona overrides

When you send a message, the selected persona transiently overrides `ServerProfile`:

| Field | Behavior |
|-------|----------|
| `systemPrompt` | Fully replaced with the persona's systemPrompt |
| `temperature` | Overrides if set (otherwise the profile value stays) |
| `topP` | Overrides if set (otherwise the model default) |
| `enabledTools` | Empty/null → profile as-is. Whitelist → **intersect** with profile's enabled tools |

Other fields (`selectedModel` · `baseUrl` · `numCtx` …) are untouched — a persona is orthogonal to the model/server choice.

### 8-5. Greeting

If a persona has a greeting and the **session is empty** (zero messages), picking that persona inserts the greeting once as an assistant message. No LLM call — purely static, zero tokens.

Switching personas mid-conversation does **not** trigger the greeting (preserves chat flow).

### 8-6. Identity guard

When a persona is active and the user asks "who are you?", the model answers in the persona's role **without revealing the base model name** (Qwen · Llama · GPT, …). For explicit model questions like "what model are you?", it answers with the `Current selected model` line — and does **not** call `ollama_list_models`.

---

## 9. Voice (STT · TTS)

### 9-1. Voice input (STT)

Tap the **mic toggle** in the send-button slot to start speech recognition. The recognized text fills the input field; if "Auto-send" is on, it's sent immediately.

- Uses the system recognizer (Android built-in or the user's default)
- `EXTRA_PREFER_OFFLINE` is **not** used on Android 13+ (avoids a TTSRecognitionService conflict)
- Messages sent via voice get `fromVoice=true` — used to gate auto-read of the reply

### 9-2. Read replies aloud (TTS)

Tap the **🔊 chip** below any assistant reply → the system TTS reads it. Tap again = stop.

- **Auto-read** — toggle in settings: reads the reply automatically when it finishes (only for replies to voice messages, or for all)
- **Multi-locale segmenter** — splits mixed-language text per language for natural pronunciation
- **Whitelist sanitize** — strips markdown / emoji symbols before speaking

### 9-3. Permission

Voice input needs the `RECORD_AUDIO` permission. Requested on first mic tap → if denied, a dialog with a deep-link to system settings.

---

## 10. Backup · Restore

Export profiles + personas as a single JSON file and re-import them on another device.

### 10-1. Export

⚙️ **App settings** ▸ "Backup profiles" → warning dialog (plain-text sensitive data notice) → SAF picker (`CreateDocument`) → save as `.json`.

### 10-2. Import

⚙️ **App settings** ▸ "Restore from backup" → SAF (`OpenDocument`) → pick the JSON → result dialog (added profiles · personas count).

**Always "add" mode**:
- All ids are re-issued as new UUIDs (never overwrites existing data)
- Name collisions get an `(imported)` suffix
- Active-profile metadata is preserved, but the new id means you may need to re-activate

### 10-3. What's in the backup

| Item | Included |
|------|----------|
| ServerProfile (URL · model · system prompt · search key · auth · temperature · topP) | ✅ |
| UserPersona / seeded built-in personas (name · emoji · system prompt · temperature · topP · tool whitelist · greeting) | ✅ |
| Active profile id | ✅ |
| **Chat history** | ❌ — privacy + file size |
| App settings (theme · language · text size) | ❌ |

### 10-4. Plain-text sensitive data

The backup file contains the following **in plain text**:

- `bearerToken` (server auth)
- `basicPassword` (Basic auth)
- `search.apiKey` (search provider API key)

The pre-export warning dialog asks for explicit confirmation. If the file leaks (cloud / email / social), those keys leak too.

### 10-5. Schema versions

`schemaVersion=2` (includes UserPersona's temperature/topP/enabledTools/greeting + ServerProfile.topP). Older v1 files import fine — new fields default to null. Future schema bumps will branch in the importer for backward compatibility.

---

## 11. Managing Chat History

### 8-1. Side drawer

Swipe from the left edge or tap the top-left menu icon to open the drawer.

- **Server profiles section** — switch profiles
- **Chat history section** — all sessions, newest first

### 8-2. New chat vs. existing chat

- **New chat button** — starts an empty session, doesn't erase existing ones
- **Tap a session** in the list — reopen that conversation
- **Date dividers** — `Tuesday, May 6, 2026` style auto-inserted between messages

### 8-3. Delete sessions

Long-press a session (or use the icon) → delete.
**Deleting a server profile does NOT touch chat history** — conversations stay even after the profile is gone.

---

## 12. App Settings — Theme · Language · Text Size

Settings → **App settings** section.

### 9-1. Theme

- **System** — follow Android's dark/light mode
- **Light** — always light (KakaoTalk light tones)
- **Dark** — always dark

### 9-2. Language

15 locales registered (system language used by default). Currently full translations for 6:

- Korean
- English
- Japanese
- Simplified Chinese
- Spanish
- Russian

Other locales (German / French / Portuguese / Vietnamese / Indonesian / Thai / Arabic / Hindi)
fall back to English when selected.

> Language changes apply instantly (no restart needed).

### 9-3. Chat text size

Text scale (0.7x ~ 1.6x) applied **only to chat message body** —
input bar, settings, drawer, etc. follow the system text size.

Live preview shows the effect immediately.

---

## 13. Common Issues

### 10-1. "Connection failed"

Checklist:

- [ ] URL format: includes `http://` or `https://`, no trailing slash
- [ ] Port number (`:11434`) included
- [ ] Phone and server on the same network (or VPN connected)
- [ ] Server is running with `OLLAMA_HOST=0.0.0.0:11434`
- [ ] Server-side firewall allows port 11434
- [ ] For HTTP (not HTTPS) URLs, `usesCleartextTraffic` is enabled (the app already permits this)

Verify:

```bash
# From a PC on the same network as your phone
curl http://<server-ip>:11434/api/tags
```

### 10-2. "Model list is empty"

The server has no models pulled:

```bash
ollama pull llama3.2:3b
ollama list           # confirm
```

### 10-3. "No response coming"

- The first token can take time, especially for big models (30s ~ 1min)
- Out of GPU memory → switch to a smaller model (`llama3.2:1b`, `qwen2.5:3b`)
- Concurrent requests get queued → wait briefly

### 10-4. "Streaming doesn't work"

- In tool-calling mode with active tool calls, streaming is forced OFF (by spec)
- Streaming may be turned off in the profile settings — check

### 10-5. "Background notifications don't show"

- Verify Android 13+ notification permission granted
- Android Settings → Apps → ChaeA Chat → Notifications → Allow
- Some OEMs (Samsung, Xiaomi) block backgrounded apps under battery optimization → add an exception

### 10-6. "Korean / multilingual auto-detection isn't working"

- Verify the model handles your language well — `qwen2.5`, `llama3.1`, `eeve-korean`, `solar` are good for Korean
- Adding "Reply in Korean" (or your target language) to the system prompt makes it more reliable

### 10-7. "PDF body isn't reaching the model"

- Some PDFs (scanned images) need OCR — currently not supported
- Password-protected PDFs fail
- Very large PDFs may exceed context length → increase `num_ctx`

---

## 14. FAQ

**Q. Where does my data go?**
A. Only to the Ollama server URL you configured. The app developer / external services are not involved.
However, when web search is enabled, the search query (only) goes to the search provider you picked.

**Q. Which models can I use?**
A. Anything Ollama supports — 100+ models (llama, qwen, mistral, gemma, phi, deepseek, solar, command-r, ...).
Full list: [ollama.com/library](https://ollama.com/library).

**Q. Can I use it offline?**
A. If the Ollama server is on your LAN, yes (no internet required). External servers need internet.
Search and tool-calling features always need internet.

**Q. Does it cost anything?**
A. The app is free. Ollama is open-source and free. Some search APIs are paid (DuckDuckGo is free).

**Q. Is there an iOS version?**
A. Currently Android only.

**Q. Can I move chat history to another device?**
A. **Profiles + personas** can be exported/imported as a JSON file ([§10](#10-backup--restore)). Chat bodies themselves are not in the export (privacy + file size). Within the same Android device, Google account auto-backup preserves some settings.

**Q. Voice / TTS support?**
A. Both are supported ([§9](#9-voice-stt--tts)). Mic toggle next to the input bar for STT, 🔊 chip below assistant replies for TTS. Auto-read toggle also available.

**Q. Where do I configure personas?**
A. Face icon next to the input bar → persona sheet ([§8](#8-personas)). The 10 built-ins are auto-seeded and all of them are freely editable / deletable / restorable.

**Q. Code execution / Python interpreter tool?**
A. Not currently. Ollama itself doesn't run code, so this would need a separate tool integration —
held back due to security concerns.

---

## 15. Privacy · Data

### 12-1. What this app collects

**Nothing.** Chat messages, attachments, profiles, and settings are all stored on-device only
(Room DB + DataStore). The only outbound network traffic is to the Ollama server URL you
explicitly configured + the search provider query when search is enabled.

### 12-2. Permissions

| Permission | Reason |
|------------|--------|
| `INTERNET` | HTTP communication with the Ollama server + web search |
| `ACCESS_NETWORK_STATE` | Detect connectivity for accurate error messages |
| `POST_NOTIFICATIONS` | Notify when a backgrounded streaming response completes |
| `RECORD_AUDIO` | Voice input (STT) — requested on first mic tap |
| `FOREGROUND_SERVICE` + `FOREGROUND_SERVICE_DATA_SYNC` | Keep response streaming after the app goes to the background |

### 12-3. Deleting data

- **Delete a session** — long-press in the drawer
- **Delete everything** — Android Settings → Apps → ChaeA Chat → Storage → Clear data
  (resets all chats + profiles + settings; onboarding shows again on next launch)

---

## Appendix — Quick Reference

| What you want | Where |
|---------------|-------|
| Start a new chat | Drawer → "New chat" or top-right ✏️ |
| Switch to a different server | Drawer → Server profiles section |
| Change the model | Top-right ⚙️ → edit profile → Model |
| Enable search | ⚙️ → Agent / Search → Mode |
| Change theme / language | ⚙️ → App settings |
| Add an attachment | + button on the left of the input bar |
| Background notifications | Automatic — just allow the permission |

---

## Help / Feedback

- Bug reports / feature requests: GitHub Issues (or the channel announced after publication)
- Security issues: separate private channel

If something here is unclear or incorrect, please let me know.

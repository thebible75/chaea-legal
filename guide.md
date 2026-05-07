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
8. [Managing Chat History](#8-managing-chat-history)
9. [App Settings — Theme · Language · Text Size](#9-app-settings--theme--language--text-size)
10. [Common Issues](#10-common-issues)
11. [FAQ](#11-faq)
12. [Privacy · Data](#12-privacy--data)

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
2. **Onboarding pager** — swipe horizontally through 5 pages
   - Welcome → Server profiles → Streaming/attachments → Agent (search/tools) → Customization
3. On the last page, tap **Get started** → enter the chat screen

> Onboarding shows once on first install. To see it again, clear app data:
> Android Settings → Apps → ChaeA Chat → Storage → Clear data.

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
If it fails → see [Section 10](#10-common-issues).

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
| **DuckDuckGo** | Free | ❌ | Works out of the box, mediocre quality |
| **Tavily** | Free 1000/mo | ✅ | LLM-friendly summaries, recommended |
| **Brave Search** | Free 2000/mo | ✅ | Privacy-focused |
| **SerpAPI** | Paid | ✅ | Google search results |
| **Bing Search** | Paid | ✅ | Microsoft Bing |
| **Google CSE** | Free 100/day | ✅ | Google Custom Search |

The settings screen has a direct **Get API key →** link for each service.

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

| Tool | Purpose |
|------|---------|
| 🌐 **web_search** | Model searches the web for current data |
| 📄 **fetch_url** | Read a page's full body |
| 📋 **list_models** | Query available models on the server |
| 🩺 **health_check** | Check server status / latency |

Each tool can be toggled individually.

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

In tool-calling mode, the model has to decide tool calls atomically, so **`stream=false`** is forced
(regardless of profile setting). After tool execution, the final answer streams normally.

---

## 8. Managing Chat History

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

## 9. App Settings — Theme · Language · Text Size

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

## 10. Common Issues

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

## 11. FAQ

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
A. Export/import not yet supported (on the roadmap). Android backups (Google Account) preserve some
preferences but not chat content.

**Q. Voice / TTS support?**
A. Not currently.

**Q. Code execution / Python interpreter tool?**
A. Not currently. Ollama itself doesn't run code, so this would need a separate tool integration —
held back due to security concerns.

---

## 12. Privacy · Data

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

---
title: Privacy Policy — ChaeA Chat
permalink: /privacy.html
---
**Language**: [English](privacy.html) · [한국어](privacy-ko.html)
**Documents**: [Guide](guide.html) · [Privacy](privacy.html) · [Terms](terms.html) · [Acceptable Use](policy.html)

---

# Privacy Policy

This Policy explains how ChaeA Chat handles user data.

> **Summary**: This App does not run a backend server. No user data is transmitted to or collected by the App developer. All data stays on your device or is sent only to the Ollama server you registered (and to the search provider you optionally selected).

## 1. Information We Collect
**None.** The App does not collect any of the following:
- Personal identifiers (name, email, phone number, etc.)
- Device identifiers (Advertising ID, IMEI, etc.)
- Location information
- Usage statistics / analytics
- Chat content / attachments

## 2. Permissions
| Permission | Purpose |
|------------|---------|
| `INTERNET` | HTTP(S) communication with the Ollama / OpenAI-compatible server you registered, plus optional web search / fetch_url tools |
| `ACCESS_NETWORK_STATE` | Detect connectivity for accurate error messages |
| `POST_NOTIFICATIONS` (Android 13+) | Show a notification when a background streaming response completes |
| `RECORD_AUDIO` | Active only when you tap the microphone button. Voice input is processed by the Android system SpeechRecognizer (see §3-4) |
| `FOREGROUND_SERVICE` / `FOREGROUND_SERVICE_DATA_SYNC` | Keep the network connection alive while the app may be backgrounded, so that the streamed LLM response is fully synced into the local DB. Stopped immediately on completion / cancellation |

## 3. Outbound Data
The App transmits data only in the following cases. Each destination is set explicitly by the user.

### 3-1. Ollama server (required)
- Chat messages / attachment content are sent to the Ollama server URL you registered.
- How that server processes the data is governed by the operator's policy.
- You are responsible for your own server's operation and security.

### 3-2. Web search provider (only if you enable search)
- The query string is sent to one of Tavily / SerpAPI / Brave / Bing / Google CSE / DuckDuckGo (whichever you picked).
- The App does not transmit any personal identifier to the search provider (it doesn't have any to send).

### 3-3. External viewer apps (when opening attachments)
- Tapping an attachment chip exposes the file via Android FileProvider as a temporary read-only URI to the external app you choose (e.g. PDF reader).
- Permissions are revoked when that external app exits.

### 3-4. Voice input (only when you tap the microphone)
- Your voice is processed by the **Android system SpeechRecognizer**, which the OS provides.
- Depending on your device / OS settings, the system SpeechRecognizer may run **on-device** or in **Google's cloud (Google Speech)**. The App does not control this choice — it follows your OS configuration.
- The App developer does not directly receive, store, or transmit your audio. The App only receives the recognized text result and shows it on screen; that text is sent to the LLM server only when you press send.
- If you do not want voice input, simply don't tap the microphone, or deny RECORD_AUDIO permission.

### 3-5. On-Device GGUF model downloads (only when you enable it)
- Active only when you create a "📱 On-Device (GGUF)" profile and pick a model.
- Only the following two HTTPS requests are made:
  - **Catalog fetch**: the App downloads the model list JSON from ChaeA's own static host (`thebible75.github.io/chaea-legal/local_models.json`). No user identifier is sent.
  - **Model file download**: a public HuggingFace direct link (`https://huggingface.co/.../resolve/main/*.gguf`) listed in the catalog. Only the model you explicitly tap is downloaded.
- The App attaches no user identifier (account, email, device ID, etc.) to HuggingFace requests — a plain HTTP GET.
- HuggingFace's own data handling is governed by [huggingface.co/privacy](https://huggingface.co/privacy).
- Downloaded model files are stored inside the app's private storage (`filesDir/models/`) and removed when the app is uninstalled.
- **Inference (chat) itself runs 100% on your phone.** Your chat messages are not transmitted off the device.
- If you do not want any model download, simply don't create an "On-Device" profile.

## 4. Local Data Storage
All data lives on your device only.

| Data | Location |
|------|----------|
| Chat history | Room (SQLite) — app-private database |
| Server profiles | Room (SQLite) |
| App settings (theme / language / text size) | DataStore Preferences |
| Attachment originals | App internal storage (`filesDir/attachments/`) |
| Cache (images, etc.) | App cache (`cacheDir/`) |

Other apps cannot access this data, and it is not included in Android system backups (Google Backup).

## 5. Right to Delete
You can delete data anytime:
- **Delete a session**: long-press a session in the drawer → delete
- **Delete everything**: Android Settings → Apps → ChaeA Chat → Storage → Clear data
- **Uninstall**: removing the app deletes all local data

## 6. Children's Privacy
The App is not directed at children under 13, and we do not knowingly collect data from children.

## 7. Third-Party Sharing
The App does not share any user data with third parties (since it does not collect any user data, there is nothing to share).

## 8. Security
- HTTPS Ollama URLs are encrypted in transit.
- HTTP (plaintext) URLs are recommended only for LAN / VPN scenarios.
- Authentication credentials (Bearer Token / Basic credentials) you enter are stored on-device only.

## 9. Device Information / Auto Context
To improve answer accuracy, the App automatically prepends the following non-sensitive context to the system prompt at the time of each chat:
- **Current date / time / timezone** — so "today" / "now" expressions resolve correctly
- **User language** (system locale)
- **User country** (SIM / mobile carrier country code, or system locale country)

Additionally, only when you explicitly ask about device status (e.g. "what's my battery level", "how much storage do I have left"), the following data is read locally on the device and included in the model response (transmitted only to the LLM server you registered, and only at your explicit request):
- Battery level / charging state / temperature, storage and memory usage
- Device specs (manufacturer, model, Android version), screen size/orientation, network type, screen-off timeout, count of apps installed on your home screen

## 10. What We Do NOT Collect
The App displays no advertising and **never collects, requests, or transmits** any of:
- Advertising ID (ADID), Android ID
- Phone number, contacts, call log, SMS
- Precise location (GPS) or approximate location
- Camera, gallery (image attachments are picked by the user via the system picker — no permission required)
- Microphone (RECORD_AUDIO is active only the moment you press the mic button)
- Crash logs, analytics data, usage statistics

## 11. Reporting Inappropriate Responses
If an LLM model produces an inappropriate response, you can report it:
- In-app: long-press the response message and select "Report"
- Email: incoad@gmail.com

We review reports and may update the system prompt / persona / tool set accordingly. Note: the App does not host or generate the LLM responses themselves — those come from the external model you chose — so we cannot accept direct liability for response accuracy or appropriateness.

## 12. Policy Changes
This Policy may be updated alongside app updates and reflected on this screen.
The effective date is updated whenever the policy changes.

## 13. Contact
For privacy-related questions:
- incoad@gmail.com

---

**Effective Date**: 2026-05-08

# DocVoice — Multilingual Voice Assistant for PDF Manuals

A lightweight, zero-setup client-side voice assistant designed for technical equipment manuals. Built with Vanilla JavaScript, Tailwind CSS, PDF.js, and Google Gemini 3.1 Flash-Lite. Hosted statically via GitHub Pages.

---

## Live Demo & Repository
* **Live Demo:** [https://dyselnyyslavyanee.github.io/voice-manual-qa/](https://dyselnyyslavyanee.github.io/voice-manual-qa/)
* **Source Code:** [https://github.com/dyselnyyslavyanee/voice-manual-qa](https://github.com/dyselnyyslavyanee/voice-manual-qa)

---

## Quick Start

1. Open the [Live Demo](https://dyselnyyslavyanee.github.io/voice-manual-qa/).
2. Enter your **Gemini API Key** in the top navigation bar and click **Save** (stored locally in browser `localStorage`).
3. Upload a text-based equipment manual (PDF up to 10 pages) via the Drag & Drop area.
4. Click the **Microphone** button to ask a question aloud, or type your query into the input field.
5. Receive a spoken response alongside visible verbatim quotations and page references.
6. Use the **Voice ON/OFF** button in the header to toggle text-to-speech output anytime.

---

## Architecture & Technical Decisions

* **Client-Side Only (Zero Backend):** Parsing, state management, and API calls are handled directly in the browser. Eliminates container overhead, dependency rot, and cloud infrastructure maintenance.
* **Low-Latency Inference:** Powered by `gemini-3.1-flash-lite` configured with `thinkingBudget: 0` and `temperature: 0.0` for sub-second deterministic responses.
* **Strict Grounding:** Prompts enforce JSON-only output with required verbatim quotations and page numbers. The model explicitly declines when information is missing from the uploaded context.
* **Web Speech Integration:** Combines native browser `SpeechRecognition` (STT) and `SpeechSynthesis` (TTS) with a sequential execution pipeline and instant mute control.

---

## Delivery Notes & Evaluation

### 1. Project Overview & Effort
* **Total Development Time:** ~2.5–3 hours.
* **AI Tools & Workflow:** 
  * Google Gemini API (`gemini-3.1-flash-lite`).
  * AI-assisted interface scaffolding and prompt optimization.
* **Verification Strategy:** Tested against synthesized edge cases with deterministic zero temperature. Quotes were cross-referenced against the raw PDF text layers to ensure exact string matching.

### 2. Measured Performance
* **Ingestion Time (2 pages PDF):** 48 ms – 75 ms (in-browser PDF.js text extraction).
* **Audible Response Time (Latency):** 620 ms – 940 ms (from query submission to output rendering).

### 3. Unit Economics & Cost Estimation
* **Ingestion Cost:** **$0.00** (processed entirely on client hardware via PDF.js).
* **Cost Per Query:** **~$0.0000825** (~$0.082 per 1,000 queries) based on commercial Google Gemini 3.1 Flash-Lite pricing:
  * Input tokens (~700 tokens): $0.075 / 1M tokens = $0.0000525
  * Output tokens (~100 tokens): $0.30 / 1M tokens = $0.0000300
* **Speech Services (STT / TTS):** **$0.00** (native browser Web Speech API).
* **Hosting:** **$0.00** (GitHub Pages static hosting).

---

## Test Set Matrix (AeroPulse Manual v1 / v2)

Evaluation performed using a fictional two-model manual (*AeroPulse Core-100* vs *AeroPulse Pro-500*):

| # | Question Type | Input Query | Expected Output | Actual Prototype Output | Quote & Page Verified? |
|---|---|---|---|---|---|
| 1 | **Direct Fact** | "What is the maximum continuous run limit for the AeroPulse Core-100?" | 12 hours. | 12 hours. | **Pass** (Page 2) |
| 2 | **Comparison** | "How do the setup procedures differ between Core-100 and Pro-500?" | Core-100 requires manual dial calibration; Pro-500 uses automated Auto-Tune button. | Core-100 uses analog rotary dial; Pro-500 uses digital Auto-Tune button for 3s. | **Pass** (Page 1) |
| 3 | **Follow-up** | "And what is its maximum airflow?" | 3,400 m³/h (for Pro-500). | 3,400 m³/h. | **Pass** (Page 2) |
| 4 | **Exception** | "Can the system run below 0°C without the pre-heater?" | Only Pro-500 down to -15°C if ISO-VG-32 oil is installed. | Only Pro-500 with ISO-VG-32 synthetic oil down to -15°C. | **Pass** (Page 2) |
| 5 | **Absent Fact** | "What is the recommended cleaning interval for the HEPA filter?" | Document does not contain this information. | Declines to answer: not specified in document. | **Pass** (No quotation generated) |
| 6 | **Replacement** | "What is the operating pressure for the Core-100?" *(tested after loading v2)* | 3.2 bar (updated from 2.5 bar). | 3.2 bar. | **Pass** (Page 2) |

* **Factual Accuracy:** 100% (6/6).
* **Citation Accuracy:** 100% (5/5 relevant citations matched document strings; absent fact correctly declined to cite).

---

## Known Limitations & Trade-offs

* **Web Speech API Environment Dependency:** Browser speech synthesis relies on locally installed operating system voice engines. On specific Chromium configurations on Windows, certain language packs may fall back to default voices. An immediate **Voice OFF** toggle was added to give the user complete audio control.
* **Context Bounds:** Designed for quick-start guides and equipment manuals up to 10 pages. Scaling to 100+ page documents would require client-side chunking with vector indexing (e.g., SQLite Wasm / embedded embeddings).

---

## Product Next Steps

1. **Streaming Audio Engine:** Transition to server-sent audio streaming via WebSocket to begin spoken output concurrently with token generation (<400 ms).
2. **Visual In-PDF Highlighting:** Leverage PDF.js canvas bounding boxes to highlight cited passages directly on the original document layout.
3. **Session Conversation Memory:** Expand multi-turn context retention for extended interactive troubleshooting trees.

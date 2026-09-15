# voice-manual-qa
High-performance voice-first web assistant for PDF manuals powered by Gemini 3.1 Flash-Lite, Web Speech API, and PDF.js.
# DocVoice — Multilingual Voice Assistant for PDF Manuals

DocVoice is a lightweight, low-latency web application designed to analyze text-based PDF documents and provide instant, voice-enabled answers with verifiable source citations.

## Key Features

* **Sub-2s Response Latency:** Optimized inference pipeline utilizing `gemini-3.1-flash-lite` with zero thinking overhead (`thinkingBudget: 0`) and strict JSON output schemas.
* **Voice-In & Voice-Out (VIVO):** Hands-free speech recognition and multi-language text-to-speech (TTS) powered by the Web Speech API.
* **Multilingual Routing:** Dynamic voice and accent selection based on model-detected language codes (`uk-UA`, `en-US`, `ru-RU`), eliminating pronunciation artifacts across Cyrillic and Latin alphabets.
* **Grounded Citations:** Every answer returns a verbatim text quote along with the exact source page number to eliminate hallucinations.
* **Client-Side Document Ingestion:** Direct text extraction in the browser via PDF.js with real-time indexing metrics.
* **Zero-Setup Deployment:** Pure client-side architecture requiring no backend server; runs directly via GitHub Pages or static hosting over HTTPS.

## Tech Stack

* **AI Inference:** Google Gemini API (`gemini-3.1-flash-lite`)
* **Frontend:** Vanilla JavaScript (ES6+), HTML5, Tailwind CSS
* **Speech Processing:** Web Speech Recognition & SpeechSynthesis APIs
* **Document Processing:** PDF.js

## Quick Start

1. Open the [Live Demo](https://<dyselnyyslavyanee>.github.io/voice-manual-qa/).
2. Enter your Gemini API Key in the top bar.
3. Drag & drop any text-based PDF document (up to 10 pages).
4. Click the microphone button or type your question to receive spoken, cited responses.

<div align="center">

<img src="https://img.shields.io/badge/DocMind-Document%20Intelligence-6C63FF?style=for-the-badge&logoColor=white" alt="DocMind"/>

# DocMind
### Enterprise Document Intelligence Platform

**Retrieval-Augmented Generation · Natural Language Q&A · Precision Citation Engine**

[![Live Demo](https://docmind6.netlify.app)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-22c55e?style=for-the-badge)](https://docmind6.netlify.app)
[![License](https://img.shields.io/badge/License-MIT-0A0F1E?style=for-the-badge)](LICENSE)

---

*Transform your document library into an intelligent knowledge engine.*
*Ask anything. Get precise, cited answers in seconds.*

</div>

---

## Table of Contents

- [Overview](#overview)
- [Core Capabilities](#core-capabilities)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Deployment](#deployment)
- [Demo Mode](#demo-mode)
- [Use Cases](#use-cases)
- [Security & Compliance](#security--compliance)
- [Roadmap](#roadmap)
- [License](#license)

---

## Overview

**DocMind** is a business-grade, AI-powered document Q&A platform built on the **Retrieval-Augmented Generation (RAG)** paradigm. It enables enterprises and professionals to extract precise, citation-backed answers from their document libraries — without hallucination, without guesswork, and without exposing sensitive data to public AI models.

Unlike generic AI assistants, DocMind grounds every response exclusively in the user's own uploaded content, with page-level citations verifiable at a glance. The system processes PDFs, Word documents, plain text, and HTML files — surfacing the exact information your team needs, when they need it.

> **"Your documents contain the answers. DocMind finds them."**

---

## Core Capabilities

### 🔐 Authentication & Access Control
- Secure email/password registration with real-time password strength enforcement
- Session management via encrypted browser storage
- Multi-account support with persistent credentials across sessions
- Forgot password and account recovery flows built in

### 📄 Intelligent Document Processing
- **Multi-format ingestion** — PDF (text-based), DOCX, TXT, HTML up to 50 MB per file
- **Client-side PDF parsing** via PDF.js — page-aware, preserves document structure
- **Sentence-aware chunking** — overlapping 600-character segments prevent context fragmentation
- **Real-time indexing status** — live progress indicators per document

### 🔍 Precision Retrieval Engine
- **BM25-style relevance scoring** — keyword frequency, phrase bonuses, and diversity ranking
- **Maximal Marginal Relevance** — eliminates redundant chunk retrieval
- **Cross-document search** — query across an entire collection simultaneously
- **Top-8 chunk retrieval** with deduplication and source attribution

### 🤖 AI-Powered Answer Generation
- Powered by **Anthropic Claude** — answers generated exclusively from retrieved context
- **Strict grounding** — model instructed never to use outside knowledge
- **Page-level citations** — every answer references the source document and section
- **Cloudflare Worker proxy** — eliminates browser CORS restrictions securely

### 📊 Full-Featured Dashboard
| Section | Functionality |
|---|---|
| **Dashboard** | Live stats, document upload, Q&A chat panel, activity feed |
| **Documents** | Full document library management with status tracking |
| **Q&A History** | Complete query log with timestamps and response times |
| **Collections** | Organise documents into topic-based groups |
| **Members** | Team workspace access and invite management |
| **Analytics** | Query volume, response times, usage metrics |
| **API & Integrations** | API key generation, REST endpoint reference |
| **Profile** | Personal information and account management |
| **Settings** | API key configuration, AI preferences, Demo Mode |

### 🎤 Demo Mode
Answer questions locally from real retrieved document content — **zero API cost, zero network dependency**. Purpose-built for presentations, interviews, and demonstrations.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT BROWSER                        │
│                                                             │
│  ┌──────────────┐    ┌─────────────┐    ┌───────────────┐  │
│  │   PDF.js     │    │   Chunker   │    │   Retrieval   │  │
│  │  (Parser)    │───▶│  (Splitter) │───▶│   Engine      │  │
│  └──────────────┘    └─────────────┘    └───────┬───────┘  │
│                                                 │           │
│                                          Top-8 Chunks       │
│                                                 │           │
└─────────────────────────────────────────────────┼───────────┘
                                                  │
                                                  ▼
                                   ┌──────────────────────────┐
                                   │   Cloudflare Worker      │
                                   │   (Secure API Proxy)     │
                                   │   CORS · Rate Limiting   │
                                   └──────────────┬───────────┘
                                                  │
                                                  ▼
                                   ┌──────────────────────────┐
                                   │   Anthropic Claude API   │
                                   │   claude-sonnet-4-6      │
                                   │   Grounded Generation    │
                                   └──────────────────────────┘
```

**Key architectural decisions:**
- **Single-file deployment** — entire frontend in one self-contained `index.html`, no build pipeline required
- **Client-side RAG** — chunking and retrieval run entirely in the browser; no document data leaves the client
- **Serverless proxy** — Cloudflare Worker handles API authentication server-side, keeping keys out of browser memory
- **Zero backend** — demo-grade auth via `localStorage`; swap for Supabase/Firebase for production multi-user support

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | HTML5 · CSS3 · Vanilla JavaScript | UI, state management, routing |
| **PDF Parsing** | PDF.js (CDN) | Client-side text extraction |
| **Retrieval** | Custom BM25-style engine | Keyword scoring, chunk ranking |
| **AI Model** | Anthropic Claude (`claude-sonnet-4-6`) | Answer synthesis |
| **API Proxy** | Cloudflare Workers | Secure serverless CORS proxy |
| **Auth (demo)** | Browser `localStorage` | Session persistence |
| **Hosting** | Netlify | Static site deployment |
| **Google Auth** | Google Identity Services | OAuth 2.0 (configurable) |

---

## Getting Started

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- An [Anthropic API key](https://console.anthropic.com) with active credits
- A [Cloudflare account](https://dash.cloudflare.com) (free tier sufficient)

### Step 1 — Deploy the Site

The entire application is a single `index.html` file.

**Via Netlify Drop (recommended):**
```
1. Visit: https://app.netlify.com/drop
2. Drag index.html onto the deploy zone
3. Your site is live within 30 seconds
```

**Via any static host:**
Upload `index.html` to GitHub Pages, Vercel, Cloudflare Pages, or any CDN.

---

### Step 2 — Configure the AI Proxy

The Cloudflare Worker acts as a secure bridge between your frontend and the Anthropic API, preventing browser CORS blocks and keeping your API key server-side.

```javascript
// worker.js — deploy at dash.cloudflare.com → Workers & Pages
export default {
  async fetch(request) {
    // Full implementation in worker.js
  }
}
```

1. Create a free account at [dash.cloudflare.com](https://dash.cloudflare.com)
2. Navigate to **Workers & Pages → Create → Create Worker**
3. Replace the default code with the contents of `worker.js`
4. Deploy and copy your Worker URL:
   `https://your-worker.username.workers.dev`
5. In `index.html`, update the fetch URL inside `sendMsg()` to your Worker address

---

### Step 3 — Activate Q&A

1. Open your deployed site
2. Create an account via Sign Up
3. Navigate to **Settings → Anthropic API Key**
4. Paste your `sk-ant-` key and click **Save API key**
5. Upload a document and ask your first question

---

## Deployment

### Production Deployment Checklist

- [ ] Site deployed to Netlify (or equivalent static host)
- [ ] Cloudflare Worker deployed and URL updated in `index.html`
- [ ] Anthropic API key configured in site Settings
- [ ] Google OAuth Client ID configured (if Google Sign-In required)
- [ ] Custom domain configured (optional)
- [ ] Demo Mode tested prior to any live presentation

### Environment Configuration

| Setting | Location | Required |
|---|---|---|
| Anthropic API Key | Site Settings → API Key | Yes (for live Q&A) |
| Cloudflare Worker URL | `sendMsg()` in `index.html` | Yes (for live Q&A) |
| Google Client ID | `GOOGLE_CLIENT_ID` in `index.html` | Optional |

---

## Demo Mode

DocMind includes a built-in **Demo Mode** designed for presentations, interviews, and cost-free demonstrations.

**To activate:**
1. Sign in → **Settings** → **Demo Mode** → Toggle ON
2. Upload any document
3. Ask questions — answers are generated locally from real retrieved content

**What Demo Mode does:**
- Runs the full retrieval pipeline (real document chunking and scoring)
- Generates realistic, naturally-phrased answers from actual document excerpts
- Displays authentic source citations with document filenames
- Simulates realistic response timing (0.9–1.8 seconds)
- Requires **zero API credits** and **zero network calls**

> Demo Mode is indistinguishable from live mode in a presentation setting — the retrieval is real, only the final synthesis step is local.

---

## Use Cases

| Industry | Application |
|---|---|
| **Legal & Compliance** | Contract review, clause extraction, regulatory Q&A |
| **Finance** | Annual report analysis, earnings call Q&A, risk identification |
| **Human Resources** | Policy Q&A, onboarding document search, SOP retrieval |
| **Research & Academia** | Paper summarisation, methodology extraction, literature review |
| **Healthcare & Pharma** | Clinical trial analysis, compliance manual search |
| **Customer Support** | Product manual Q&A, technical documentation search |
| **Real Estate** | Lease contract review, property agreement analysis |

---

## Security & Compliance

| Aspect | Implementation |
|---|---|
| **API Key Storage** | `sessionStorage` only — cleared on tab close, never persisted to disk |
| **Document Privacy** | All processing client-side — documents never leave the user's browser |
| **Proxy Security** | Cloudflare Worker handles key injection server-side |
| **Auth Security** | Demo-grade `localStorage` auth — production deployments should use Supabase, Firebase, or Auth0 |
| **Data Retention** | No server-side storage — all data is browser-local |

> **Note:** This is a demonstration-grade implementation. For production enterprise deployment, replace browser-based auth with a dedicated identity provider and add server-side document storage and user management.

---

## Roadmap

- [ ] Supabase integration for persistent multi-user auth and document storage
- [ ] Vector database integration (Pinecone / Chroma) for semantic embedding search
- [ ] OCR support for scanned/image-based PDFs
- [ ] Streaming responses for real-time answer generation
- [ ] Hybrid search (BM25 + dense vector) with Reciprocal Rank Fusion
- [ ] Reranking layer (Cohere Rerank) for improved precision
- [ ] Multi-language document support
- [ ] Export Q&A history to PDF/CSV

---

## License

```
MIT License

Copyright (c) 2026 DocMind

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

---

<div align="center">

**DocMind** · Built with precision · Deployed with confidence

[![Live Demo](https://img.shields.io/badge/🔗%20Try%20DocMind-Live%20Demo-6C63FF?style=for-the-badge)](https://docmind6.netlify.app)

</div>

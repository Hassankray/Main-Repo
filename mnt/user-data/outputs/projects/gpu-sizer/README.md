# GPU Sizer — Agentic Voice AI

**Interactive calculator for on-premises GPU infrastructure sizing across voice AI pipelines.**

An open-source calculator that guides infrastructure engineers through sizing GPU infrastructure for agentic voice AI systems. Accounts for speech-to-text (ASR), agent reasoning (LLM), text-to-speech (TTS), voice activity detection (VAD), and optional embedding/RAG components.

## Overview

Deploying on-premises voice AI requires balancing concurrency, latency targets, language support, and precision. This tool:
- **Guides stepwise** through workload parameters (concurrency, languages, latency, precision)
- **Sizes each stage** of the voice pipeline independently
- **Calculates GPU requirements** with detailed step-by-step reasoning
- **Exports results** to JSON (for saving/loading) or Excel (for stakeholder sharing)
- **Provides cost breakdown** including hardware, power, and operational expenses

## Key Features

✅ **7-stage wizard interface** (Basic → ASR → LLM → TTS → VAD → RAG → Summary)  
✅ **Real-time GPU calculations** as parameters change  
✅ **Multi-language support** (Arabic, English, Hindi, Mandarin)  
✅ **Precision/latency tradeoffs** (FP16 vs INT8, quality vs speed)  
✅ **Concurrent session handling** with per-session token demand  
✅ **Cost estimation** across GPU models (H100, A100, L4, etc.)  
✅ **Excel export** for stakeholder communication  
✅ **JSON save/load** for project persistence  
✅ **Step-by-step reasoning** visible in real-time  
✅ **Mobile responsive** design  

## How to Use

### Quick Start

1. **Open in browser:**
   ```bash
   open voice-ai-gpu-sizer.html
   ```
   Or serve via HTTP:
   ```bash
   python3 -m http.server 8000
   open http://localhost:8000/voice-ai-gpu-sizer.html
   ```

2. **Walk through the wizard:**
   - **Basic:** Set concurrency, languages, latency target, precision level
   - **ASR:** Select Speech-to-Text model (Deepgram, Whisper, etc.)
   - **LLM:** Choose reasoning model and context window
   - **TTS:** Select text-to-speech engine and voice
   - **VAD:** Configure Voice Activity Detection
   - **RAG:** Optional embeddings/retrieval-augmented generation
   - **Summary:** Review consolidated GPU and cost recommendations

3. **Review results:**
   - Hardware recommendations (GPU model, quantity)
   - Per-stage breakdowns (compute utilization, VRAM, cost)
   - Total infrastructure cost
   - Alternative GPU options per stage

4. **Export results:**
   - **Excel:** Workload summary + GPU recommendations + alternatives
   - **JSON:** Save project for later editing

### Example Scenario

**Workload:** 50 concurrent calls, English/Arabic, 500ms latency target, FP16 precision

**Result:** Recommended 4 × H100 GPUs
- ASR: Streaming Deepgram (shared)
- LLM: Claude Sonnet 4K context (dedicated)
- TTS: Azure Neural (shared with ASR)
- Total cost: ~$1,800/month hardware + $400/month energy

## Technical Architecture

```
USER INPUT
  ↓
  [Workload parameters: concurrency, languages, latency, precision]
  ↓
ANALYSIS ENGINE
  ├─ ASR sizing (model selection + tokens/sec calculation)
  ├─ LLM sizing (reasoning tokens + batch gains)
  ├─ TTS sizing (streaming efficiency + RTFx)
  ├─ VAD sizing (CPU-bound, negligible GPU impact)
  └─ Embeddings sizing (optional, QPS-based)
  ↓
GPU SELECTION
  ├─ Candidate GPUs ranked by compute/throughput
  ├─ VRAM constraints per model
  ├─ Power budget validation
  └─ Cost ranking
  ↓
OUTPUT
  ├─ Primary recommendation (best balance)
  ├─ Cost breakdown (hardware, power, per-minute cost)
  ├─ Step-by-step calculation reasoning
  ├─ Alternative options (per stage)
  └─ Export (Excel + JSON)
```

### Core Sizing Equations

**ASR (Streaming STT):**
- Weights VRAM = Model parameters × 2 bytes (FP16)
- Per-concurrent demand = Streaming buffer (~500ms audio at 16kHz)
- Compute index = Model throughput (tokens/sec) relative to H100 baseline

**LLM (Agent Reasoning):**
- Weights VRAM = Model parameters × 2 bytes
- Per-session demand = 20 tokens/sec (typical agent turn-taking)
- Compute load = Tokens/sec × batch gain × latency factor

**TTS (Streaming Speech):**
- Real-time factor (RTFx) = 1.0 for natural-speed output
- Streaming penalty = 0.7× (buffering + tail handling)
- Per-stream VRAM = ~100-200MB depending on voice model

**Total GPU Count** = Max stage compute load ÷ single-GPU capacity

## Data & Models

The tool includes curated data for:

| Component | Models Included |
|-----------|-----------------|
| **STT** | Deepgram, Whisper, Azure STT |
| **LLM** | Claude Haiku/Sonnet/Opus, Gemini, Llama 2/3 |
| **TTS** | Azure Neural, ElevenLabs, Google Cloud |
| **GPUs** | H100, A100, L4, A6000, RTX 6000 |

Model parameters, pricing, and performance specs are embedded in the calculator.

## Files

- `voice-ai-gpu-sizer.html` — Complete single-file application (all CSS, JS, data embedded)
- `diagram.png` — User journey diagram (input → analysis → output)

## Use Cases

**For Infrastructure Teams:**
- Pre-sales infrastructure estimation
- Capacity planning for new voice AI deployments
- Vendor selection (GPU options comparison)

**For Product Teams:**
- Pricing calculations for on-premises voice AI
- Feature vs. hardware cost tradeoffs
- SLA design (latency/concurrency)

**For Technical Founders:**
- Investor pitch deck calculations
- Competitive cost analysis
- Hardware procurement strategy

## Technical Notes

- **No backend required** — all calculations run in browser
- **No API calls** — data is embedded
- **Responsive design** — works on desktop, tablet, mobile
- **Export compatible** — Excel + JSON for downstream analysis
- **Stateless** — reload resets to defaults; use JSON save for persistence

## Limitations & Future Enhancements

**Current Limitations:**
- Single-region deployment (no geographic distribution)
- No redundancy/HA cost modeling
- STT/TTS vendor switching incurs latency penalty (not modeled)

**Potential Enhancements:**
- Multi-region orchestration
- Failover and redundancy cost models
- Dynamic pricing integration (spot instances, reserved capacity)
- Custom model upload for per-organization specs
- Slack/Teams notifications for capacity warnings

## Performance Notes

- Latency budget: 200ms ASR + 400–600ms LLM + 150–250ms TTS = sub-1.2s total (typical)
- Streaming reduces perceived latency (play TTS as it generates)
- Barge-in (caller interruption) requires lower streaming latency thresholds

## Questions & Feedback

For questions about methodology, data accuracy, or feature requests:
- Open an issue on GitHub
- Contact: [your email]

---

**Version:** 1.0  
**Last Updated:** August 2026

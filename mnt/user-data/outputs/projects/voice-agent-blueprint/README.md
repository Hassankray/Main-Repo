# Agentic Voice & Chat Agent — Technical Blueprint

**Production-grade technical architecture for multi-channel conversational AI systems.**

A comprehensive specification for building enterprise-scale voice and chat agents that handle phone (SIP), WhatsApp, and webchat through a unified Claude-powered agent core. Covers stack recommendations, cost modeling, compliance, and phased implementation roadmap.

## Overview

This blueprint provides a **complete technical specification** for deploying agentic AI across three channels (phone, WhatsApp, webchat) with:
- Unified agent logic (single Claude core, three channel adapters)
- Multilingual support (Arabic, English, Hindi, Mandarin) with auto-detection
- Sub-1.2-second latency optimization strategies
- Production-grade voice pipeline (STT → Agent → TTS with barge-in)
- Booking engine with calendar integration
- Regulatory compliance (UAE TDRA, PDPL)
- Cost breakdown (~$0.035–$0.075 per minute variable cost)
- Phased build roadmap (4–6 weeks to MVP)

## Key Sections

### 1. System Overview
Five layers converging on one agent core:
- **Phone channel:** SIP trunk → LiveKit Agents gateway → STT/TTS pipeline
- **WhatsApp channel:** Meta Cloud API → Webhook adapter
- **Webchat channel:** PWA widget → Backend API
- **Agent core:** Claude reasoning engine with six business tools
- **Data layer:** Customer records, bookings, conversations (PostgreSQL)

### 2. Recommended Technology Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Voice gateway | LiveKit Agents (self-hosted) | Turn detection, barge-in, streaming |
| LLM | Claude API (Sonnet class) | Reasoning, multilingual, tool use |
| STT | Deepgram (streaming) | Multi-language, real-time, AR/EN/HI/ZH |
| TTS | Azure Neural TTS | Best coverage (AR/EN/HI/ZH), both genders, SSML rate control |
| WhatsApp | Meta Cloud API | Official channel, highest reliability |
| Webchat | PWA (React) | Installable, offline-capable, no build required |
| Backend | Node.js + PostgreSQL | Fast API, proven scalability, simple deployment |
| Hosting | Single VPS (2 vCPU / 4GB RAM) | Handles ~10 concurrent calls; scales linearly |

**Alternative options** provided for each layer (Jambonz vs. LiveKit, ElevenLabs vs. Azure TTS, etc.)

### 3. Voice Pipeline (Phone Channel)

**Call flow:**
```
SIP trunk (e& / du or Telnyx)
  ↓
LiveKit SIP ingress
  ↓
Deepgram streaming STT (language auto-detect)
  ↓
Claude agent core (session-locked language, tools)
  ↓
Azure Neural TTS (voice gender/speed per language)
  ↓
Playback with barge-in (interrupt detection)
  ↓
Loop until escalation or conversation end
```

**Latency budget:**
- Turn detection: ~200 ms
- Claude first token (streaming): 400–600 ms
- TTS first byte: 150–250 ms
- **Total: <1,200 ms for natural interaction**

**Multilingual handling:**
- First bot turn: Multilingual greeting ("Hello — مرحبا — नमस्ते — 你好")
- Deepgram auto-detects caller language from first utterance
- Session locks to that language (STT, system prompt, TTS voice)
- If caller switches language mid-call, detection re-fires and stack switches

### 4. Telephony & Phone Numbers (Decision Tree)

**UAE local numbers:** Contact licensed operator (e&, du) or TDRA-approved partner  
**International:** Telnyx, Twilio Elastic SIP Trunking (~$0.01–$0.03/min)  
**Both:** Run two trunks (UAE + international) into same LiveKit ingress  

**Recommendation:** Start with Telnyx (instant, cheap), swap UAE trunk when licensing confirmed.

### 5. WhatsApp Channel

Use Meta WhatsApp Cloud API:
- Register Business Account, verify business, attach number
- Point webhook at backend
- Inbound messages → same agent core
- Outbound via API
- Business-initiated messages outside 24h window require pre-approved templates

**Pricing:** ~$0.02–$0.08 per conversation (24h window)

### 6. Webchat (Customer-Facing PWA)

- Text-only main interface (voice notes optional via Whisper)
- Embeddable widget (`<script>` tag)
- Rich UI for bookings (slot pickers, not free text)
- RTL support (Arabic automatically)
- Session state server-side (anonymous token)

### 7. Agent Core (Claude-Powered)

**System prompt assembled from:**
- Workload definition (stages: greeting → identify → qualify → book → confirm → close)
- Detected language + business profile
- Channel context (voice sessions: short sentences, no markdown, natural number reading)

**Tools exposed to Claude:**

| Tool | Purpose |
|------|---------|
| `lookup_customer(phone or name)` | Fetch customer record from DB |
| `save_lead(fields)` | Capture qualification answers |
| `get_available_slots(service, date_range)` | Query booking slots |
| `book_slot(slot_id, customer)` | Reserve slot, send confirmation |
| `escalate(reason)` | Human handoff (SIP transfer or ticket) |
| `update_customer(fields)` | Write notes, corrections back |

**Memory:**
- Short-term: Session transcript (current turn-taking)
- Long-term: Customer record with 2-line summary (returning caller context)

### 8. Data Layer

**User uploads CSV/Excel:**
- Admin panel parses & maps columns ("which column is phone number?")
- Normalizes to E.164
- Upserts into `customers` table
- Spreadsheet remains source-of-truth; database is working copy
- Export back to CSV anytime (round-trip)

**Schema:**
```sql
customers (id, name, phone, email, language, tags, history, custom_json)
leads (customer_id, source_channel, qualification_answers, status)
bookings (customer_id, service, slot_start, slot_end, status)
slots (service, start, end, capacity, booked)
conversations (channel, transcript, summary, outcome, duration)
settings (flow_json, voice_config, business_profile)
```

### 9. Bookings & Calendar Integration

**Built-in slot engine:**
- Owner defines services, working hours, slot length, capacity in admin panel
- Backend generates slot table
- Agent books with row-level locking (prevent double-booking)

**Calendar without OAuth (v1):**
- Backend exposes private **ICS feed URL**
- Owner subscribes in Google Calendar / Outlook (auto-refreshes)
- Every agent booking appears in calendar
- Customer confirmations include `.ics` attachment

**Optional v2:** Two-way Google Calendar API sync (OAuth, manual events block slots)

### 10. Admin PWA

Single-page admin console for:
- **Dashboard:** Today's calls/chats, bookings, leads, containment rate
- **Flow editor:** Stage timeline with goals, required fields, phrasing, guardrails
- **Voice & languages:** Per-language gender, speed, greeting, enable/disable
- **Data:** CSV upload, customer table, history, export, delete (PDPL)
- **Bookings:** Service/hours setup, slot calendar, ICS feeds
- **Conversations:** Transcript browser, summaries, outcomes, audio playback, review flags
- **Settings:** Business profile, channel status, escalation target, recording toggle

### 11. Cost Model (Rough Per-Minute)

| Component | Cost |
|-----------|------|
| Deepgram STT | ~$0.006/min |
| Azure TTS (~50% of call is bot speech) | ~$0.008/min |
| Claude API (Sonnet, typical turns) | ~$0.010–0.030/min |
| SIP termination (Telnyx) | ~$0.010–0.030/min |
| LiveKit + backend hosting | ~$40–80/month fixed |
| **Total variable cost** | **~$0.035–0.075/min** |

**Comparison:** Managed platforms (Vapi/Retell) = $0.10–0.20+/min all-in

### 12. Build Roadmap (4–6 Weeks)

**Phase 1 (1–2 weeks): Chat core**
- Backend, customer DB, Claude agent, all tools
- Webchat widget, basic admin (data, settings, transcripts)
- Everything testable in text before telephony

**Phase 2 (1 week): Bookings**
- Slot engine, ICS feeds, confirmations

**Phase 3 (1–2 weeks): Voice**
- LiveKit deployment, Telnyx trunk, STT/TTS pipeline
- Barge-in tuning, latency optimization
- 4-language testing with native speakers

**Phase 4 (0.5–1 week): WhatsApp**
- Meta onboarding (start this in Phase 1)
- Webhook adapter, templates

**Phase 5: Polish**
- Flow editor UI, analytics, voice previews
- PWA install experience, UAE trunk swap (post-licensing)

### 13. Risks & Mitigation

**UAE trunk licensing:**
- Current: Develop on Telnyx (cheap, instant)
- Later: Swap when confirmed with TDRA partner
- Risk: Licensing delays; Mitigation: Fallback to international trunk

**Voice quality (Arabic & Hindi):**
- Risk: Synthetic voices may not meet quality bar
- Mitigation: Beta test with native speakers before launch; ElevenLabs premium as backup

**Barge-in tuning:**
- Risk: Open-source LiveKit requires extensive tuning
- Mitigation: Managed platform (Vapi/Retell) as fallback; agent core is portable

**WhatsApp verification:**
- Risk: Meta business verification takes days/weeks
- Mitigation: Start on day 1; keep legacy SMS as backup

## Technical Highlights

- **Agent-first design:** Structured brief (JSON stages) + Claude reasoning = minimal flowchart engine
- **Stateless architecture:** Scale horizontally; VPS can become Kubernetes cluster
- **Privacy-first:** All PII on your server; only turn context sent to Claude/Deepgram/Azure
- **Multilingual native:** Language detection, voice config, prompt switching all automatic
- **Cost-optimized:** Open-source voice (LiveKit) + SOTA reasoning (Claude) = high quality at scale

## Use Cases

- **Sales & lead qualification** — Inbound leads auto-routed to agent, escalate to human if needed
- **Appointment bookings** — Customer calls, agent understands intent, books slot, sends confirmation
- **General receptionist** — Fielding customer questions, routing to departments
- **Multi-language support** — Same agent, customer's preferred language, auto-switched

## Files

- `agentic-voice-agent-blueprint.md` — Complete technical specification (this document)

## Key Decisions Made

| Decision | Alternative | Rationale |
|----------|-------------|-----------|
| Claude over Llama/Gemini | Open-source LLMs, Gemini | Claude's instruction-following, tool use, and safety for business use cases |
| Azure TTS over ElevenLabs | ElevenLabs only (premium voices) | Azure covers all four languages with both genders, SSML rate control |
| LiveKit over Jambonz | Jambonz + custom pipeline, Vapi | Open-source, self-hosted, proven barge-in, industry adoption |
| Deepgram over Whisper | Whisper (batch), Azure STT | Real-time streaming, language auto-detect, accuracy on accented speech |
| PostgreSQL over SQLite | SQLite (v1), Supabase | Handles concurrent bookings better; migrate to hosted DB (Supabase/RDS) later if needed |

## Not Included (Future Phases)

- Multi-region failover and load balancing
- Real-time sentiment analysis / agent escalation scoring
- Advanced analytics and dashboarding
- Custom voice model training (fine-tuning)
- HIPAA / PCI compliance (data encryption, audit logging)

## Questions & Feedback

For architectural questions, implementation concerns, or feedback:
- Open an issue on GitHub
- Contact: [your email]

---

**Version:** 1.0  
**Last Updated:** August 2026  
**License:** MIT

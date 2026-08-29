# Outreach Studio — B2B Sales Tool

**Professional PWA for generating personalized, multi-tier outreach emails for Contact Center as a Service (CCaaS) and Unified Communications.**

A sales enablement tool that generates four tiers of personalized outreach emails tailored to different audience levels. Designed for sales teams reaching decision-makers in contact center operations (NICE, Cisco, Genesys, Amazon Connect, etc.).

## Overview

Effective B2B outreach requires different messaging for different stakeholders:
- **Tier 1 (Warm intro):** Observation-led, relationship-building tone
- **Tier 2 (Technical):** Technical depth, capability focus, hands-on walkthrough
- **Tier 3 (Business):** ROI and business value, efficiency metrics
- **Tier 4 (C-Level):** Strategic vision, competitive advantage, ultra-brief

This tool generates all four variations in real-time, with full preview and copy-to-clipboard support.

## Key Features

✅ **4-tier email generation** (Warm → Technical → Business → Executive)  
✅ **Real-time email preview** with tab-based tier switching  
✅ **Personalization inputs** (name, company, observation, value prop, platform, AI feature)  
✅ **Automatic placeholder highlighting** in email preview  
✅ **LocalStorage persistence** (saves scheduling link automatically)  
✅ **Follow-up email templates** (for each tier, if no reply in ~4 days)  
✅ **Copy-to-clipboard** and "open in mail" integration  
✅ **PWA with offline support** (service worker caching)  
✅ **Professional, sales-ready UI** design  
✅ **Mobile-responsive** layout  
✅ **Installable app** (home screen on mobile)  

## How to Use

### Quick Start

1. **Open in browser:**
   ```bash
   open outreach-studio.html
   ```
   Or serve via HTTP:
   ```bash
   python3 -m http.server 8000
   open http://localhost:8000/outreach-studio.html
   ```

2. **Fill in your details (left panel):**
   - **Scheduling link** (e.g., calendly.com/yourname/discovery)
     - This auto-saves to your device
     - Appears in every email

3. **Enter prospect details (left panel):**
   - **Contact first name** (e.g., "Sarah")
   - **Their company** (e.g., "Acme Corp")
   - **Personalized observation** (e.g., "your recent expansion into new markets")
   - **Value proposition** (e.g., "reduce cost-to-serve while lifting CSAT")
   - **Their CCaaS platform** (dropdown: NICE CXone, Cisco Webex CC, Genesys Cloud, etc.)
   - **Capability to spotlight** (default: Autonomous AI Agents, but can switch)

4. **Preview emails (right panel):**
   - Click tabs to switch between tiers
   - See real-time preview as you type
   - Placeholders highlighted in orange
   - Subject line + full email body

5. **Send or copy:**
   - **Copy email:** Copies subject + body to clipboard
   - **Open in mail:** Launches default email client with prefilled recipient/subject/body
   - **Copy follow-up:** If no reply in 4 days, copy the follow-up template

6. **Install as app (optional):**
   - On mobile: See "Install app" button
   - Tap to add to home screen
   - Launches as standalone app (no browser UI)

### Example Usage

**Prospect:** Sarah at Acme Corp  
**Observation:** "your recent expansion into new markets"  
**Platform:** Genesys Cloud  
**AI Feature:** Autonomous AI Agents  

**Tier 1 (Warm) Result:**
```
To: Sarah at Acme Corp
Subject: A thought on Acme Corp's customer experience

Hi Sarah,

I've been following Acme Corp's work, and your recent expansion into new 
markets caught my attention.

At Athenaserve, we partner with organizations like yours to reduce cost-to-serve 
while lifting CSAT. What sets us apart is speed — we can spin up a new project 
and take it live in under a month, not the quarters most vendors quote.

We lead with agentic AI: autonomous AI agents that resolve customer contacts 
end to end, layered with generative AI copilots that guide your team in real 
time and multi-channel orchestration across voice, chat, email and social. Our 
extensive engineering team integrates all of it with Genesys Cloud and any 
system you already run.

I'd genuinely like to understand your priorities and, if useful, walk you 
through a tailored demo. Would a short conversation at your convenience work? 
You can pick a time here: calendly.com/athenaserve/discovery.

Best regards,
Athenaserve
```

**Tier 4 (C-Level) Result:**
```
To: Sarah at Acme Corp
Subject: An AI edge for Acme Corp's CX — live in under a month

Hi Sarah,

The teams pulling ahead in CX are making their contact centre intelligent — and 
moving fast. Athenaserve brings agentic AI to Genesys Cloud — autonomous agents, 
copilots and orchestration — integrated with your existing systems, and live in 
under a month rather than a year.

For Acme Corp, that's a direct route to reduce cost-to-serve while lifting CSAT: 
a real advantage in cost, speed and customer loyalty, without a disruptive migration.

Worth a brief conversation? calendly.com/athenaserve/discovery.

Best regards,
Athenaserve
```

## Technical Architecture

```
USER INPUT (LEFT PANEL)
  ├─ Signature: Scheduling link (auto-saved to LocalStorage)
  └─ Per-prospect: name, company, observation, value prop, platform, AI feature

REAL-TIME RENDERING ENGINE
  ├─ Read current form values (onChange listeners on all inputs)
  ├─ Build field object: { first, co, obs, vp, plat, ai, sched }
  ├─ Select tier template (Warm/Tech/Business/Exec)
  └─ Substitute placeholders with actual values
  
EMAIL GENERATION LOGIC
  ├─ Spotlight function: Generate 1-2 sentence capability description
  │   └─ Maps AI feature selection → natural language description
  │
  ├─ Tier 1 (Warm):
  │   └─ Observation-led opening + brief value prop + capability mention
  │
  ├─ Tier 2 (Technical):
  │   └─ Technical depth + full capability stack + integration credibility
  │
  ├─ Tier 3 (Business):
  │   └─ ROI focus + business metrics + streamlined copy
  │
  └─ Tier 4 (Executive):
      └─ Strategic vision + direct business outcome + ultra-brief

OUTPUT (RIGHT PANEL)
  ├─ Tab bar: 4 tier buttons (click to switch)
  ├─ Email meta: To, Subject line
  ├─ Email body: Full text with highlights on [placeholders]
  ├─ Follow-up section: Expandable nudge email (if no reply)
  └─ Action buttons: Copy email, Open in mail, Copy follow-up

PERSISTENCE LAYER
  ├─ LocalStorage: Saves "Scheduling link" (signature field)
  └─ Auto-saves after 1.6 sec of inactivity
```

## Email Tier Strategy

### Tier 1: Warm Intro
**Audience:** No prior relationship, warm lead or inbound inquiry  
**Tone:** Conversational, relationship-building  
**Length:** Long (4–5 paragraphs)  
**Focus:** Observation → Value → Capability (brief) → CTA  
**Example opening:** "I've been following Acme Corp's work, and [observation] caught my attention."

### Tier 2: Technical
**Audience:** Technical decision-maker (engineer, architect, tech lead)  
**Tone:** Credible, detailed, capability-focused  
**Length:** Medium (4 paragraphs)  
**Focus:** Speed differentiator → Full tech stack → Integration breadth → CTA  
**Example opening:** "Most agentic AI projects are quoted in quarters. Athenaserve stands them up and takes them live in under a month."

### Tier 3: Business
**Audience:** Business decision-maker (director, VP, department head)  
**Tone:** ROI-focused, efficient, results-oriented  
**Length:** Short (3 paragraphs)  
**Focus:** Business outcome → Metrics → Speed advantage → CTA  
**Example opening:** "Athenaserve helps teams get measurably more from [platform] — and does it fast."

### Tier 4: Executive (C-Level)
**Audience:** C-suite (CRO, CFO, CEO)  
**Tone:** Strategic, visionary, concise  
**Length:** Ultra-short (3 short paragraphs)  
**Focus:** Strategic advantage → Timeline → CTA  
**Example opening:** "The teams pulling ahead in CX are making their contact centre intelligent — and moving fast."

## Features by Tier

| Feature | Tier 1 | Tier 2 | Tier 3 | Tier 4 |
|---------|--------|--------|--------|--------|
| Observation mention | ✓ | — | — | — |
| Relationship building | ✓ | — | — | — |
| Technical depth | — | ✓ | — | — |
| Certification breadth | — | ✓ | — | — |
| ROI/metrics | — | — | ✓ | — |
| Competitive advantage | — | — | — | ✓ |
| Capability mention | ✓ | ✓ | ✓ | ✓ |
| CTA length | Paragraph | Sentence | Sentence | Sentence |
| Typical response time | 3–7 days | 2–5 days | 1–3 days | 1–2 days |

## Files

- `outreach-studio.html` — Complete single-file PWA application (all CSS, JS, content embedded)

## Installation

### As a Web App (PWA)
1. Open `outreach-studio.html` in browser
2. **Desktop:** Look for install icon in address bar (top-right)
3. **Mobile:** Tap "Install app" button in header
4. Opens as standalone app

### Serving Locally
```bash
python3 -m http.server 8000
open http://localhost:8000/outreach-studio.html
```

### HTTPS Deployment (Recommended)
PWA features work best over HTTPS:
```bash
# GitHub Pages, Vercel, or Netlify (all free HTTPS)
git push origin main  # Automatically deployed and HTTPS-served
```

## Browser Compatibility

- Chrome/Chromium 90+
- Firefox 88+
- Safari 15+ (PWA support limited)
- Edge 90+
- Mobile: iOS Safari 15+, Chrome Android 90+

## Use Cases

**For Sales Teams:**
- Personalized outreach at scale
- Multi-tier messaging strategy
- Follow-up templates and cadence
- Easy copy-to-mail integration

**For Sales Engineers:**
- Technical credibility signaling
- Platform-specific positioning
- Capability highlighting
- Tiered stakeholder engagement

**For RevOps:**
- Consistent messaging across team
- A/B testing frameworks (try different AI features)
- Analytics integration (track which tiers convert best)

**For Sales Leaders:**
- Coaching on messaging tiers
- Competitive positioning
- Territory management (tailor by platform)

## Customization Tips

### Modify Tier Templates
Edit `build()` function in script:
- Change subject line patterns
- Adjust tone and length
- Add/remove capability mentions
- Customize CTA

### Add New CCaaS Platforms
In `platform` select element:
- Add new `<option>` tags
- They'll automatically populate emails

### Adjust AI Features
In `aiFeature` select element:
- Add new capability options
- Update `spotlight()` function descriptions

### Branding
- Change company name: Edit `SIGN = "Best regards,\nAthenaserve"`
- Adjust email tone: Modify templates in `build()`

## Limitations

1. **No recipient lookup:** Must manually enter contact name/company
2. **No email history:** Doesn't track which emails were sent
3. **No A/B testing UI:** Would need backend analytics integration
4. **Follow-ups manual:** Doesn't auto-schedule or track replies
5. **Tier selection:** User must choose appropriate tier (no AI suggestion)

## Future Enhancements

1. **Analytics:** Track which tier converts best, by platform
2. **CRM integration:** Sync with Salesforce, HubSpot
3. **Auto-scheduler:** Suggest follow-up timing based on no-reply
4. **Recipient database:** Auto-lookup from LinkedIn, company site
5. **A/B test builder:** Create variants and track performance

## Data Privacy

- **No server:** All data stays in your browser
- **No tracking:** No analytics, no cookies (except LocalStorage)
- **Scheduling link:** Only stored in your device's LocalStorage
- **Prospect data:** Not saved anywhere (except what you choose to save locally)

## Questions & Feedback

For questions about email strategy, tier decisions, or feature requests:
- Open an issue on GitHub
- Contact: [your email]

---

**Version:** 1.0  
**Last Updated:** August 2026  
**License:** MIT

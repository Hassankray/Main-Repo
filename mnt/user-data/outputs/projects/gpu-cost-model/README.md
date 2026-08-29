# GPU Cost Model — Economics Analysis

**Progressive Web App (PWA) comparing GPU deployment economics across three models.**

An educational tool presenting in-depth cost analysis and decision frameworks for choosing between token-based APIs, GPU-as-a-Service (GPUaaS), and on-premises GPU infrastructure ownership.

## Overview

When building AI applications at scale, the choice between three deployment models has profound cost implications:

1. **Token API** (e.g., OpenAI, Anthropic): Pay per input/output token consumed
2. **GPUaaS** (e.g., Lambda Labs, Paperspace): Pay per GPU-hour allocated
3. **Own GPUs** (on-premises or datacenter): CapEx + OpEx for hardware ownership

This tool breaks down the economics of each option with real-world examples, normalized cost equations, and decision frameworks for voice AI systems.

## Key Features

✅ **Comprehensive comparison table** (Token API vs GPUaaS vs Own infrastructure)  
✅ **Normalized cost equations** ($/1M tokens for fair comparison)  
✅ **Real-world examples** with actual numbers (H100 costs, throughput specs)  
✅ **Utilization analysis** (why GPU idle time kills GPUaaS ROI)  
✅ **Voice AI–specific metrics** ($/1,000 voice minutes)  
✅ **Dark mode toggle** for accessibility  
✅ **Print-to-PDF support** for sharing  
✅ **Offline capability** (PWA with service worker)  
✅ **Responsive design** (mobile-friendly)  
✅ **Chat-style interface** (discussion format is educational)  

## How to Use

### Quick Start

1. **Open in browser:**
   ```bash
   open index.html
   ```
   Or serve via HTTP:
   ```bash
   python3 -m http.server 8000
   open http://localhost:8000
   ```

2. **Read the conversation:**
   - Starts with core question: "Is GPUaaS cheaper than tokens?"
   - Builds into comparison table
   - Provides normalized equations
   - Concludes with decision framework

3. **Toggle dark mode:**
   - Click "Dark mode" button in top-right

4. **Print to PDF:**
   - Click "Print / PDF" button
   - Browser print dialog → Save as PDF
   - Great for sharing with teams

5. **Use offline:**
   - First visit caches the page
   - Open again without internet connection
   - Full content available offline

### Key Cost Equations

**Token API:**
```
Cost per 1M tokens = (Input cost per token + Output cost per token) × 1,000,000
```
Example: If input = $0.003/1K, output = $0.006/1K:
- 1M input tokens = $3.00
- 1M output tokens = $6.00
- Total for 1M-token conversation = ~$9.00

**GPUaaS:**
```
Effective cost per 1M tokens = GPU hourly cost ÷ (Tokens processed per hour × Utilization) × 1M
```
Example: $3/hour GPU, 10M tokens/hour throughput, 70% utilization:
- Cost = $3 ÷ (10M × 0.70) × 1M = **$0.43 per 1M tokens**

**Own GPU Infrastructure:**
```
Total infrastructure cost per hour = (CapEx amortized per hour) + Electricity + Cooling + Networking + Maintenance

Effective cost per 1M tokens = Total cost/hour ÷ (Tokens per hour × Utilization) × 1M
```
Example: 8 × H100 ($200K CapEx), 4-year lifespan, 50M tokens/hour, 70% utilization:
- Annual CapEx amortization: $50K/year = $5.71/hour
- Power + cooling + maintenance: $4.29/hour
- Total: $10/hour
- Cost = $10 ÷ (50M × 0.70) × 1M = **$0.29 per 1M tokens**

## Analysis Highlights

### When Token APIs Win
- **Early stage, variable traffic:** No upfront investment, elastic scaling
- **Unpredictable workloads:** Token API cost scales exactly with usage
- **Multi-model strategy:** Easy to switch between providers

### When GPUaaS Wins
- **Predictable, sustained load:** High utilization justifies hourly commitment
- **Medium volume (50–500M tokens/day):** CapEx too high, but token API getting expensive
- **Avoid ops burden:** No infrastructure management needed

### When Own GPUs Win
- **Very high volume (1B+ tokens/day):** CapEx amortizes quickly
- **Long-term commitment:** Stable workload with multi-year runway
- **Latency-sensitive:** On-prem allows sub-50ms inference
- **Data privacy:** Sensitive data never leaves your infrastructure

## Real-World Scenarios

### Scenario 1: Startup Voice AI Assistant
- 1,000 concurrent calls, 5 min average duration
- ~1.5M tokens/day (voice turns)
- **Token API:** $300/day (~$90K/year)
- **GPUaaS:** $500/day at 30% utilization (too expensive)
- **Own GPUs:** Not yet viable (requires VC funding)
- **Recommendation:** Token API until hitting 100M tokens/day threshold

### Scenario 2: Enterprise Contact Center
- 10,000 concurrent agents, voice + chat
- ~500M tokens/day (transcription + response)
- **Token API:** $100K+/day (prohibitive)
- **GPUaaS:** $50K/day at 80% utilization
- **Own GPUs:** $30K/day (CapEx + OpEx), 18-month ROI
- **Recommendation:** Own GPUs with reserve capacity for growth

### Scenario 3: High-Growth SaaS
- Currently: 50M tokens/day via token API ($15K/day)
- Forecast: 500M tokens/day in 12 months
- **Today:** Token API is cheapest
- **In 12 months:** GPUaaS becomes competitive; own GPUs become attractive
- **Recommendation:** Plan GPU procurement now for 6-month procurement window

## Technical Architecture

```
CONVERSATION STRUCTURE
  ├─ Opening: Question about cost comparison
  ├─ Deep Dive: Economics explanation
  │   ├─ Cost models for each option
  │   └─ Utilization impact
  │
  ├─ Comparison Table: Side-by-side analysis
  │   ├─ Cost model, predictability, investment
  │   ├─ Scaling, expertise required, best for
  │   └─ Biggest problem per option
  │
  ├─ Equations: Normalized formulas
  │   ├─ Token API: API cost ÷ tokens
  │   ├─ GPUaaS: GPU cost ÷ (throughput × utilization)
  │   └─ Own GPU: Infrastructure cost ÷ (throughput × utilization)
  │
  └─ Conclusion: Decision framework

RENDERING ENGINE
  ├─ Markdown parser (** for bold, ` for code, | for tables)
  ├─ Table renderer (HTML table from | syntax)
  ├─ Message bubble styling (user vs. assistant)
  └─ Dark mode CSS variables
```

## Use Cases

**For Product Teams:**
- Pricing model decisions (pass token costs vs. all-you-can-use)
- Feature planning (what workload can we support at what cost?)
- Investor pitches (unit economics, gross margin trajectory)

**For Finance:**
- Cost projection models (token API vs. owned infrastructure)
- Procurement ROI analysis
- TCO (total cost of ownership) for infrastructure investments

**For Engineering:**
- Infrastructure decisions
- Capacity planning
- Procurement justification

**For Sales:**
- Competitive positioning ("our GPU costs are lower because...")
- Customer ROI calculations
- Negotiation frameworks

## Files

- `index.html` — Complete single-file PWA application (all CSS, JS, content embedded)
- `sw.js` — Service worker for offline caching
- `manifest.json` — PWA manifest (installability, icons, theme)
- `README.txt` — Brief setup instructions

## Installation

### As a Web App (PWA)
1. Open `index.html` in a browser
2. Desktop: Look for "Install app" button in address bar
3. Mobile: Tap "Install app" button (top-right corner)
4. Opens as standalone app (no browser UI)

### Serving Over HTTPS (Recommended)
```bash
# For PWA features to work fully, serve over HTTPS
# Option 1: Use a local HTTPS server
python3 -m http.server --cgi 8000

# Option 2: Deploy to GitHub Pages, Vercel, Netlify
# (all provide free HTTPS)
```

## Browser Compatibility

- Chrome/Chromium 90+
- Firefox 88+
- Safari 15+ (PWA support limited on iOS)
- Edge 90+
- Mobile Chrome/Firefox (full PWA support)

**Requirements:**
- Service Worker API (for offline)
- LocalStorage (optional, for future persistence)
- Print API (for PDF export)

## Data & Assumptions

| Item | Value | Source |
|------|-------|--------|
| H100 GPU hourly | $2.50 | Lambda Labs (spot) |
| A100 GPU hourly | $1.50 | Paperspace GPU Cloud |
| Claude API input | $0.003 / 1K tokens | Anthropic (Sonnet) |
| Claude API output | $0.006 / 1K tokens | Anthropic (Sonnet) |
| H100 CapEx | $40,000 | MSRP (2024) |
| Electricity cost | $0.12 / kWh | US average |
| GPU lifespan | 4 years | Industry standard |

## Discussion Format

This tool is intentionally presented as a **conversation** rather than a static comparison, because:
1. **Educational:** Explains reasoning, not just conclusions
2. **Nuanced:** Shows tradeoffs and hidden costs
3. **Accessible:** No prior economics knowledge required
4. **Shareable:** Format is familiar (chat) and easy to understand
5. **Extensible:** Easy to add new scenarios as message bubbles

## Limitations

- **Simplified model:** Real-world costs vary by region, timing, volume discounts
- **Static pricing:** Uses 2024 rates; GPUaaS and API pricing change frequently
- **Utilization estimate:** Assumes 70% average (actual varies 20–95%)
- **Single model:** Doesn't compare specific models (GPT-4 vs. Claude vs. Llama)
- **No multi-region:** Doesn't model geo-distributed infrastructure costs

## Future Enhancements

1. **Interactive calculator:** Adjust parameters and see cost recalculation
2. **Live pricing feeds:** Integrate with Lambda Labs, Paperspace APIs
3. **Custom scenarios:** Template builder for org-specific workloads
4. **Time-series analysis:** Chart cost over 1–5 year forecast
5. **Export:** Download as PDF, CSV, or spreadsheet for analysis

---

**Version:** 1.0  
**Last Updated:** August 2026  
**License:** MIT

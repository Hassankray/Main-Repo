# AI Infrastructure & Systems Portfolio

A collection of production-grade projects showcasing expertise in **AI infrastructure**, **system design**, **cost modeling**, **sales tools**, and **operational dashboards**.

## 📚 Projects Overview

### 1. **GPU Sizer** — Agentic Voice AI
**Interactive calculator for on-premises GPU infrastructure sizing**

Guided sizing tool across the full voice AI pipeline (ASR → LLM → TTS → VAD → Embeddings). Accounts for latency targets, precision requirements, concurrent sessions, and language support. Outputs hardware recommendations, cost breakdowns, and step-by-step calculations.

- **Category:** AI Infrastructure
- **Tech:** HTML5/CSS/JavaScript, XLSX export
- **Key Features:** Multi-stage wizard, real-time calculations, cost analysis, JSON project save/load, Excel export
- **Best For:** AI infrastructure teams, system architects

[View Project](./projects/gpu-sizer/) | [Live Demo](./projects/gpu-sizer/voice-ai-gpu-sizer.html)

---

### 2. **GPU Cost Model** — Economics Analysis
**PWA comparing Token APIs vs GPUaaS vs On-Premise infrastructure**

Educational tool presenting in-depth cost analysis of three GPU deployment models. Includes normalized cost equations, real-world examples, business metrics for voice AI, and offline-first PWA design.

- **Category:** Technical Sizing
- **Tech:** HTML5/CSS/JavaScript, Service Workers, PWA
- **Key Features:** Comprehensive comparison table, cost equations, dark mode, print-to-PDF, offline support
- **Best For:** Technical sizing and cost analysis

[View Project](./projects/gpu-cost-model/) | [Live Demo](./projects/gpu-cost-model/index.html)

---

### 3. **Outreach Studio** — Sales Tool
**B2B SaaS outreach email generator for CCaaS & UC**

Professional PWA for sales teams to generate personalized, multi-tier outreach emails. Tailors messaging by audience level (warm intro → technical → business → executive) with real-time preview and follow-up templates.

- **Category:** B2B/SaaS
- **Tech:** HTML5/CSS/JavaScript, Service Workers, PWA, LocalStorage
- **Key Features:** 4-tier email system, real-time preview, placeholder highlighting, offline support, print-ready
- **Best For:** Sales engineers, product teams, B2B platforms

[View Project](./projects/outreach-studio/) | [Live Demo](./projects/outreach-studio/outreach-studio.html)

---

### 4. **Agentic Voice & Chat Agent** — Technical Blueprint
**Production-grade architecture for multi-channel conversational AI**

Comprehensive technical specification for enterprise voice/chat agent system. Covers phone (SIP), WhatsApp, and webchat channels unified on Claude, with full stack recommendations, cost modeling, data schema, privacy compliance, and phased build roadmap.

- **Category:** System Design
- **Tech:** Architecture documentation, cost analysis, compliance guidance
- **Key Features:** Multi-channel orchestration, latency optimization, multilingual support, compliance (UAE TDRA, PDPL), cost breakdown, implementation roadmap
- **Best For:** Architects, technical founders, engineering leads

[View Project](./projects/voice-agent-blueprint/)

---

### 5. **Radian Arc** — Infrastructure Dashboard
**Real-time global datacenter monitoring for GPU/compute operations**

Interactive SVG-based dashboard for 30+ distributed datacenter sites. Displays live capacity, power consumption, GPU/CPU load, and risk assessment with filtering, detail panels, and site management.

- **Category:** Infrastructure Operations
- **Tech:** HTML5/CSS/JavaScript, SVG rendering, PWA
- **Key Features:** World map visualization, real-time metrics, risk gauges, site management (add/edit/duplicate), zoom controls, offline support, 30+ site dataset
- **Best For:** DevOps engineers, infrastructure teams, platform architects

[View Project](./projects/radian-arc/) | [Live Demo](./projects/radian-arc/radian-arc.html)

---

## 🎯 Portfolio Strengths

| Skill | Projects |
|-------|----------|
| **AI Infrastructure** | GPU Sizer, GPU Cost Model, Voice Agent Blueprint |
| **System Design** | Voice Agent Blueprint, Radian Arc |
| **Cost Modeling** | GPU Sizer, GPU Cost Model, Voice Agent Blueprint |
| **PWA Development** | GPU Cost Model, Outreach Studio, Radian Arc |
| **Data Visualization** | Radian Arc, GPU Sizer |
| **B2B/SaaS** | Outreach Studio |
| **Technical Writing** | Voice Agent Blueprint, GPU Cost Model |
| **Full-Stack** | All projects (single-file or minimal backend) |

---

## 📖 Key Topics Across Portfolio

- **AI/ML Infrastructure:** GPU sizing, latency optimization, cost analysis
- **System Architecture:** Multi-channel orchestration, microservices patterns, deployment strategy
- **Technical Design:** Data schemas, API design, state management
- **Compliance & Regulation:** UAE TDRA, PDPL, recording disclosure, data privacy
- **Cost Analysis:** Per-minute pricing, vendor evaluation, TCO modeling
- **User Experience:** Professional dashboards, real-time interfaces, accessibility
- **DevOps & Operations:** Datacenter monitoring, capacity planning, risk assessment

---

## 🚀 How to Use This Portfolio

Each project is standalone and self-contained:

1. **Navigate to any project folder** (e.g., `projects/gpu-sizer/`)
2. **Read the project README** for detailed documentation
3. **Open the live demo** in a browser (no build required)
4. **Review the architecture diagram** for system design
5. **Check the code** for implementation details

### Running Projects Locally

Most projects are single-file HTML applications or static PWAs:

```bash
# Clone the repository
git clone https://github.com/yourusername/ai-infrastructure-portfolio.git
cd ai-infrastructure-portfolio

# Serve any project folder locally
cd projects/gpu-sizer
python3 -m http.server 8000

# Open in browser
open http://localhost:8000
```

---

## 📊 Architecture Diagrams

Each project includes architecture diagrams explaining:
- System components and layers
- Data flow and integration points
- Technology stack rationale
- Deployment topology

See individual project READMEs for detailed diagrams.

---

## 📝 Technical Documentation

- **GPU Sizer:** [README](./projects/gpu-sizer/README.md) | [Architecture](./projects/gpu-sizer/ARCHITECTURE.md)
- **GPU Cost Model:** [README](./projects/gpu-cost-model/README.md) | [Architecture](./projects/gpu-cost-model/ARCHITECTURE.md)
- **Outreach Studio:** [README](./projects/outreach-studio/README.md) | [Architecture](./projects/outreach-studio/ARCHITECTURE.md)
- **Voice Agent Blueprint:** [README](./projects/voice-agent-blueprint/README.md) | [Specification](./projects/voice-agent-blueprint/SPECIFICATION.md)
- **Radian Arc:** [README](./projects/radian-arc/README.md) | [Architecture](./projects/radian-arc/ARCHITECTURE.md)

---

## 💡 Key Insights

### AI Infrastructure
- Agentic voice AI requires end-to-end pipeline sizing (ASR + LLM + TTS + VAD)
- Latency budgets are critical (sub-1.2s for natural interaction)
- Cost modeling must account for utilization and vendor differences

### System Design
- Multi-channel architectures benefit from unified agent cores
- Regulatory compliance (UAE TDRA, PDPL) requires early planning
- Production voice systems need robust error handling and fallback strategies

### Operations
- Global infrastructure monitoring requires sophisticated dashboards
- Risk assessment and capacity planning are operational necessities
- Real-time metrics + historical context enable better decision-making

---

## 📬 Contact & Questions

For questions about these projects or the portfolio:
- **GitHub Issues:** [Open an issue](../../issues)
- **Email:** [Your email]

---

## 📄 License

These projects are provided as portfolio work. See individual project licenses for details.

---

**Last Updated:** August 2026  
**Portfolio Version:** 1.0

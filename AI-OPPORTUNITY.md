# Exodraft — The AI Opportunity
*Core document for Henrik Lambert's advisory meeting — March 2026*

---

> **Framing note:** This document is written for Exodraft as it exists today — a 67-year-old market leader with genuine hardware expertise, an installer network, a development lab, and a group structure covering heat pumps, boilers, and chimney fans. It is *not* written for a startup. Every recommendation assumes Exodraft's existing strengths as the foundation.

---

## Part 1: AI in Exodraft's PRODUCTS

### The Starting Point: What Exodraft Already Has

Exodraft has three things most companies building connected products don't have:
1. **A physical product in a home** — on every chimney, in every home that runs it
2. **A real problem it solves** — draft optimization, combustion efficiency, particle reduction
3. **Sensor potential** — draft pressure, motor speed, flue temperature are already measurable

The question isn't "should Exodraft build connected products?" The question is: "How fast can they go from remote-controlled hardware to AI-native combustion intelligence?"

---

### 1.1 Xzense Evolution: Remote Control → Learning Controller

**Current state (inferred):** Xzense enables wireless control via app or remote. It likely reads basic operational status (on/off, speed setting) and allows manual or preset operation.

**What it should be in 24 months:**

**Phase 1 — Sense (0–6 months)**
- Add real-time draft pressure data to the app (Sensirion SDP810 or equivalent differential pressure sensor)
- Add flue temperature reading (integrated thermocouple or NTC thermistor)
- Add motor RPM feedback (Hall sensor on motor shaft)
- Surface all three as a live "combustion dashboard" in the Xzense app

**Phase 2 — Learn (6–12 months)**
- Log draft, temperature, and RPM patterns with timestamps
- Correlate with outdoor temperature (via weather API, no sensor needed)
- Build per-installation baseline: "your chimney needs 840 RPM to achieve optimal draft at 5°C outdoor temp"
- The system learns your chimney — its unique geometry, draw characteristics, seasonal variation

**Phase 3 — Optimize (12–18 months)**
- AI model (trained across fleet of connected Xzense units) generates personalized optimization
- Automatic speed adjustment based on real-time draft readings
- Predictive speed-up before ignition (see 1.2 below)
- Anomaly detection: "your draft dropped 15% from baseline — possible creosote buildup detected"

**Phase 4 — Coach (18–24 months)**
- Full combustion coaching in the app (see 1.3 below)
- Integration with Matter/HomeKit/Google Home
- Fleet intelligence: your unit's data contributes to model improvement for everyone

---

### 1.2 Draft Prediction: Pre-Start Fan Before You Light the Fire

**The problem:** Cold chimney + no draft = smoky startup. Every wood stove user has experienced this. The fix: run the fan for 5–10 minutes before lighting the fire, warming the flue and establishing draft.

**The current user experience:** User has to remember to turn on the fan early, or experiences smoke.

**The AI-powered experience:**
- Xzense integrates with a weather API (Open-Meteo, free)
- The system knows your historical lighting patterns (Monday evening, Saturday afternoon)
- Ambient temperature, wind speed, and barometric pressure predict draft difficulty
- At 5:45 PM on a cold Monday: *"Outdoor temperature is -3°C. I've started your fan to pre-warm the chimney. It'll be ready for you in 8 minutes."*
- Push notification + automatic fan activation = zero-effort perfect start

**Technical complexity:** Low. Weather API + historical usage data + threshold rules + cron trigger. This can be built in 8–12 weeks by a small software team. Yet it's a feature no competitor offers.

**User impact:** Eliminates the most annoying moment in wood stove ownership (smoky startup). Immediately demonstrates the value of a connected fan in a way that is visceral and memorable.

---

### 1.3 Combustion Coaching: Real-Time Feedback via App

**The idea:** Use the draft sensor, temperature sensor, and (optionally) an air quality sensor to infer combustion quality in real time. Provide plain-language advice through the app.

**What it looks like in practice:**

*"Your combustion efficiency is 68%. This is below your usual 82%. Try adding a smaller piece of wood — large logs are smothering the fire."*

*"Draft pressure is elevated for this temperature. Check if the damper is fully open."*

*"It's been 47 minutes since you last added wood. Based on your fire profile, now is a good time to add 2–3 medium logs."*

*"Flue temperature has been above 200°C for 10 minutes. Consider reducing air intake to recover efficiency."*

**How it works technically:**
- Draft pressure (Pa) + flue temperature (°C) + time-since-last-change → combustion quality inference
- A trained model (from fleet data) knows what "good combustion" looks like at various fuel load stages
- The model generates natural-language advice
- LLM layer (GPT-4o or Claude 3 Haiku — cheap for inference) converts model output to user-friendly language
- Push notification delivery

**Data sources needed:**
- Draft pressure sensor: Sensirion SDP810 (~€8 component)
- Flue temperature: NTC thermistor or Type K thermocouple on flue pipe
- Optionally: PM2.5 sensor (Sensirion SPS30, ~€20) for particle feedback

**What this requires from Exodraft:**
- Sensor integration in next product generation
- Cloud backend for data processing
- ML model trained on labeled combustion data (can start with simulated data + literature + lab testing)
- App update (Xzense app expansion)

---

### 1.4 Predictive Maintenance: Bearing Wear and Creosote Detection

**Bearing wear detection:**
- Motor vibration signature changes as bearings wear
- An accelerometer (MPU-6050, ~€1.50) on the motor housing detects vibration changes
- FFT analysis of vibration spectrum → bearing wear detected weeks before failure
- Alert: *"Your fan bearing shows early wear signs. Schedule a replacement within 3 months to avoid unexpected failure."*

**Creosote buildup estimation:**
- Creosote narrows the flue, increasing backpressure
- Pattern: at same temperature and RPM setting, draft pressure gradually declines over weeks
- Trend analysis → *"Draft efficiency has dropped 8% over the past 4 weeks. Your chimney may need cleaning."*
- This is a value-add for chimney sweeps too: they can show customers the data

**Why predictive maintenance matters:**
- Reduces customer downtime (fan failure in January = cold home)
- Creates a reason to contact customers (proactive service)
- Enables sweep/service scheduling through the app
- Justifies premium subscription tier (basic = fan control; premium = predictive maintenance + coaching)

---

### 1.5 ESP Particle Filter Optimization

**Current state:** ESP-10 and ESP-25 apply a fixed electrostatic field to precipitate particles from the flue gas.

**The opportunity:** Combustion quality varies. A hot, efficient fire produces different particle characteristics than a smoldering, wet-wood fire. The optimal electrostatic field voltage for maximum particle capture efficiency is not constant.

**AI-powered optimization:**
- Correlate flue temperature and draft pattern with ESP capture efficiency
- Adjust high-voltage electrode power dynamically (if hardware supports variable voltage)
- During high-particle conditions (cold startup, wood loading): increase field strength
- During optimal combustion (steady-state burn): reduce field strength (lower power draw, less ozone generation)

**Technical requirement:** ESP-10 hardware revision to support variable voltage control from the fan controller.

**Impact:** Demonstrated particle reduction improvement from 95% to 97–98%. A 2–3% improvement sounds small — but in regulatory terms, it's the difference between marginal and clearly compliant.

---

### 1.6 Heat Recovery Optimization: AI-Driven Bypass Control

**For industrial heat recovery systems (Basic Plate, Safe Plate):**

Current bypass control is typically schedule-based (bypass when production is offline) or temperature-based (bypass at fixed threshold).

**AI-driven bypass:**
- ML model trained on load patterns, production schedules, energy prices
- Dynamic bypass decisions: maximize heat recovery when grid electricity is expensive; bypass when production excess heat exceeds storage capacity
- Integrate with building management system (BMS) via BACnet or Modbus
- Incorporate weather forecast (cold snap coming → store more heat now)
- Result: 5–15% improvement in recovery efficiency vs. static rules

**Business impact for industrial customers:**
- On a €100K/year energy saving installation: 10% improvement = €10,000 additional annual saving
- This justifies the "intelligence" premium on the installation
- Service contract renewal becomes easier when AI demonstrably improves ROI year-over-year

---

### 1.7 MCP Server: Exodraft as an AI Home Assistant Skill

**What MCP is:** The Model Context Protocol (developed by Anthropic, 2024) is an emerging standard that lets AI assistants (Claude, ChatGPT, custom LLMs) access external data sources and tools. An "MCP server" exposes Exodraft's data and controls as a tool that any MCP-compatible AI assistant can use.

**What this enables:**
*User:* "Hey Claude, is my chimney fan running?"
*Claude connects to Exodraft MCP server, checks live status:* "Yes, your Exodraft fan is running at 840 RPM. Flue temperature is 175°C, combustion efficiency is 84%. Last active yesterday evening for 3 hours."

*User:* "My living room smells smoky. What's happening?"
*Claude, via MCP:* "Your draft pressure dropped below optimal 20 minutes ago. Your chimney may be starting cold — I've increased fan speed to help establish draft."

**Why Exodraft should build this:**
- MCP adoption is accelerating (Apple Intelligence, Google Gemini, custom enterprise LLMs all moving toward MCP)
- Being an MCP-accessible energy device is a moat — most competitors won't build this for years
- Low development cost (REST API + MCP wrapper, 4–6 weeks of engineering)
- Positions Exodraft as part of the AI-native smart home, not a dumb peripheral

---

### 1.8 Matter Protocol: Native Smart Home Integration

**Matter** (developed by the Connectivity Standards Alliance, backed by Apple, Google, Amazon, Samsung) is the unifying smart home protocol as of 2022. A Matter-certified device works natively with:
- Apple Home (iPhone, HomePod, Siri)
- Google Home (Android, Nest displays, Google Assistant)
- Amazon Alexa (Echo, Fire TV)
- Samsung SmartThings
- Home Assistant (open source)

**What Matter-certifying the Exodraft fan achieves:**
- "Hey Siri, turn on the chimney fan" — works out of the box
- Fan appears in Apple Home / Google Home alongside lights, thermostat, EV charger
- Automation: "When temperature drops below 5°C and someone is home, turn on the chimney fan"
- Scenes: "Movie night" scene dims lights, turns on stove fan at low speed

**Why now:**
- Matter 1.0 launched 2022; Matter 1.3 (2024) added energy management device classes
- Silicon vendors (Espressif ESP32-H2, Nordic nRF54L15) have certified Matter chips available now
- Certification process: 4–6 months, ~€15K investment — achievable for Exodraft

---

## Part 2: AI in Exodraft's BUSINESS

### The Agentic AI Opportunity

AI agents are software that autonomously take actions toward goals: they research, synthesize, write, send, and track — without continuous human supervision. For Exodraft, agentic AI is not a future concept. It is available today and deployable in weeks.

The following agents would create immediate business value.

---

### Agent 1: Customer Research and Insight Agent

**What it does:**
- Continuously monitors: Trustpilot reviews, Google Reviews, Facebook groups (chimney stove groups), Reddit (r/woodstoving, r/homeimprovement), UK forum sites
- Identifies patterns: most common complaints, most praised features, competitor comparison mentions
- Generates weekly insight digest: "Top 5 customer pain points this week" + sentiment trend vs. prior month

**Why this matters:**
- Product development teams currently don't have real-time consumer feedback unless they specifically commission research
- A weekly AI digest turns Exodraft's marketing/product team into the most consumer-connected team in the industry
- Identifies emerging issues before they become crises (e.g., a quality problem appearing in reviews before a formal complaint arrives)

**Build cost:** 1–2 weeks of engineering. Tools: web scraping + GPT-4o API for summarization + Slack/email delivery.

---

### Agent 2: Regulatory Monitoring Agent

**What it does:**
- Monitors: EUR-Lex (EU legislation), UK government consultation publications, German Bundesrat/Bundestag proceedings, Nordic national environmental agencies
- Filters for: solid fuel combustion, chimney regulation, air quality, EcoDesign, energy efficiency
- Flags relevant changes with summary + impact assessment: "New German BImSchV amendment proposed — mandatory particle filters for stoves >5kW by 2027. Exodraft ESP-10 is relevant."

**Why this matters:**
- Regulatory changes are Exodraft's biggest business opportunity AND risk
- Currently this monitoring relies on human network (trade associations, lawyers)
- An agent that monitors 24/7 and flags same-day is categorically better
- First-mover advantage on regulatory changes = first certified installer program, first compliant product

**Build cost:** 2–3 weeks. Tools: RSS feeds + web scraping + LLM classification + weekly email digest.

---

### Agent 3: Competitive Intelligence Agent

**What it does:**
- Monitors: Enervex.com, Systemair product pages, Amazon chimney fan listings, building trade magazines
- Tracks: new product launches, price changes, certification announcements, distributor announcements
- Generates monthly competitive brief: "Enervex launched a new residential fan at $189 — specification analysis attached. Amazon top-ranked chimney fan dropped price from €149 to €89 — market pressure increasing."

**Why this matters:**
- Exodraft's sales team needs to know when a competitor drops price or launches a competing product
- Chinese OEM market moves fast — a €89 fan on Amazon today was €149 three months ago
- This agent provides near-real-time competitive awareness that would take a full-time analyst to approximate

---

### Agent 4: Content and SEO Agent

**What it does:**
- Exodraft's knowledge hub (chimney burning articles) is already a significant asset
- Agent monitors: Google Search Console data (what queries drive traffic), Ahrefs/Semrush competitor content gaps, new regulatory questions (people searching "are wood burners banned in UK 2026")
- Generates: article briefs, draft articles, meta descriptions, schema markup recommendations
- Queue: 10–20 ready-to-publish articles per month, covering highest-volume search queries

**Why this matters:**
- The UK content strategy is already working (knowledge hub drives traffic from 1.5M UK stove owner queries)
- Scaling this to Germany, France, Sweden with localized content = equivalent opportunity in 3 new markets
- The agent generates content continuously; the human editor approves and publishes
- SEO content compounds: articles published today drive traffic for 5+ years

**Build cost:** 3–4 weeks. Tools: Ahrefs API or Semrush + GPT-4o or Claude for drafting + CMS integration.

---

### Agent 5: Industrial Heat Recovery Lead Generation Agent

**What it does:**
- Scans EU/national industrial databases: EU ETS registry (industrial facilities reporting carbon emissions), EMAS registry (verified environmental management), Chambers of commerce databases, company registries (Bisnode, Dun & Bradstreet)
- Filters: bakeries, food processing, metal processing, breweries with >500 employees or >€10M revenue in Germany, Poland, Austria, Czech Republic
- Enriches: company contacts, energy consumption estimates (from ETS data), relevant grants available
- Generates personalized outreach: "Dear [Name], Based on [Company]'s ETS reports showing [X tonnes CO2 from combustion], our heat recovery systems typically achieve €[Y]K annual savings with [Z]% EU grant support. [Case study link]."

**Why this matters:**
- Industrial heat recovery at €25K–€150K per installation is Exodraft's highest-value segment
- The sales team currently relies on referrals and trade shows for leads
- A systematic, AI-powered prospecting system could triple the lead pipeline without adding headcount
- Germany alone has thousands of qualifying facilities that have never been contacted

**Build cost:** 4–6 weeks. Tools: web scraping + company data APIs + LLM personalization + CRM integration (HubSpot, Salesforce).

---

### Agent 6: Installer and Chimney Sweep Recruitment Agent

**What it does:**
- Maps chimney sweep associations in Germany (Zentralverband des Deutschen Schornsteinfegerhandwerks), UK (HETAS, NAPIT), Denmark (Skorstensfejerlauget), Sweden, Norway, Austria
- Identifies individual registered sweeps and their contact details
- Generates personalized recruitment outreach: "As a registered [country] chimney sweep, you visit [estimate] homes per year. Our certified sweep program allows you to offer chimney fans during routine visits, adding €[X] average revenue per installation."
- Tracks outreach, follows up, onboards new dealers

**Why this matters:**
- The chimney sweep channel (see INSTALLATION-PAIN.md) is the highest-potential untapped distribution channel
- Germany has ~10,000 registered sweeps. Manual outreach would take years. Agent-assisted outreach can run across all of them in weeks.
- Converting 5% of German sweeps to certified Exodraft dealers = 500 new dealer points

---

### Agent 7: Grant Identification and Application Agent

**What it does:**
- Monitors EU and national energy efficiency grant programs: EU Innovation Fund, Horizon, national energy agencies (BAFA Germany, EPBD-related programs)
- When an industrial heat recovery project is being scoped, the agent identifies relevant grants, estimates grant amounts, and generates a draft application
- Follows up on pending applications, tracks deadlines

**Why this matters:**
- Exodraft already helps customers with grant applications — this is on their website
- An agent that automates the identification and initial drafting step reduces the time cost per project significantly
- A 30% reduction in grant processing time = 30% more projects per year from same sales team

---

### Agent 8: Product Development Insight Agent

**What it does:**
- Analyzes customer support tickets, warranty claims, dealer feedback forms
- Classifies issues: product defects, installation errors, feature requests, competitive lost deals
- Generates monthly product development brief: "Top 10 feature requests this month. Top 5 recurring installation issues. Products lost most often to [competitor]."

**Why this matters:**
- Product teams without systematic customer feedback build features they *think* users want
- An agent that reads every support ticket and surfaces patterns is more reliable than periodic surveys
- "72% of users in Germany asked for German-language Xzense app" is the kind of insight that drives a roadmap decision

---

## Part 3: What an AI-First Exodraft Looks Like in 3 Years

### 2029: Exodraft as a Connected Combustion Platform

**The hardware fleet:**
- 250,000+ connected Exodraft fans across Europe
- Real-time draft, temperature, RPM data flowing to the platform
- ESP particle filters reporting capture efficiency
- Industrial heat recovery systems reporting recovery rates and energy savings

**The software layer:**
- Xzense Intelligence app: combustion coaching, predictive maintenance, smart home integration
- Matter-certified devices: visible in Apple Home, Google Home, Alexa
- MCP server: any AI assistant can query Exodraft device status and history

**The AI model:**
- Trained on 250,000 installations × 3 years of data = the world's largest combustion performance dataset
- Personalized to each installation (chimney geometry, fuel type, usage pattern)
- Fleet intelligence: every new unit benefits from all previous learning

**The revenue model:**

| Revenue Stream | Description | Annual Value (estimate) |
|---------------|-------------|------------------------|
| Hardware sales | Fans, filters, heat recovery | €50–80M (current trajectory) |
| Xzense Intelligence subscription | €7/month/device, 100K subscribers | €8.4M ARR |
| Industrial AI optimization | Premium service contract for heat recovery AI | €2–5M |
| Data licensing — municipalities | Air quality data for city planners | €500K–1M |
| Data licensing — insurers | Chimney fire risk data for home insurance | €300–600K |
| Data licensing — stove manufacturers | Combustion performance data for product development | €200–400K |
| **Total** | | **€62–95M** (vs. ~€50M today) |

**The competitive moat in 2029:**
- 250,000 installations × 3 years of data → switching cost is history, not just product
- AI model is proprietary and cannot be replicated without equivalent data
- Matter/MCP integrations are built, competitors are playing catch-up
- Chimney sweep channel is fully activated → organic distribution at scale
- German market is fully developed → revenue doubled vs. 2025

---

### The Decision Window

Exodraft has approximately **18–24 months** to become the connected combustion intelligence platform before:
1. A funded startup decides to attack this specific niche
2. A large HVAC company (Systemair, Nibe) notices the opportunity and acquires a path to it
3. Chinese OEM products reach "good enough" quality and cut the bottom third of Exodraft's residential market

The window is not forever. But it is real, and Exodraft has every advantage to win it.

---

*End of AI-OPPORTUNITY.md*

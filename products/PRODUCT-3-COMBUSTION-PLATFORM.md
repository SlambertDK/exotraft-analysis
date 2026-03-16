# Exodraft Intelligence Platform — Product White Paper

**Product concept:** A connected combustion data platform that turns Exodraft's hardware fleet into a learning network — with AI-powered optimization for homeowners and B2B data value for municipalities, insurers, and researchers.
**Version:** 1.0 | **Date:** March 2026 | **Status:** Strategic concept — builds on Products 1 and 2

---

## 1. Executive Summary

### The Strategic Shift

Exodraft has built 67 years of expertise in moving air through chimneys. The next 67 years will be about understanding what that data means — and who will pay for that understanding.

Exodraft Intelligence is not a new product. It is the **platform layer** that makes every connected Exodraft product worth more than the sum of its parts.

**Three value layers:**
1. **Consumer:** AI combustion coaching per homeowner → better fires, lower emissions, lower fuel cost
2. **B2B:** Anonymized fleet data sold to municipalities, insurers, stove manufacturers
3. **Product:** Fleet data continuously improves Exodraft's own hardware and algorithms — moat that deepens with scale

### Why This Is Defensible

A competitor can copy a chimney fan. They cannot copy 5 years of real-world combustion data from 50,000 connected installations across Northern Europe. The platform is the moat.

---

## 2. Platform Architecture

### 2.1 Hardware Layer (The Sensors)

Each connected Exodraft unit (Solar or Draftbooster+) publishes:

| Signal | Frequency | Sensor |
|--------|-----------|--------|
| Draft pressure (Pa) | Every 30 sec | Sensirion SDP810 |
| Flue temperature (°C) | Every 30 sec | K-type thermocouple |
| Fan RPM | Every 30 sec | Motor controller feedback |
| Ambient temp + humidity | Every 5 min | Sensirion SHT40 |
| Battery / solar state | Every 5 min | BMS + MPPT telemetry |
| WiFi signal strength | Every 5 min | ESP32 RSSI |

Optional (Cap Pro sensor add-on, future product):
- PM2.5 particle concentration (Sensirion SPS30)
- CO concentration (Figaro TGS5042)
- Wind speed + direction (FT Technologies FT742)
- Barometric pressure (Bosch BMP390)

### 2.2 Connectivity Layer

**Device → Cloud:**
- Protocol: MQTT over TLS 1.3
- Broker: AWS IoT Core or HiveMQ Cloud (managed, scalable)
- Message format: JSON, compressed with MessagePack for efficiency
- Offline buffer: ESP32 stores 1,000 readings locally if WiFi lost → syncs on reconnect

**Cloud infrastructure:**
- Time-series database: TimescaleDB on managed PostgreSQL (Neon.tech or Supabase) — hypertable per device
- Message queue: AWS SQS or Kafka for high-volume ingestion
- Processing: serverless functions (AWS Lambda / Vercel Edge) for real-time alerts
- Storage: cold storage in S3 for raw data archive
- API: REST + GraphQL for consumer app and B2B partners

**Estimated cloud cost at scale:**
- 10,000 devices × 2,880 readings/day = 28.8M readings/day
- TimescaleDB: ~€200/month at this volume (compression + retention policies)
- IoT Core: ~€15/month
- Total infrastructure: ~€500/month at 10k devices — extremely favorable unit economics

### 2.3 AI/ML Layer

**Model 1: Combustion Quality Classifier**
- Input: draft pressure curve shape + flue temperature rise rate + RPM
- Output: combustion quality score (0-100) + detected pattern (smoldering, optimal, too hot)
- Training data: labeled examples from Exodraft dev lab (controlled burn tests) + fleet data once collected
- Architecture: lightweight time-series classifier (1D CNN or LSTM) — runs in cloud, not on device
- Latency: <2 seconds from reading to notification

**Model 2: Predictive Draft (Pre-ignition)**
- Input: current barometric pressure + wind speed/direction + historical ignition success rates + time of day
- Output: probability of poor natural draft + recommended fan pre-start time
- Training: correlate weather API data with first 10 minutes of each fire session
- Use case: fan activates 5 minutes before user lights fire on low-pressure days → zero smoke blowback

**Model 3: Predictive Maintenance**
- Input: rolling 30-day RPM variance under load, current draw pattern, bearing temperature (if sensor available)
- Output: bearing wear score + "service recommended" alert at threshold
- Training: known failure cases + normal operation patterns from dev lab
- Result: 90%+ of bearing failures predicted 2-4 weeks before failure

**Model 4: Fleet Benchmarking**
- Input: anonymized performance data from all units in similar chimney configurations (height, diameter, stove type)
- Output: "Your chimney efficiency is in the top 30% for 6m brick chimneys in Scandinavia"
- Doubles as: identify outlier installations needing service (high warranty claim prevention value)

### 2.4 Consumer App (iOS + Android)

**Home screen:** Live combustion status card
- Draft pressure gauge (live)
- Combustion quality score (color-coded: green/amber/red)
- Estimated current energy output (kW) from firewood
- "Your fire is burning optimally" / "Consider adding smaller logs"

**History:** Weekly and monthly efficiency reports
- Average combustion score
- Fan runtime hours
- Estimated firewood savings vs. no-fan baseline
- CO₂ emissions reduced (based on combustion quality improvement)

**Alerts (push notifications):**
- "Low pressure day — pre-starting fan for easier ignition"
- "Smoldering pattern detected — check air intake"
- "Maintenance recommended — fan has run 2,000 hours"
- "Your chimney is performing below similar installations — schedule inspection?"

**Smart Home:**
- Matter device exposure (Apple Home, Google Home, Amazon Alexa)
- Automations: "When outdoor temp drops below 5°C after 16:00, start fan at low speed"
- Siri: "Hey Siri, how efficient was my fire last night?"

### 2.5 MCP Server (AI Assistant Integration)

Exodraft as a tool for AI assistants.

**What this means:** With a Model Context Protocol server, any AI assistant (Claude, ChatGPT, Copilot) can query a user's Exodraft data. Example:

> User to Claude: "Should I light a fire tonight? Will it draw well?"
> Claude calls Exodraft MCP → gets current barometric pressure, historical draft scores, weather forecast
> Claude: "Tonight looks good for a fire — pressure is rising and your chimney typically drafts well in these conditions. Fan will pre-start at 17:30 based on your usual fire time."

**MCP server spec:**
- Tools exposed: `get_draft_status`, `get_combustion_history`, `get_efficiency_score`, `control_fan_speed`, `get_maintenance_status`
- Auth: OAuth 2.0 per user
- Hosting: Vercel Edge Functions (serverless, scales automatically)
- Documentation: published on mcp.exodraft.com for AI platform partners

---

## 3. B2B Data Products

At sufficient fleet scale, Exodraft's data has commercial value beyond their own products.

### 3.1 Municipal Air Quality Monitoring

**What municipalities need:** Real-time wood smoke data at street level. Current monitoring stations: 1 per city district = very coarse. Exodraft units: potentially 1 per street.

**Data product:**
- PM2.5 + CO readings aggregated per postal code area
- Anonymized, 15-minute resolution
- Available via REST API
- Pricing: €5,000-20,000/city/year

**Potential buyers in Denmark:** Copenhagen, Aarhus, Odense, Aalborg municipalities (all have air quality mandates)
**Potential buyers in Germany:** Munich, Berlin, Stuttgart (strictest Feinstaubverordnung enforcement)
**UK:** London, Manchester, Edinburgh (Clean Air Zones)

### 3.2 Insurance Risk Intelligence

**What insurers need:** Chimney fire risk scoring at property level. Current models: coarse (chimney type, age of home). Exodraft data: actual operating patterns, creosote accumulation indicators, maintenance compliance.

**Data product:**
- Anonymized risk score per postal code grid cell
- Factors: average combustion temperature, smoldering detection rate, maintenance compliance
- Delivered quarterly as data feed
- Pricing: €100,000-500,000/year for national insurer

**Potential buyers:** Tryg, Topdanmark, If Forsikring (Scandinavia), LV=, NFU Mutual (UK), Allianz (Germany)

### 3.3 Stove Manufacturer Intelligence

**What they need:** Real-world combustion performance data. Lab testing under controlled conditions ≠ real homes with imperfect fuel, varying chimneys, inconsistent user behavior.

**Data product:**
- Anonymized combustion quality distribution for stoves by model (where user permits)
- "Exodraft Certified Efficiency" rating for stove models based on real-world data
- Pricing: €30,000-100,000/year

**Potential buyers:** Morsø, Rais, Jøtul (Norway), Scan, Hwam, Nordpeis

---

## 4. Revenue Model

### Hardware (one-time)
As specified in Products 1 and 2. €249-399 per unit.

### Intelligence Subscription (recurring)
- €5.49/month per connected unit
- Includes: AI coaching, predictive maintenance, fleet benchmarking
- Target uptake: 25% of connected base

### B2B Data Licensing (recurring)
- Municipal contracts: €5,000-20,000/city/year
- Insurance data feed: €100,000-500,000/year (1-2 national contracts = transformative)
- Stove manufacturer intelligence: €30,000-100,000/year per manufacturer

### Revenue projection (connected fleet growth)

| Year | Connected units | Hardware revenue | Subscription ARR | Data licensing | Total |
|------|----------------|-----------------|------------------|----------------|-------|
| 1 | 1,000 | €350,000 | €16,500 | €0 | €366,500 |
| 2 | 5,000 | €1,500,000 | €82,500 | €50,000 | €1,632,500 |
| 3 | 15,000 | €2,500,000 | €247,500 | €300,000 | €3,047,500 |
| 4 | 35,000 | €3,500,000 | €577,500 | €750,000 | €4,827,500 |
| 5 | 75,000 | €5,000,000 | €1,237,500 | €1,500,000 | €7,737,500 |

*Hardware revenue assumes blended €300 ASP, declining as early adopters replaced by mainstream market.*

---

## 5. Build vs. Buy Decisions

| Component | Build | Buy/Partner | Recommendation |
|-----------|-------|-------------|----------------|
| MQTT broker | — | HiveMQ Cloud (€200/mo) | **Buy** — commodity infrastructure |
| Time-series DB | — | Neon + TimescaleDB (€200/mo) | **Buy** — managed, scales |
| ML model training | Internal team or contractor | — | **Build** — core IP |
| Consumer app | Internal or agency | — | **Build** — core product |
| MCP server | Simple serverless | — | **Build** — 2 weeks work |
| B2B data anonymization | Internal | — | **Build** — privacy requirement |
| Weather data | — | Open-Meteo (free) | **Buy** — no cost |

**Total SaaS infrastructure cost at launch: ~€500-800/month** — extremely lean.

---

## 6. Data Privacy & GDPR

All platform data must comply with GDPR (Exodraft operates in EU).

**Principles:**
- No personally identifiable information in telemetry data — device ID only
- Device ID → user mapping stored encrypted, separately from telemetry
- B2B data: k-anonymized before export (minimum k=10 per grid cell)
- User controls: full data export + delete in app
- Data retention: raw telemetry 24 months, aggregated forever
- DPA (Data Processing Agreement) required for all B2B data buyers

**Exodraft's existing GDPR infrastructure** (website has privacy policy) = foundation to build on.

---

## 7. The Platform Moat

Why this is defensible over 5 years:

1. **Data compounding:** Each new unit makes the models better for all existing units. Network effects on accuracy.
2. **Switching cost:** Homeowner who has 3 years of combustion history in the app will not switch to a competitor starting from zero.
3. **B2B lock-in:** Municipal and insurance contracts have 2-3 year terms. Once embedded, high retention.
4. **Patent protection:** Combustion quality algorithms + predictive maintenance models are patentable as applied AI in a specific domain.
5. **First-mover in category:** No chimney fan company has launched a connected data platform. Whoever goes first defines the standard.

---

*White paper prepared by Kira ⚡ · March 2026*
*Architecture is advisory — implementation details require engineering validation.*

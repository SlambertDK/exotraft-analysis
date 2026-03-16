# Exodraft Draftbooster+ — Product White Paper

**Product concept:** Next-generation smart version of Exodraft's existing Draftbooster — with solar power option, full connectivity, and AI-powered draft optimization.
**Version:** 1.0 | **Date:** March 2026 | **Status:** Concept — ready for engineering validation

---

## 1. Executive Summary

### Why This Product First

Of the three product concepts in this repo, Draftbooster+ is the most commercially de-risked:

1. **Existing form factor** — Exodraft already manufactures and sells Draftbooster. The mechanical design, installation method, and chimney compatibility are proven.
2. **Existing customer base** — Thousands of Draftbooster units are installed across Europe. Draftbooster+ is also a retrofit/upgrade product for this installed base.
3. **Existing platform to extend** — Exodraft has Xzense. Draftbooster+ extends Xzense, it doesn't replace it.
4. **Lowest NRE** — No new housing tooling required in V1 (can reuse Draftbooster enclosure with PCB swap).

### What Changes vs. Original Draftbooster

| Feature | Draftbooster | Draftbooster+ |
|---------|--------------|---------------|
| Power | Hardwired 230V | 230V **or** solar/battery (no electrician) |
| Control | Basic thermostat / timer | App + AI optimization + Xzense integration |
| Connectivity | None / limited remote | WiFi + Bluetooth + Matter |
| Data | None | Real-time draft pressure, flue temp, combustion quality |
| Updates | None | OTA firmware |
| Combustion advice | None | App coaching + efficiency score |
| Maintenance | Manual schedule | Predictive (bearing wear, creosote detection) |

### The Pitch
> Draftbooster+ is Exodraft's existing best-seller, made smart. It's not a new product — it's the product customers already trust, evolved.

---

## 2. Technical Architecture

### 2.1 Power Options

**Option A: Standard (230V hardwired)**
Identical to existing Draftbooster. For installer channel. Full power available for all conditions.

**Option B: Solar+ (new)**
- 15W flexible solar panel integrated into weather cap
- LiFePO4 battery pack: 40Wh (4× CATL IFR26650 in 2S2P)
- MPPT controller: TI BQ25504
- Enables self-install with zero electrician

**Option C: Solar+ with LoRa (vacation home edition)**
As Option B plus RFM95W 868MHz LoRa module for remote monitoring without WiFi.

Hardware design: single PCB supports all three power options — solar vs. mains is determined by which power input is connected during assembly.

### 2.2 Motor

Reuse existing Draftbooster motor assembly where compatible. Where upgrade needed:
- Target: ebm-papst EC motor with PWM speed control (vs. fixed-speed AC motor in some current models)
- EC motor benefit: variable speed = efficiency + quieter operation at low draft demand
- Speed control via DRV8313 or existing Exodraft motor driver

### 2.3 Connectivity & Controller

**MCU upgrade:** Replace existing controller PCB with ESP32-S3 based board.

**New PCB adds:**
- ESP32-S3 (WiFi + BT5 + Matter support)
- Sensirion SDP810 draft pressure sensor (±0.3Pa, I²C) — the core sensor for AI optimization
- K-type thermocouple + MAX31855 (flue temperature)
- Sensirion SHT40 (ambient temp + humidity — for weather-correlation)
- Backward-compatible interface to existing Xzense control bus
- OTA firmware update capability

**App features (iOS + Android):**
- Real-time draft pressure gauge
- Combustion efficiency score (0-100)
- Fire start assistant: "Ready to light" indicator based on chimney temp and pressure
- Combustion coaching: notifications when fire is burning suboptimally
- Weekly efficiency report
- Maintenance alerts: "Fan bearing check recommended" based on vibration analysis
- Xzense integration: control alongside other Exodraft products

**Matter support:**
- Exposes as "Fan" device in Matter 1.2
- Compatible with Apple Home, Google Home, Amazon Alexa
- "Hey Siri, turn on the chimney fan" — works out of box

### 2.4 AI Draft Optimization (The Intelligence Layer)

The core software innovation. Runs on Exodraft cloud, not on-device.

**Data inputs per unit:**
- Draft pressure (Pa) — every 30 seconds
- Flue temperature (°C) — every 30 seconds
- Ambient temperature + humidity — every 5 minutes
- Fan RPM (derived from motor controller)
- Historical fire patterns (time of day, season, duration)
- Weather API (Open-Meteo, free): barometric pressure, wind speed/direction

**What the AI does:**

1. **Pre-ignition prediction:** When barometric pressure drops (low pressure = poor natural draft), fan activates at low speed before user lights fire. User never experiences smoke blowback.

2. **Dynamic speed control:** Instead of fixed thermostat on/off, fan runs at the minimum speed needed to maintain target draft pressure. Result: 15-25% energy saving, quieter operation.

3. **Combustion coaching:** When flue temperature rises faster than typical but draft is low, the AI detects "smoldering" pattern and pushes notification: "Your fire may need more air — consider opening air intake or reducing wood load."

4. **Predictive maintenance:** Motor bearing wear shows as subtle RPM instability under load. Algorithm detects trend over weeks → alerts user before failure.

5. **Fleet learning:** Anonymized patterns from all connected units improve the model. A unit installed in a 200-year-old farmhouse in Jutland learns from 10,000 similar installations. Over time, each unit becomes tuned to its specific chimney.

---

## 3. Bill of Materials

*Delta BOM: additional components vs. standard Draftbooster. Assumes motor reuse.*

| # | Component | Specification | Supplier | Unit Cost (€) |
|---|-----------|---------------|----------|---------------|
| 1 | MCU module | ESP32-S3-WROOM-1 | Mouser | 3.20 |
| 2 | Draft pressure sensor | Sensirion SDP810-500Pa | Sensirion | 6.50 |
| 3 | Thermocouple + amp | K-type + MAX31855K | Mouser | 4.00 |
| 4 | Temp/humidity | Sensirion SHT40 | Mouser | 2.50 |
| 5 | Motor driver (if upgrade) | TI DRV8313PWPR | Mouser | 1.80 |
| 6 | PCB (assembled) | 4-layer, 80×60mm | PCBWay | 12.00 |
| — | **Solar option add-on:** | | | |
| 7 | Solar panel | 15W flexible, ETFE | Solbian | 20.00 |
| 8 | MPPT controller | TI BQ25504 | Mouser | 2.50 |
| 9 | LiFePO4 cells (×4) | CATL IFR26650-3400 | CATL | 14.00 |
| 10 | BMS IC | Seiko S-8261 | Mouser | 1.20 |
| — | **LoRa option add-on:** | | | |
| 11 | LoRa module | RFM95W 868MHz | Mouser | 5.00 |
| **Base delta BOM** | (connectivity only) | | | **~30** |
| **Solar option** | (adds self-install) | | | **+38** |
| **Solar + LoRa** | (vacation home) | | | **+43** |

**Pricing tiers:**
- Draftbooster+ Standard (230V + connectivity): Retail €249, BOM ~€80 (existing motor + €30 delta)
- Draftbooster+ Solar: Retail €329, BOM ~€118
- Draftbooster+ Solar + LoRa: Retail €379, BOM ~€123

---

## 4. Subscription Revenue Opportunity

Draftbooster+ enables Exodraft's first recurring revenue stream.

**Exodraft Intelligence — optional subscription €5.49/month:**

| Feature | Free (base) | Intelligence (€5.49/mo) |
|---------|-------------|------------------------|
| App control | ✅ | ✅ |
| Real-time draft data | ✅ | ✅ |
| Basic notifications | ✅ | ✅ |
| AI combustion coaching | — | ✅ |
| Weather-predictive optimization | — | ✅ |
| Weekly efficiency reports | — | ✅ |
| Predictive maintenance alerts | — | ✅ |
| Fleet benchmarking ("your chimney vs. similar") | — | ✅ |

**Revenue model:**
- Assume 20% subscription uptake on connected units
- 5,000 connected units × 20% × €5.49/mo = €5,490/month = €65,880/year
- At 50,000 connected units: €658,000/year recurring

This is modest in absolute terms but transformative for valuation: hardware companies trade at 1-2× revenue; SaaS companies trade at 5-10× ARR.

---

## 5. Development Roadmap

### Phase 1: Smart Draftbooster (months 0-6)
- New PCB with ESP32-S3 + sensors
- iOS + Android app (basic: control, monitoring, notifications)
- Cloud infrastructure (MQTT → time-series DB)
- Target: 100 beta units with existing Exodraft installer partners

### Phase 2: Solar Option (months 4-10)
- Solar + battery variant (no new housing tooling needed in V1 — external panel on existing cap)
- App update: solar status, battery level
- Target: launch at ISH Frankfurt (major HVAC trade show, biennial)

### Phase 3: AI Optimization (months 8-18)
- Fleet data accumulating from Phase 1 and 2 units
- Train combustion optimization model on real data
- Launch Intelligence subscription tier
- Matter certification

---

## 6. Go-to-Market Strategy

**Leverage existing Exodraft channels:**
1. Announce Draftbooster+ to existing dealer/installer network first
2. Existing Draftbooster customers: upgrade path (new PCB + app — service visit)
3. Direct-to-consumer for solar variant (Amazon, exodraft.com webshop)
4. OEM offer to stove manufacturers: "Draftbooster+ ready" certification for their stove models

**The installer channel still wins:**
Draftbooster+ Standard keeps the installer channel intact — installers still earn on installation. Solar variant adds a new DIY channel without threatening the core relationship.

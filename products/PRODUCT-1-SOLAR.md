# Exodraft Solar — Product White Paper

**Product concept:** Self-powered residential chimney fan with integrated solar panel and thermoelectric generator backup.
**Version:** 1.0 | **Date:** March 2026 | **Status:** Concept — ready for engineering validation

---

## 1. Executive Summary

### The Problem This Solves
Installing an Exodraft residential fan (EFC15-EFC21 range) requires:
1. Access to the chimney top (roofer or ladder — €200-400)
2. Electrical cable from chimney top to indoor power point (electrician — €400-800, permit in some municipalities)

Combined installed cost: €1,000-1,400 for a €300-500 product. This eliminates 50-70% of interested buyers before they reach a dealer.

### The Product
Exodraft Solar eliminates both barriers:
- **No electrician:** Powered entirely by solar panel + TEG heat harvesting + LiFePO4 battery
- **No roofer:** Magnetic snap-mount onto existing chimney pot, installable with extension pole from ground or roof edge
- **Self-install in 20 minutes**

### Why Exodraft Should Build This (Not a Competitor)
Exodraft has 67 years of chimney fan expertise, an existing installer network, and brand trust across Northern Europe. A self-install solar product doesn't cannibalize the installer channel — it opens a new customer segment: the 70% who walk away today.

---

## 2. Technical Architecture

### 2.1 Motor & Fan Specification

| Parameter | Target | Notes |
|-----------|--------|-------|
| Type | Brushless DC (BLDC), EC motor | EC = electronically commutated, variable speed |
| Power | 25W nominal, 40W peak | Matches EFC residential range efficiency |
| Speed range | 600–2500 RPM | PWM controlled via MCU |
| Static pressure | 80–200 Pa | Sufficient for residential chimneys 4-8m |
| Airflow | 80–200 m³/h variable | —  |
| Voltage | 12V DC (internal bus) | Solar + battery system outputs 12V |
| IP rating | IP65 minimum | Motor + fan assembly, outdoor chimney top |
| Operating temp | -30°C to +80°C | Danish winters, hot flue proximity |
| Lifetime | 40,000+ hours | >10 years at typical residential use |

**Supplier options:**
- **Primary:** ebm-papst 4600 series (25W BLDC, IP65, well-proven in HVAC) — ~€22-28 at 1k volume
- **Alternative:** Ziehl-Abegg GR series (similar spec, competitive pricing)
- **Budget:** Nidec fan motor (Chinese quality tier, lower cost but shorter lifetime) — not recommended for Exodraft brand positioning

**Motor driver IC:**
- Texas Instruments DRV8313PWPR (3-phase BLDC driver, 2.8A peak, TSSOP-28) — €1.80 at volume
- Alternative: Trinamic TMC6300 (ultra-silent, 1.2A — suitable for smaller motor configs)

### 2.2 Solar Power System

**Panel specification:**
- Type: Flexible monocrystalline, ETFE coating (UV-resistant, weatherproof)
- Target wattage: 10W (conservative for Nordic latitudes)
- Calculation:
  - Fan average draw: 25W × 30% duty cycle = 7.5W average
  - Nordic sun: 3.5 peak sun hours/day (winter minimum)
  - 10W panel × 3.5h = 35Wh/day available
  - Daily consumption: 7.5W × 4h fire + sensors 1W × 24h = 54Wh → requires battery buffer (see below)
- Size: ~200×300mm (fits within chimney pot cap diameter for 200mm+ pots)
- Mount angle: 55-60° tilt, fixed (optimized for 55°N latitude, Denmark/southern Scandinavia)

**Supplier options:**
- **SunPower Maxeon flexible cells** (highest efficiency, 23%+, OEM available) — ~€15-20 at 1k
- **Solbian SP series** (Italian, ETFE, marine-grade, proven outdoor) — ~€18-22
- **Solarland / ETFE generic** (Chinese OEM, lower cost) — ~€8-12, verify quality

**MPPT charge controller IC:**
- Texas Instruments BQ25504 (nano-power boost charger with MPPT, ultra-low quiescent) — €2.50
- Alternative: CN3791 (simpler, lower cost at ~€0.80, less efficient MPPT)

### 2.3 Battery System

**Chemistry choice: LiFePO4 (Lithium Iron Phosphate)**

Why LiFePO4 over Li-ion for this application:
- Operates to -20°C (Li-ion degrades severely below -10°C)
- No thermal runaway risk (critical for outdoor unattended product)
- 2,000+ cycle life (vs 500-800 for standard Li-ion)
- Lower energy density (acceptable — weight target is <3.5kg total)

**Capacity calculation:**
- Cover 2 dark winter days + evening fire: 54Wh × 2 = 108Wh target
- Practical: 80Wh pack (balances weight and coverage)
- Configuration: 4× IFR18650 cells in 2S2P → 6.4V × 5Ah = 32Wh → upgrade to 26650 format for 80Wh

**Cell options:**
- CATL IFR26650-3400 (3.4Ah, 3.2V nominal, -20°C rated) — €3.50 per cell at volume
- EVE IFR26650-3200 (3.2Ah, proven reliability, available through European distributors) — €3.20/cell
- Headway 38120 (larger cylindrical, 8Ah, easier to hit capacity target in fewer cells) — €8/cell

**BMS specification:**
- Seiko Instruments S-8261ABJMD (cell balancing, over-voltage, under-voltage, over-current)
- Low-temperature charging cutoff: 0°C (prevents lithium plating in winter)
- Unit cost: ~€1.20

### 2.4 Thermoelectric Generator (TEG) Backup

The chimney flue reaches 150-300°C during normal operation. A TEG collar converts the heat differential between flue surface and ambient air into electricity — powering the fan during evening/night fires when solar is unavailable.

**Physics:**
- Available ΔT: 200°C flue surface, 5°C ambient (winter worst case) → ΔT = 195°C
- TEG collar surface area: ~150cm² (fits 150mm diameter chimney)
- TEG efficiency at this ΔT: ~4-6%
- Available heat flux: ~500W/m² × 0.015m² = 7.5W thermal
- Electrical output: 7.5W × 5% = ~0.375W usable
- Use case: trickle-charge battery during fire, not primary power source

**TEG modules:**
- Ferrotec 9500/127/060 B (standard bismuth telluride, 127 couples, 15W max output at ΔT=200°C)
- Micropelt eTEG HV56 (thin-film, better for lower ΔT) — niche, higher cost
- Practical recommendation: 2× Ferrotec modules in parallel on collar = ~0.6W continuous during operation

**Integration:** TEG output fed to BQ25504 MPPT input alongside solar — controller handles both sources.

### 2.5 Controller / PCB

**MCU:** Espressif ESP32-S3-WROOM-1 (8MB flash)
- Dual-core 240MHz, 512KB SRAM
- WiFi 802.11 b/g/n + Bluetooth 5.0 LE
- Deep sleep: 7µA (battery conservation)
- OTA firmware updates over WiFi
- Matter 1.2 support (via ESP-IDF framework) — future-proofing for Apple Home / Google Home

**Sensors:**
- Draft pressure: Sensirion SDP810-500Pa (differential, I²C, ±0.3Pa accuracy) — €6.50
- Flue temperature: K-type thermocouple + Maxim MAX31855K amplifier — €4.00 combined
- Ambient temperature: Sensirion SHT40 (temp + humidity, I²C) — €2.50
- Battery voltage: simple resistor divider on ADC pin

**Connectivity:**
- BLE: onboarding + local control (app)
- WiFi: cloud telemetry, remote monitoring, OTA updates
- Optional: RFM95W LoRa module (868MHz, SPI) for vacation homes without WiFi — adds €5

**PCB:** 4-layer, 80×60mm, designed for JLCPCB PCBA at prototype stage.

---

## 3. Bill of Materials

*1,000 unit volume pricing. All components available from Mouser, Digi-Key, or direct.*

| # | Component | Specification | Supplier | Unit Cost (€) |
|---|-----------|---------------|----------|---------------|
| 1 | BLDC Motor | ebm-papst 4606NR, 25W, IP65 | ebm-papst | 25.00 |
| 2 | Motor driver | TI DRV8313PWPR | Mouser | 1.80 |
| 3 | MCU module | ESP32-S3-WROOM-1 (8MB) | Mouser | 3.20 |
| 4 | Solar panel | 10W flexible monocrystalline, ETFE | Solbian OEM | 16.00 |
| 5 | MPPT controller | TI BQ25504 | Mouser | 2.50 |
| 6 | LiFePO4 cells (×4) | CATL IFR26650-3400 | CATL direct | 14.00 |
| 7 | BMS IC | Seiko S-8261ABJMD | Mouser | 1.20 |
| 8 | TEG modules (×2) | Ferrotec 9500/127/060 B | Ferrotec EU | 8.00 |
| 9 | Pressure sensor | Sensirion SDP810-500Pa | Sensirion | 6.50 |
| 10 | Temp/humidity | Sensirion SHT40 | Mouser | 2.50 |
| 11 | Thermocouple + amp | K-type + MAX31855K | Mouser | 4.00 |
| 12 | Magnets (×4) | N52 neodymium, 50×10×5mm | Supermagnete.de | 4.00 |
| 13 | Mounting adapter collar | Stainless 316L, 3 sizes | Contract mfg | 8.00 |
| 14 | Housing (molded) | PA66-GF30, UV-stabilized | Contract mfg | 18.00 |
| 15 | IP67 cable gland | M16, polyamide | Würth | 1.50 |
| 16 | PCB (assembled) | 4-layer, 80×60mm, PCBA | PCBWay | 14.00 |
| 17 | Misc (fasteners, seals, wire) | — | — | 4.00 |
| **TOTAL** | | | | **~134.20** |

*BOM target: €120-140 at 1k volume. Retail: €299 (standard) / €399 (Pro with LoRa + extra sensors)*
*Gross margin at retail €299: ~55% at 1k volume, ~62% at 5k volume*

---

## 4. Installation Design

**Mounting system:**
- Magnetic snap-mount base adapter (N52 neodymium, clamping force >15kg vertical pull)
- Adapter collars: 3 sizes covering 150mm, 175mm, 200mm chimney pots (standard EU)
- Extension pole compatible: M16 thread on base (standard chimney sweep rod thread)
- No drilling, no tools beyond tightening one thumbscrew

**Weight target:** 2.8kg complete (within extension pole operational limit)

**Installation sequence:**
1. Measure chimney pot diameter → select adapter collar
2. Attach adapter collar to chimney pot — clamps with thumbscrew (no drilling)
   - From roof edge with extension pole: ~10 minutes
   - Or during annual sweep visit: 5 minutes
3. Snap fan unit onto magnetic base — one click, locked
4. Download Exodraft app, scan QR code, connect to WiFi
5. Done. Fan starts monitoring and activates automatically when fire lit.

**Total installation time: 15-20 minutes**

---

## 5. Certifications Required

| Certification | Standard | Estimated Cost | Timeline |
|--------------|----------|----------------|----------|
| CE marking | EU Declaration | Included below | — |
| EMC | EN 55032 + EN 55035 | €8,000-12,000 | 3-4 months |
| Radio (WiFi + BT) | RED EN 300 328 | €5,000-8,000 | 2-3 months |
| Electrical safety | LVD EN 62368-1 | €4,000-6,000 | 2-3 months |
| IP67 | IEC 60529 | €2,000-3,000 | 1 month |
| Battery safety | IEC 62133-2 | €5,000-8,000 | 3-4 months |
| RoHS | EU 2011/65/EU | €1,000-2,000 | 1 month |
| **Total** | | **€25,000-39,000** | **4-6 months** |

Recommended test house: Nemko (Nordic expertise, Danish relationships) or TÜV Rheinland.

---

## 6. Manufacturing Approach

**Phase 1 — Prototype (months 0-4):**
- Housing: SLS 3D printed nylon (Protolabs, ~€200/unit at 5 units)
- PCB: JLCPCB PCBA, 10 units, ~€80/unit
- Total: ~€8,000 for 5 working units
- Goal: concept validation, app development, installer feedback

**Phase 2 — Pre-production (months 4-10):**
- Housing tooling: aluminum mold, ~€15,000-20,000 (China) or €30,000-40,000 (Denmark/Germany)
- Run: 200 units for pilot program with existing Exodraft installer network
- PCB: PCBWay PCBA, 200 units

**Phase 3 — Production (months 10+):**
- Contract assembly: Poland or Czech Republic (EU quality standards, lower labor than DK)
- Final QC + firmware flash: Langeskov facility
- Target: monthly batches scaled with demand

---

## 7. IP / Patentability

**Novel patentable elements:**
1. TEG-assisted solar hybrid power system specifically for chimney fan applications
2. Magnetic quick-release chimney pot cap with integrated variable-speed draft fan
3. Draft prediction algorithm combining ambient pressure, flue temperature, and historical ignition patterns

**Recommendation:** File Danish utility model (brugsmodel) within 3 months of prototype completion — establishes priority date at low cost (€800-1,500). File EPO application within 12 months for European protection.

**Prior art to check:** EP applications by Exhausto/Exodraft, Enervex, Systemair under F23L17/00 (chimney accessories) and H02J7/00 (battery charging from non-electrical sources).

---

## 8. Business Case

### Development Investment (NRE)
| Item | Cost |
|------|------|
| Electronics design (PCB + firmware) | €35,000-50,000 |
| Housing design + tooling | €20,000-35,000 |
| App development (iOS + Android) | €25,000-40,000 |
| Certifications | €25,000-39,000 |
| Prototype + testing | €15,000-25,000 |
| **Total NRE** | **€120,000-189,000** |

### Unit Economics

| Volume | BOM | Landed cost (+30%) | Retail | Gross margin |
|--------|-----|--------------------|--------|-------------|
| 500 | €134 | €174 | €299 | 42% |
| 2,000 | €115 | €150 | €299 | 50% |
| 10,000 | €95 | €124 | €299 | 59% |

### Break-even
At €299 retail, 50% gross margin: covers €150,000 NRE at ~2,000 units sold.

### Strategic Value Beyond Margin
Each Solar unit sold:
- Opens a customer previously lost (the 70% who walked away from installation cost)
- Onboards a connected device into Exodraft's data platform
- Creates a recurring touchpoint via app (service, upgrade, cross-sell)

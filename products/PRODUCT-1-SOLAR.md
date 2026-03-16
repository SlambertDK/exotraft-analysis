# Exodraft Solar — Product White Paper
**Version 1.0 | March 2026 | CONFIDENTIAL**

---

## 1. Executive Summary

### Product Vision
Exodraft Solar is a self-powered chimney draft fan that requires zero external electricity, zero electrician, and zero building permits. It integrates a flexible monocrystalline solar panel, a LiFePO4 battery pack, and a thermoelectric generator (TEG) backup — all in a weatherproof housing that installs from ground level using a standard chimney sweep extension pole in under 20 minutes.

### Target Customer
- Homeowners with wood-burning stoves or fireplaces in rural Denmark and Scandinavia
- Summer house / vacation cabin owners where electrician costs are prohibitive
- Renovation projects where routing power cables to the chimney top is structurally impractical
- Eco-conscious urban homeowners who want grid-independent operation

### Key Differentiator
Exodraft Solar is the only chimney draft fan in the world that is genuinely self-installing, self-powering, and self-regulating — with no trades required. Where competitors force a choice between a €400-800 electrician bill or not buying at all, Exodraft Solar removes the question entirely.

### Why This Product Wins
1. **Zero barriers to purchase** — no electrician, no permits, no roofer: the customer can go from box to working fan in one afternoon.
2. **Self-sustaining power** — solar primary + TEG backup means the fan works day and night, summer and winter, even when the sun hasn't shone for days.
3. **Premium positioning at accessible price** — at €299–399, it undercuts the total cost of any wired competitor (product + electrician) while delivering a smarter, connected product.

---

## 2. Technical Architecture

### 2.1 Motor Specification

**Selection Rationale:** The motor must operate reliably at -30°C to +70°C, tolerate vibration, and deliver sufficient pressure rise to augment natural draft in a 150–250mm chimney. Target: 15–40W power draw, 800–2500 RPM variable speed.

| Parameter | Specification |
|-----------|--------------|
| Motor type | Brushless DC (BLDC), outer rotor preferred |
| Power draw | 15–40W (variable, MPPT-controlled) |
| Speed range | 800–2500 RPM |
| Voltage | 12V or 24V DC bus |
| Efficiency | ≥85% at design point |
| IP rating | IP65 minimum (motor itself, before housing) |
| Operating temp | -30°C to +70°C |
| Frame size | ~80–100mm diameter |

**Evaluated Motor Options:**

| Supplier | Model / Series | Key Specs | Unit Cost (1k) | Notes |
|----------|---------------|-----------|----------------|-------|
| **ebm-papst** | W2E200-HK38 series | 24V DC, EC motor, 30–60W, IP44 base | €18–22 | Industry standard, excellent cold-weather track record. Upgrade to IP65 variant. |
| **ebm-papst** | GR series (GR52) | 24V DC, BLDC, 15–35W, compact OD 52mm | €12–16 | Smaller form factor, ideal for 150mm chimney variant |
| **Nidec** | BH series (BH5002) | 24V DC, 30W, integrated encoder | €20–28 | Good availability, slightly higher cost |
| **Maxon Motor** | EC-i 40 series | 24V DC, high precision, 20–50W | €45–80 | Overkill for this application; reserve for Pro variant |
| **Mabuchi** | RS-775 (brushed) | 12V, 25W | €3–5 | Brushed — not recommended (limited life outdoors) |

**Recommended:** ebm-papst GR52 (compact) for 150–175mm models; ebm-papst W2E200 for 200–250mm. These are off-the-shelf, ATEX-certified for temperature extremes, and widely used in harsh outdoor HVAC applications.

**Impeller Design:** Custom injection-molded glass-filled PA66 radial impeller, 6-blade, optimized for 100–300 Pa static pressure rise at 100–300 m³/h flow rate. Tooling: ~€8,000–12,000.

---

### 2.2 Solar Panel Specification

**Power Budget Calculation:**

```
Average motor draw:          25W (50% of max, typical duty cycle)
Duty cycle (fan running):    60% of daylight hours
Daily energy needed:         25W × 8h operation = 200 Wh/day

Solar irradiance (Denmark):
  Summer (June):             4.5 peak sun hours/day
  Winter (December):         0.8 peak sun hours/day
  Annual average:            ~2.5 peak sun hours/day

Winter design case:
  Required panel output:     200 Wh ÷ 0.8h ÷ 0.85 (system efficiency) = 294W peak
  → Too large for integrated panel

Realistic solar role:        Supplement + top-up during day
  Solar panel target:        20–40W peak (realistically provides 16–32 Wh/day winter)
  Battery carries remaining load → TEG provides continuous trickle backup
```

**Conclusion:** In Nordic winter, solar alone cannot sustain full fan operation. Solar + TEG + battery is required. Solar handles summer/spring/autumn autonomously. TEG + battery handles winter night operation.

**Solar Panel Options (Flexible, ETFE-coated Monocrystalline):**

| Supplier | Model | Specs | Efficiency | Unit Cost (1k) | Notes |
|----------|-------|-------|------------|----------------|-------|
| **SunPower** | Flexible 20W (via distributors) | 20W, 12V, ETFE, 2.5mm thin | 22.7% | €28–35 | Highest efficiency available; good for limited surface area |
| **Solbian** | SP 20 / SP 36 | 20W or 36W, ETFE, marine-grade | 20–22% | €35–50 | Premium marine spec; excellent weather resistance; slightly larger |
| **Sunman** | eArc series 30W | 30W, ETFE, 2mm thin, no glass | 21% | €22–28 | No-glass design reduces weight significantly |
| **Renogy** | Flexible 20W | 20W, ETFE monocrystalline | 18–20% | €15–18 | Budget option; acceptable for entry product |

**Recommended:** Sunman eArc 30W or SunPower 20W. Target: **two 20W panels** (40W total) integrated into the top surface of the fan housing, angled at **55° from horizontal** (matching 55–60° latitude solar angle for maximum annual energy capture).

**Panel Mounting Integration:** Panels are bonded into recessed channels on the top housing using UV-stable silicone adhesive (Dowsil 791). Panels curve gently to follow housing profile. No external brackets. Wiring runs through internal conduit sealed with potting compound.

---

### 2.3 Battery System

**Capacity Calculation:**

```
Post-sunset operation requirement: 4 hours at 25W = 100 Wh
Add 20% DoD buffer (LiFePO4 safe to 20% SoC):  100 ÷ 0.8 = 125 Wh
Add temperature derating (-20°C, ~20% capacity loss): 125 ÷ 0.8 = 156 Wh

Nominal cell voltage (LiFePO4): 3.2V
Target pack configuration: 4S1P (12.8V nominal, 14.4V max charge)
Required capacity: 156 Wh ÷ 12.8V = 12.2 Ah → specify 15 Ah pack
```

**Battery Chemistry:** LiFePO4 (Lithium Iron Phosphate) — mandatory for this application.
- Operating range: -20°C to +60°C (discharge); 0°C to +45°C (charge)
- Cycle life: 2,000+ cycles vs. ~500 for Li-ion — critical for daily cycling
- No thermal runaway risk — fire-safe for rooftop deployment
- Flat discharge curve maintains motor performance throughout cycle

**Cell Options:**

| Supplier | Cell Model | Format | Capacity | Unit Cost (1k, per cell) | Notes |
|----------|-----------|--------|----------|--------------------------|-------|
| **CATL** | IFR26650-3400 | 26650 cylindrical | 3.4 Ah | €3.20 | Best availability, CATL quality; 4S5P = 17 Ah pack |
| **EVE Energy** | LF50K (prismatic) | Prismatic pouch | 50 Ah | €18–22 per cell | Oversized for this app; better for Product 2 |
| **Headway** | 38120 | 38120 cylindrical | 8 Ah | €7–9 | Excellent cold temp performance to -40°C; 4S2P = 16 Ah |
| **A123 Systems** | ANR26650M1B | 26650 cylindrical | 2.5 Ah | €4–6 | Proven outdoor/automotive use; excellent pulse power |

**Recommended:** Headway 38120 cells, 4S2P configuration → 12.8V / 16 Ah = **204 Wh** (exceeds target with margin). Weight: ~1.1 kg for cell pack.

**BMS Requirements:**

| Function | Specification |
|----------|--------------|
| Cell balancing | Passive balancing, ±20mV accuracy |
| Low-temp charge cutoff | Inhibit charging below 0°C (critical for Danish winters) |
| Over-current protection | 20A continuous, 40A peak |
| Over-temp protection | Cutoff at 60°C cell temp |
| Under-voltage protection | Cutoff at 2.8V/cell |
| Communication | I2C to main MCU (fuel gauge: TI BQ34Z100) |
| Recommended IC | Daly Smart BMS (production), TI BQ76940 (custom PCB) |

**Note on Low-Temperature Charging:** This is the most critical BMS feature. LiFePO4 cells permanently damaged by charging below 0°C. BMS must measure cell temperature and disable charge path when cold. A heating element (PTC resistor, 2–5W) can pre-warm cells before charging in sub-zero conditions — powered from TEG output.

---

### 2.4 Thermoelectric Generator (TEG) Backup

**Operating Principle:** A TEG module generates electricity from a temperature differential between a hot side (flue exterior surface) and a cold side (ambient air). No moving parts. Silent. Works whenever the fire is burning.

**Power Calculation:**

```
Typical chimney surface conditions (when fire burning):
  Flue exterior temperature: 120–180°C (insulated chimney liner)
  Ambient air temperature: -10°C to +20°C
  ΔT available: 100–190°C

TEG module efficiency (Bi₂Te₃ at ΔT=150°C): ~5–7%
Heat flux through mounting contact area:
  Contact area: 4 × modules × 40mm × 40mm = 64 cm²
  Heat flux estimate: 1,500 W/m² × 0.0064 m² = 9.6W thermal input per module
  TEG electrical output: 9.6W × 6% efficiency = 0.58W per module

With 4 modules in parallel:  ~2.3W continuous TEG output
Sustained over 6h fire burn: 2.3W × 6h = 13.8 Wh contribution
```

**Analysis:** TEG provides a modest but meaningful continuous trickle charge during fire operation — approximately 14–20 Wh per typical fire cycle. This extends battery life and provides "fire = fan running" autonomous behavior. TEG alone cannot power the fan but meaningfully extends battery range.

**TEG Module Options:**

| Supplier | Module | Specs | Unit Cost (1k) | Notes |
|----------|--------|-------|----------------|-------|
| **Ferrotec** | HT8-12-40-F2 | 12V, 40×40mm, Tmax 300°C | €12–18 | Industry standard, good at high ΔT |
| **Kryotherm** | TB-127-2.0-1.3 | 5V, 40×40mm, up to 200°C hot side | €8–12 | Good value, widely available |
| **Micropelt** | TE-CORE series | Thin-film, high power density | €25–40 | Premium; better for constrained space |
| **Custom** | Based on Bi₂Te₃ pellets | Matched to chimney ΔT profile | €6–10 | Requires custom assembly |

**Recommended:** 4× Ferrotec HT8-12-40-F2 modules mounted on a custom copper saddle that clamps around the upper chimney collar. Spring-loaded contact pressure maintained by Belleville washers.

**TEG Integration:** TEG output (≈2–4W at 8–12V) fed through a dedicated MPPT harvesting IC (TI BQ25570, designed for TEG/thermoelectric inputs) into the main 12.8V power bus. BQ25570 handles the highly variable TEG output voltage as ΔT changes with fire intensity.

---

### 2.5 Controller / PCB

**System Architecture:** Single custom PCB (2-layer, 100×80mm) housing all power management, motor control, sensing, and connectivity functions.

**Microcontroller:** ESP32-S3 (Espressif)
- Dual-core Xtensa LX7, 240MHz
- Integrated WiFi 802.11 b/g/n + Bluetooth 5.0 LE
- Rich peripheral set: ADC, I2C, SPI, UART, PWM
- Deep sleep current: 7µA (critical for battery conservation)
- Unit cost: ~€3.50 at 1k volume
- Alternative: Nordic nRF5340 if WiFi not required (lower power, better BLE, +€2)

**Power Management ICs:**

| Function | IC | Notes |
|----------|----|-------|
| Solar MPPT charging | TI BQ25504 | Ultra-low power, handles panel voltages 0.1–5V above battery; cold-start from 330mV |
| TEG MPPT harvesting | TI BQ25570 | Same family, optimized for ultra-low input power sources |
| Battery fuel gauge | TI BQ34Z100-G1 | Accurate SoC reporting, temperature compensation |
| Motor driver | TI DRV8313 | 3-phase BLDC, 2.5A per phase, integrated protection |
| Buck/boost regulator | TI TPS63020 | Provides stable 3.3V for MCU from battery across full SoC range |
| Load switch | TI TPS22810 | Controlled power gating for sensor subsystems |

**Sensors:**

| Sensor | Part | Measurement | Interface | Cost |
|--------|------|-------------|-----------|------|
| Draft pressure | Sensirion SDP810-500Pa | Differential pressure ±500Pa | I2C | €8–12 |
| Flue temperature | NTC thermistor 100kΩ + ADC OR PT100 RTD | 0–400°C | ADC/SPI | €1–3 |
| Ambient temperature | Sensirion SHT40 | Temp + Humidity | I2C | €2–4 |
| Battery temperature | NTC 10kΩ (on BMS) | -40°C to +85°C | ADC | €0.30 |
| Accelerometer | STMicro LIS2DW12 | Tamper/vibration detection | I2C | €1.50 |

**Connectivity:**
- **Primary:** Bluetooth 5.0 LE via ESP32-S3 onboard radio → smartphone app (iOS/Android)
- **Optional add-on:** RFM95W LoRa module (868MHz, SPI) → Exodraft Gateway or direct cloud via LoRaWAN (TTN)
- **LoRa use case:** Vacation homes/cabins where no WiFi exists; LoRa range 5–15km LOS, 1–3km through buildings

**Control Algorithm:**
```
Priority 1: Draft pressure feedback (target: -10 to -15 Pa negative pressure)
Priority 2: Flue temperature > 80°C → fan ON
Priority 3: Time-of-day schedule (via app)
Safety: If flue temp > 350°C → emergency fan ramp-up (overheat protection)
Safety: If pressure sensor reads positive → backdraft warning → app notification
```

---

### 2.6 Housing / Enclosure

| Parameter | Specification |
|-----------|--------------|
| Primary material | PA66-GF30 (glass-filled nylon, 30% glass fiber) |
| UV stabilization | Carbon black pigment + HALS additives; 10+ year UV resistance |
| IP rating | IP67 (tested per IEC 60529) |
| Operating temp | -30°C to +70°C |
| Finish | Textured dark gray or RAL 7016 anthracite |
| Chimney compatibility | 150mm, 175mm, 200mm, 250mm via interchangeable adapter rings |
| Overall dimensions (200mm variant) | Ø280mm × 180mm tall |
| Weight target | <3.0 kg complete assembly |

**Assembly:** Two-piece clamshell design. Upper half houses solar panels, electronics, battery. Lower half forms the fan impeller chamber and mounting interface. Joined by stainless steel M4 captive screws + silicone gasket (Shore A 50, -55°C rated).

**Cable penetrations:** Zero. All power is internal. Only external interface is the BLE/LoRa antenna (internal PCB antenna through radome window in housing).

---

## 3. Bill of Materials (BOM)

*Pricing: estimated 1,000-unit volume. EUR. Excludes tooling amortization.*

| # | Component | Specification | Primary Supplier | Backup Supplier | Unit Cost (€) | Lead Time | Notes |
|---|-----------|--------------|-----------------|-----------------|---------------|-----------|-------|
| 1 | BLDC Motor | ebm-papst GR52 or W2E200, 24V | ebm-papst | Nidec BH series | 15.00–22.00 | 8–10 wks | Core component; negotiate volume price |
| 2 | Impeller | PA66-GF30, 6-blade radial, custom | In-house mold | Outsource China | 3.50 | — | Tooling: €8k; amortized over 5k units |
| 3 | Solar panel | Sunman eArc 20W × 2 (ETFE flex) | Sunman / Suntech | Renogy flex | 22.00 | 4–6 wks | 40W total; custom size option at 5k units |
| 4 | Battery cells | Headway 38120 LiFePO4, 4S2P (8 cells) | Headway | CATL IFR26650 | 52.00 | 6–8 wks | 16Ah / 204Wh; 8 cells × €6.50 |
| 5 | BMS module | TI BQ76940-based, 4S, low-temp cutoff | Custom PCB | Daly Smart BMS | 4.50 | 4 wks | Integrated on main PCB |
| 6 | TEG modules | Ferrotec HT8-12-40-F2, ×4 | Ferrotec | Kryotherm TB-127 | 14.00 | 6–8 wks | 4 modules × €3.50 |
| 7 | TEG copper saddle | Custom CNC copper, spring-loaded | Local CNC | — | 4.00 | 2 wks | Simple part; local sourcing preferred |
| 8 | Main PCB | 2-layer, 100×80mm, assembled | JLCPCB (proto) / CMC (prod) | PCBWay | 12.00 | 3–5 wks | JLCPCB for <500 units; CMC for 1k+ |
| 9 | MCU | ESP32-S3 (Espressif, WROOM module) | Mouser/Digi-Key | LCSC | 3.50 | 2–4 wks | Consider nRF5340 for BLE-only variant |
| 10 | Solar MPPT IC | TI BQ25504 | TI Direct / Mouser | LCSC | 2.80 | 2–4 wks | |
| 11 | TEG MPPT IC | TI BQ25570 | TI Direct / Mouser | LCSC | 2.80 | 2–4 wks | |
| 12 | Motor driver IC | TI DRV8313 | TI Direct / Mouser | — | 2.50 | 2–4 wks | |
| 13 | Pressure sensor | Sensirion SDP810-500Pa | Sensirion Direct | Mouser | 9.00 | 4–6 wks | Key sensor; single source risk — qualify backup |
| 14 | Temp/humidity | Sensirion SHT40 | Sensirion / Mouser | — | 2.80 | 2 wks | |
| 15 | Buck/boost reg | TI TPS63020 | Mouser | LCSC | 1.20 | 2 wks | |
| 16 | LoRa module (optional) | HopeRF RFM95W, 868MHz | HopeRF / Mouser | RAK Wireless | 4.50 | 2–4 wks | Optional; adds €4.50 to Pro SKU |
| 17 | Housing (upper) | PA66-GF30, custom injection mold | Tooling China / Assembly Denmark | — | 8.00 | — | Tooling: €15k; amortized over 10k units |
| 18 | Housing (lower) | PA66-GF30, with adapter system | Same as above | — | 6.00 | — | |
| 19 | Adapter rings ×4 | PA66-GF30, 150/175/200/250mm | Same mold family | — | 1.50 | — | Customer selects; include 1 in box |
| 20 | Magnets | N52 neodymium, Ø30×10mm, ×8 | CMS Magnetics | Amazon Supply | 6.00 | 2–4 wks | See Section 4 for force calculation |
| 21 | Gasket / seals | Silicone, Shore A 50, -55°C rated | Trelleborg | Parker Hannifin | 1.80 | 2 wks | |
| 22 | Stainless hardware | M4 screws, washers, Belleville washers | Fastenright EU | — | 0.80 | 1 wk | SS316 grade |
| 23 | Antenna | PCB trace + IPEX connector for ext. | Taoglas / Molex | — | 0.50 | 1 wk | Internal; radome window in housing |
| 24 | Potting compound | Dowsil 1-4173 (electronics potting) | Dow Corning | Henkel Loctite | 0.80 | 2 wks | For PCB conformal coat |
| 25 | Packaging | Recycled cardboard, FSC certified | Local EU supplier | — | 2.50 | 2 wks | Branding opportunity |
| 26 | Quick-start guide | 4-page fold, 6 languages | Local print | — | 0.40 | 1 wk | |
| **TOTAL BOM** | | | | | **€84–102** | | *Excl. tooling amortization* |

**BOM Notes:**
- Motor is the single largest cost driver (15–25% of BOM)
- Battery is second largest (~€52 at current cell prices; LiFePO4 prices declining ~8%/year)
- At 5k+ volume, direct OEM agreements with ebm-papst and Headway reduce costs ~15%
- Pro version (with LoRa) adds ~€4.50 to BOM; retail premium of €100 → strong margin uplift

---

## 4. Installation Design

### 4.1 Magnetic Quick-Release Mounting System

**Concept:** Eight N52 neodymium disc magnets embedded in the lower housing ring engage with a stainless steel receiver plate that replaces the existing chimney cap. The magnetic coupling holds the fan securely against wind forces while allowing quick removal for cleaning without tools.

**Magnet Specification:**
- Grade: N52 neodymium (highest commercially available grade)
- Dimensions: Ø30mm × 10mm disc
- Pull force per magnet (N52, 30×10mm): ~18 kg (176 N) — measured steel-to-steel
- 8 magnets in ring pattern: total pull force ~144 kg (1,410 N)

**Wind Load Calculation:**
```
Fan housing projected area: ~0.06 m² (280mm × 215mm profile)
Wind speed (design): 35 m/s (Danish coastal storm)
Wind force: 0.5 × 1.2 kg/m³ × (35 m/s)² × 0.06 m² × 1.2 (drag coefficient) = 52.9 N

Safety factor: 1,410 N available ÷ 52.9 N load = 26.6× safety factor ✓
```

**Safety Release:** The magnetic coupling is designed to release cleanly at >200N uplift force (e.g., deliberate removal, extreme mechanical load) to prevent chimney liner damage.

**Receiver Plate:** 2mm 316L stainless steel laser-cut plate with central hole matching chimney flue ID. Installs once (permanent), snap-locks to existing chimney cap recess or replaces cap entirely. Include in box.

### 4.2 Extension Pole Compatibility

- Thread: M16 female socket (recessed in underside of housing)
- Compatible with: standard European chimney sweep extension poles (M16 is industry standard in Denmark/Germany/Netherlands)
- Also accepts: ½" BSP adapter (included) for UK/Irish market
- Pole mounting: used for installation only — once magnets engage, pole unscrews and is removed

### 4.3 Weight Budget

| Component | Est. Weight (g) |
|-----------|----------------|
| Motor + impeller | 580 |
| Housing (both halves) | 420 |
| Solar panels (×2) | 200 |
| Battery pack | 1,100 |
| TEG + saddle | 280 |
| PCB + electronics | 180 |
| Magnets (×8) | 160 |
| Hardware + misc | 80 |
| **TOTAL** | **3,000 g** |

*At the 3kg limit. Lightweight motor selection (GR52) and thin-panel choice are critical.*

### 4.4 Installation Procedure (Target: <20 minutes)

1. Remove existing chimney cap (from ground using extension pole + cap-removal tool, if required)
2. Place magnetic receiver plate over chimney flue opening — it self-centers
3. Attach Exodraft Solar to extension pole via M16 thread
4. Raise unit to chimney top; align with receiver plate
5. Lower into position — magnets engage with audible/tactile click
6. Unscrew extension pole — it releases from the M16 socket
7. Download Exodraft app; scan QR code on housing; pair via Bluetooth
8. Configure desired draft level and schedule
**Total: 15–20 minutes. No tools beyond standard chimney sweep pole.**

---

## 5. Certifications Required

| Certification | Standard/Directive | Scope | Est. Cost | Timeline |
|--------------|-------------------|-------|-----------|----------|
| CE Marking | EU Declaration of Conformity | Required to sell in EU | — | Ongoing |
| — Radio Equipment | RE Directive 2014/53/EU | BLE + LoRa radio | €8,000–12,000 | 3–4 months |
| — LVD | EN 62368-1:2020 | Battery-powered device safety | €5,000–8,000 | 2–3 months |
| — EMC | EN 55032, EN 55035 | Emissions + immunity | €4,000–6,000 | 2–3 months |
| IP67 | IEC 60529 | Ingress protection | €2,000–3,000 | 4–6 weeks |
| Battery Safety | IEC 62133-2 | Li-ion/LiFePO4 pack | €6,000–10,000 | 3–4 months |
| RoHS | EN IEC 63000 | Hazardous substance compliance | €1,500–2,500 | 2–4 weeks |
| REACH | EU 1907/2006 | Chemical compliance | €500–1,500 | 2–4 weeks |
| **TOTAL ESTIMATE** | | | **€27,000–43,000** | **5–7 months** |

**Recommended Certification Partner:** TÜV Rheinland or Bureau Veritas (both have Danish offices). SGS for lower cost.

**Pre-compliance Testing:** Engage test lab at PCB prototype stage (before final housing) to catch EMC issues early. Pre-compliance test cost: ~€2,000–3,000 (saves money vs. failing formal testing).

**Certification Strategy:** File as a radio equipment product under RED Directive. This covers EMC and LVD within a single CE declaration, reducing total certification cost vs. filing separately.

---

## 6. Manufacturing Approach

### Phase 1: Prototype (0–12 months, 0–50 units)
- **PCB:** JLCPCB (China) or Eurocircuits (Europe) for assembled prototype boards
- **Housing:** 3D printed SLS PA12 (same material family as target PA66-GF30); external: SLS vendor like Materialise or Shapeways
- **Battery:** Hand-assembled from cells; custom BMS on prototype PCB
- **Assembly:** In-house, Denmark
- **Cost per unit:** €400–800 (prototype cost; acceptable for validation)

### Phase 2: Pilot Production (12–24 months, 50–500 units)
- **PCB:** PCBWay (assembled SMT + THT) or Eurocircuits
- **Housing:** Soft tooling (aluminum mold, 3,000-shot life) — cost: €8,000–15,000 per tool
- **Final assembly:** In-house, Denmark (maintain quality control, build process knowledge)
- **Battery pack:** Sourced from Headway or equivalent, custom dimensions

### Phase 3: Volume Production (24+ months, 500–5,000 units/year)
- **PCB:** Transition to contract manufacturer — recommended: Elcoteq Estonia or Enics Denmark
- **Injection molding:** Hard steel tooling (100,000-shot life) in China (Foxconn IDC or equivalent) — cost: €25,000–40,000 total tooling
- **Final assembly & QC:** Contract partner in Poland or Czech Republic (lower cost than Denmark, EU quality standards) OR keep in Denmark at premium for "Danish Design" marketing angle
- **Logistics:** Direct ship from CE to customers (avoid double-handling)

### Cost Reduction Roadmap
| Volume | BOM Cost | Action |
|--------|----------|--------|
| 500 units | €102 | Baseline |
| 1,000 units | €92 | Motor volume discount; direct cell sourcing |
| 5,000 units | €78 | Custom solar panel OEM; tooling fully amortized; EE assembly |
| 10,000 units | €65 | Full OEM agreements; offshore assembly; optimized PCB layout |

---

## 7. IP / Patentability

### Novel Elements

**Element 1: TEG-Assisted Solar Hybrid Power for Chimney Draft Applications**
- Combining solar + thermoelectric generation specifically for chimney-mounted fan operation is novel
- The integration of TEG heat harvesting from flue gas surface with MPPT charging of a shared battery is not found in prior art (chimney applications)
- **Recommendation:** File utility patent in Denmark (DK), then PCT international within 12 months
- **Strength:** High — specific application + integration method

**Element 2: Magnetic Quick-Release Chimney Cap with Integrated Fan**
- Quick-release magnetic mounting of powered devices to chimney tops is not found in existing chimney products
- The receiver-plate + magnet array system for rooftop equipment is likely novel
- **Recommendation:** File design patent + utility model (utility model is faster: 6 months vs. 2–3 years for full patent)

**Element 3: Draft Pressure + Flue Temperature Combined Control Algorithm**
- Using differential pressure feedback (not just temperature) to continuously modulate fan speed for optimal draft is likely novel in chimney applications
- **Recommendation:** Trade secret initially; patent if algorithm proves highly effective

### Prior Art — Key Patents to Check
- US9341373B2 — "Chimney fan with thermoelectric power generation" (check scope carefully)
- EP2366951A2 — "Chimney draught fan" (existing Exodraft-adjacent art)
- WO2014/173384 — Solar-powered ventilation systems (broad; check if chimney-specific excluded)
- US20190186747A1 — Magnetic mounting systems for HVAC components

### Recommendation
1. **Immediate:** File Danish utility model for magnetic mounting system (cost: ~€2,500–5,000 with attorney; fast track 6 months)
2. **Year 1:** File full utility patent for TEG-solar hybrid power architecture (cost: €8,000–15,000 DK + PCT)
3. **Year 2:** Evaluate US and German patent filing based on market traction
4. **Trade secrets:** Control algorithm, sensor fusion logic — keep in firmware, do not publish

---

## 8. Business Case

### Development Cost (NRE — Non-Recurring Engineering)

| Item | Cost Estimate (€) | Notes |
|------|-------------------|-------|
| Electronics design (PCB, firmware) | 35,000–55,000 | 6–9 months, 1–2 engineers |
| Mechanical design (housing, impeller) | 20,000–30,000 | 3–4 months, industrial designer + ME |
| Injection mold tooling (4 tools) | 35,000–55,000 | Includes adapter ring tools |
| Prototype builds (3 rounds, 10 units each) | 15,000–25,000 | |
| Certifications | 27,000–43,000 | See Section 5 |
| IP filing (utility model + patent) | 12,000–20,000 | |
| App development (iOS + Android) | 20,000–35,000 | MVP: 3 months |
| **Total NRE** | **€164,000–263,000** | Mid-point: ~€215,000 |

### Timeline
```
Month 0–2:   Electronics design + BOM finalization
Month 2–6:   PCB prototype; firmware v0.1; 3D printed housing testing
Month 6–9:   Soft tool injection molded prototype; field testing (Denmark, 20 units)
Month 9–12:  Certification testing begins; firmware refinement
Month 12–15: Certifications complete; tooling ordered for production
Month 15–18: Pilot production (200 units); sales begin
Month 18+:   Volume production ramp
```

### Pricing & Margin Analysis

| SKU | Retail (€) | BOM (1k vol, €) | Gross Margin | Notes |
|-----|-----------|-----------------|--------------|-------|
| Exodraft Solar (entry) | 299 | 92 | 69% | BLE only, 2×20W solar |
| Exodraft Solar Pro | 399 | 97 | 75% | + LoRa, 2×20W solar + TEG |
| Replacement receiver plate | 29 | 4 | 86% | High-margin accessory |
| Extension pole (branded) | 49 | 8 | 84% | Bundled or sold separately |

*Note: 69–75% gross margin is excellent for hardware. Target 65%+ at full scale.*

### Break-Even Analysis

| Scenario | Units to Break Even |
|----------|---------------------|
| NRE = €215k, GM = €185/unit (entry) | **1,162 units** |
| NRE = €215k, GM = €240/unit (Pro) | **896 units** |
| NRE = €215k, blended GM = €200/unit | **1,075 units** |

**Break-even at ~1,000–1,200 units is highly achievable** given Denmark alone has ~500,000 wood-burning installations, and the Nordics + Germany represent a total addressable market of several million units.

### Revenue Projections

| Year | Units | Revenue | Gross Profit |
|------|-------|---------|--------------|
| Year 1 (launch) | 300 | €90,000 | €55,000 |
| Year 2 | 1,200 | €420,000 | €252,000 |
| Year 3 | 3,500 | €1,225,000 | €787,500 |
| Year 4 | 8,000 | €2,800,000 | €1,960,000 |

*Assumes 70% Pro mix at €399 average. Y3+ includes German/Swedish market entry.*

### Risk Factors
1. **Battery cost volatility** — LiFePO4 cell prices have been falling; risk is upside
2. **Solar performance in winter** — managed by TEG + battery architecture; honest marketing required
3. **Motor single-source risk** — ebm-papst dependence; qualify Nidec as backup
4. **Regulatory changes** — drone/IoT regulations could affect LoRa; unlikely to affect BLE
5. **Copycat risk** — Chinese competitors can copy quickly; IP + brand + service moat is the defense

---

*Document prepared by Exodraft Product Team | Version 1.0 | March 2026*
*Classification: Confidential — Internal Use Only*
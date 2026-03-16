# ExoTraft Cap Pro — Product White Paper

**Version:** 1.0 | **Date:** March 2026 | **Classification:** Confidential

---

## 1. Executive Summary

### Product Vision
ExoTraft Cap Pro is a premium all-in-one smart chimney cap that replaces the existing chimney pot cap entirely. It integrates draft assistance, a full weather station, PM2.5/CO air quality monitoring, wind-direction optimization, and connectivity — in a single unit that installs in under 30 minutes using the existing chimney pot as its mounting base.

### Target Customer
- New build premium homes (architect-specified)
- Property renovation projects (replaces old cap as part of chimney service)
- Smart home enthusiasts (Matter-compatible, full app integration)
- Housing associations and property managers (fleet monitoring dashboard)

### Why This Product Wins
**For the homeowner:** One product replaces four: chimney cap + draft fan + weather station + air quality monitor. Everything in one app.

**For ExoTraft:** The Cap Pro is the data platform play in hardware form. At 10,000 units, ExoTraft operates a hyperlocal air quality and weather sensor network across Denmark and Northern Europe — with commercial value to municipalities, insurers, and researchers.

**For the market:** No competitor offers this. Enervex sells industrial fans. Netatmo sells weather stations. Nobody has combined them for the chimney.

---

## 2. Technical Architecture

### 2.1 Integrated Functions

| Function | Description | Component |
|----------|-------------|-----------|
| Draft fan | Primary draft assistance | BLDC motor + impeller |
| Wind direction optimization | Rotating cap deflects prevailing wind | Servo + encoder + passive vane |
| Rain/debris protection | Covers chimney opening | Integrated louver design |
| PM2.5 sensor | Combustion quality monitoring | Sensirion SPS30 |
| CO sensor | Safety + combustion optimization | Figaro TGS5042 |
| Wind speed/direction | Hyperlocal weather data | Gill Ultrasonic or FT Technologies FT742 |
| Barometric pressure | Draft prediction, fire readiness score | Bosch BMP390 |
| Temperature/humidity | Ambient conditions | Sensirion SHT40 |
| Flue temperature | Combustion state detection, auto-start | K-type thermocouple + MAX31855 |
| Connectivity | App + cloud + smart home | ESP32-S3 (WiFi + BT5) + optional LoRa |

### 2.2 Motor & Fan Specification

**Primary draft fan:**
- Type: Brushless DC, EC motor
- Supplier options: ebm-papst 4600 series, Ziehl-Abegg GR series
- Power: 25W nominal, 40W peak
- Speed range: 500–2500 RPM (PWM controlled)
- Static pressure: 100–250 Pa (sufficient for residential chimneys up to 8m)
- Airflow: 80–250 m³/h variable
- IP rating: IP65 minimum (fan + motor assembly)
- Operating temperature: -30°C to +70°C
- Expected lifetime: 40,000 hours (>10 years at typical use)

**Motor driver IC:**
- Primary: Texas Instruments DRV8313 (3-phase BLDC, 2.8A peak)
- Alternative: Trinamic TMC6300 (ultra-silent operation)

### 2.3 Wind Direction Optimization System

**Passive vane (standard):**
- Cap housing shaped with asymmetric aerodynamic profile
- Rotates freely on low-friction PTFE bearing around chimney pot
- Orients opening leeward automatically — no motor, no power
- Prevents wind-induced backdraft without fan running at all
- Manufacturing: injection-molded PA66-GF30

**Active servo option (Pro version):**
- Servo: Hitec HS-5086WP (waterproof, 2.5kg·cm)
- Encoder: AS5048A magnetic absolute encoder
- Controller: reads wind vane sensor → adjusts cap orientation optimally
- Benefit: can orient for stack effect enhancement, not just backdraft prevention
- Adds ~€15 BOM cost

### 2.4 Air Quality Sensors

**PM2.5 — Sensirion SPS30:**
- Measurement range: 0–1000 µg/m³
- Accuracy: ±10 µg/m³ or ±10% (whichever greater)
- Interface: I²C / UART
- Operating temperature: -10°C to +60°C (heated inlet required for Nordic winter)
- Lifespan: 8 years continuous operation
- Unit cost: ~€12 (volume)
- Note: Require periodic cleaning — design access port in housing

**CO sensor — Figaro TGS5042:**
- Electrochemical sensor, highly selective for CO
- Range: 0–1000 ppm
- Response time: T90 < 30 seconds
- Lifespan: 5 years
- Unit cost: ~€8 (volume)
- Alternative: SGX Sensortech MICS-4514 (dual CO + NO2, €5)

### 2.5 Weather Station Sensors

**Wind — Gill WindSonic (ultrasonic anemometer):**
- No moving parts — critical for zero-maintenance outdoor product
- Speed: 0–60 m/s, ±2% accuracy
- Direction: 0–359°, ±3° accuracy
- Operating temp: -35°C to +70°C
- Interface: RS-232 / SDI-12
- Unit cost: ~€180 (too expensive for consumer product)
- **Alternative: FT Technologies FT742-SM** (~€45, OEM pricing) — recommend for production
- **Budget alternative: Inspeed Vortex** (~€25) — lower accuracy but sufficient for app use case

**Pressure — Bosch BMP390:**
- Range: 300–1250 hPa
- Accuracy: ±0.5 Pa (extremely precise for draft prediction)
- Temperature compensation built-in
- Interface: I²C / SPI
- Unit cost: ~€1.50

**Temperature/Humidity — Sensirion SHT40:**
- Temp: -40°C to +125°C, ±0.2°C
- Humidity: 0–100% RH, ±1.8% RH
- Factory calibrated, no external components needed
- Unit cost: ~€2.50

**Flue temperature — K-type thermocouple + MAX31855:**
- Thermocouple range: -200°C to +1350°C (far exceeds chimney requirements)
- Probe mounted in chimney flue adapter collar
- MAX31855 amplifier + cold junction compensation
- Unit cost: ~€4 combined

### 2.6 Power System

**Solar primary:**
- Panel: SunPower Maxeon flexible cell, 10W, ETFE coated
- Integrated into top surface of cap housing at 60° angle (optimal for 55° latitude)
- Size: ~200×250mm (fits within cap diameter for 200mm+ chimney pots)
- MPPT controller: Texas Instruments BQ25504 (ultra-low power harvesting)

**Battery backup:**
- Chemistry: LiFePO4 (safe, -20°C capable with BMS, 2000+ cycle life)
- Capacity: 20Wh (sufficient for 8+ hours sensor operation + 2 hours fan)
- Cells: CATL IFR18650-1500mAh or EVE IFR18650-1500
- BMS: Seiko Instruments S-8261 series with low-temp charging protection
- Charge cutoff: 0°C (prevents lithium plating in winter)

**Auxiliary low-voltage input:**
- 2-wire 12V DC input via thin cable through chimney liner
- Enables 100% uptime in winter/cloudy conditions
- Cable: Helukabel SIHF 2×0.5mm², rated to 180°C
- Connector: IP67 M8 circular connector at cap base

**Power budget at steady state:**
| Component | Current draw |
|-----------|-------------|
| ESP32-S3 (active WiFi) | 160 mA |
| Sensirion SPS30 | 60 mA |
| Wind sensor FT742 | 15 mA |
| BMP390 + SHT40 | 1 mA |
| Fan motor (idle) | 0 mA |
| Fan motor (running) | 110–175 mA |
| **Total sensors + MCU** | **~240 mA @ 5V = 1.2W** |
| **Total with fan running** | **~400 mA @ 5V = 2W** |

10W solar panel in Nordic conditions (avg 3.5 peak sun hours/day) → ~35Wh/day available vs. ~15Wh/day consumption → comfortable margin even in winter.

### 2.7 Connectivity & Firmware

**MCU:** Espressif ESP32-S3
- Dual-core 240MHz, 512KB SRAM
- WiFi 802.11 b/g/n + Bluetooth 5.0 LE
- 8MB flash for OTA firmware updates
- Deep sleep: 7µA (for battery conservation at night)

**Protocols:**
- **BLE**: direct app connection, onboarding, local control
- **WiFi/MQTT**: cloud telemetry, remote monitoring
- **Matter 1.2**: smart home integration (Apple Home, Google Home, Amazon Alexa)
- **LoRa 868MHz** (optional add-on module): vacation homes without WiFi, range 2–5km

**Data published every 60 seconds:**
- Draft status (on/off, RPM, pressure)
- PM2.5 (µg/m³)
- CO (ppm)
- Wind speed + direction
- Temperature, humidity, pressure
- Flue temperature
- Battery state of charge
- Solar panel output

---

## 3. Bill of Materials (BOM)

*Pricing at 1,000 unit volume. All components available from Mouser, Digi-Key, or direct.*

| # | Component | Specification | Supplier | Unit Cost (€) | Lead Time |
|---|-----------|---------------|----------|---------------|-----------|
| 1 | BLDC Motor | ebm-papst 4606NR, 25W, IP65 | ebm-papst direct | 28.00 | 6-8 weeks |
| 2 | Motor driver | TI DRV8313PWPR | Mouser/Digi-Key | 1.80 | Stock |
| 3 | MCU module | ESP32-S3-WROOM-1 (8MB) | Espressif/Mouser | 3.20 | Stock |
| 4 | PM2.5 sensor | Sensirion SPS30 | Sensirion direct | 12.00 | 4-6 weeks |
| 5 | CO sensor | Figaro TGS5042 | Mouser | 8.00 | 8 weeks |
| 6 | Wind sensor | FT Technologies FT742-SM | FT direct | 45.00 | 8-10 weeks |
| 7 | Pressure | Bosch BMP390 | Mouser | 1.50 | Stock |
| 8 | Temp/RH | Sensirion SHT40 | Mouser | 2.50 | Stock |
| 9 | Thermocouple amplifier | Maxim MAX31855K | Mouser | 2.00 | Stock |
| 10 | K-type thermocouple probe | 6mm, 300°C rated | RS Components | 4.00 | Stock |
| 11 | MPPT controller | TI BQ25504 | Mouser | 2.50 | Stock |
| 12 | Solar panel | SunPower 10W flexible | SunPower OEM | 18.00 | 10-12 weeks |
| 13 | LiFePO4 cells (×2) | CATL IFR18650-1500 | CATL direct | 4.00 | 8 weeks |
| 14 | BMS IC | Seiko S-8261ABJMD | Mouser | 1.20 | Stock |
| 15 | Servo (active version) | Hitec HS-5086WP | HiTec direct | 15.00 | Stock |
| 16 | Magnetic encoder | AMS AS5048A | Mouser | 3.00 | Stock |
| 17 | PTFE bearing (×2) | 50mm ID flanged | igus direct | 3.00 | Stock |
| 18 | Housing (injection molded) | PA66-GF30, UV stabilized | Contract manufacturer | 22.00 | Tooling 10-14 wks |
| 19 | IP67 M8 connector | 2-pin female panel mount | Würth Elektronik | 2.50 | Stock |
| 20 | PCB (assembled) | 6-layer, 100×80mm | PCBWay/JLCPCB | 18.00 | 3-4 weeks |
| 21 | Misc (fasteners, seals, wiring) | — | — | 5.00 | — |
| **TOTAL** | | | | **~202.20** | |

*Note: Wind sensor (FT742) dominates BOM at €45. Budget alternative (Inspeed, €25) reduces total to ~€182. Target retail: €499–599 (gross margin 55–60%).*

---

## 4. Installation Design

**Mounting:** Cap Pro replaces the existing chimney pot cap entirely.
- Standard EU chimney pot diameters: 150, 175, 200, 250mm
- Stainless steel 316L adapter collar for each diameter
- Tool-free: collar clamps with thumbscrews from outside
- Weight: target <4kg (manageable with extension pole)

**Installation sequence:**
1. Remove existing chimney cap (usually just lifts off)
2. Slide ExoTraft Cap Pro adapter collar over chimney pot
3. Tighten thumbscrews (no tools)
4. Install thermocouple probe in flue access port (or through liner)
5. Optional: route 12V backup cable through liner to indoor power point
6. Download app, scan QR code on unit, done

**Time to install:** 20-40 minutes (with optional cable routing: 60 minutes)

---

## 5. Certifications Required

| Certification | Directive/Standard | Notes | Estimated Cost |
|--------------|-------------------|-------|----------------|
| CE marking | EU Declaration of Conformity | Required for EU market | Included in below |
| EMC | EN 55032, EN 55035 | Radio emissions + immunity | €8,000–12,000 |
| RED (Radio) | EN 300 328, EN 301 489 | WiFi + BT + LoRa | €5,000–8,000 |
| LVD | EN 62368-1 | Electrical safety | €4,000–6,000 |
| IP67 | IEC 60529 | Ingress protection testing | €2,000–3,000 |
| Battery safety | IEC 62133-2 | LiFePO4 pack | €5,000–8,000 |
| RoHS | EU 2011/65/EU | Hazardous substances | €1,000–2,000 |
| **Total estimated** | | | **€25,000–39,000** |

Timeline: 4–6 months from final hardware to certification. Recommend engaging TÜV Rheinland or Nemko (Nordic expertise) as test house.

---

## 6. Manufacturing Approach

**Phase 1 — Prototype (0–6 months):**
- Housing: 3D printed SLS nylon (Protolabs or Shapeways)
- PCB: JLCPCB with PCBA, 10 units
- Total prototype cost: ~€8,000 for 5 functional units
- Purpose: concept validation, sensor calibration, app development

**Phase 2 — Pre-production (6–12 months):**
- Housing tooling: aluminum mold, ~€18,000 (China) or €35,000 (EU)
- Run 100 units for installer pilot and beta testing
- PCB: PCBWay PCBA, 100 units

**Phase 3 — Production (12+ months):**
- Contract manufacturer: Eastern Europe (Poland, Czech Republic) for assembly
- Rationale: lower labor than Denmark, EU supply chain, quality oversight easier than Asia
- Target: monthly batches of 200–500 units
- QC: final test + firmware flash at ExoTraft Denmark facility

---

## 7. IP / Patentability

**Novel combinations patentable as utility models or patents:**

1. **Passive rotating chimney cap with integrated draft fan** — the combination of wind-direction self-orientation and powered draft assistance in one unit has no prior art in consumer chimney products

2. **Chimney-mounted hyperlocal air quality + weather station** — the specific integration of PM2.5, CO, ultrasonic anemometer, and barometric pressure in a chimney cap form factor

3. **TEG-assisted solar + mains hybrid power system for chimney applications** — the specific power architecture combining solar MPPT, TEG heat harvesting, and low-voltage mains input with LiFePO4 storage

**Recommended:** File utility model (brugsmodel) in Denmark first (fast, cheap, €800–1,500) to establish priority date, then file full EPO patent application within 12 months.

**Search:** Check Espacenet for EP applications by Enervex (formerly Exhausto), Schiedel, and Systemair in class F23L17/00 (chimney accessories) and H02J7/00 (battery charging).

---

## 8. The Data Platform Angle

This is what makes Cap Pro more than a hardware product.

**Data generated per unit per day:**
- 1,440 PM2.5 readings (1/minute)
- 1,440 CO readings
- 1,440 wind speed/direction readings
- 1,440 pressure + temperature readings
- Draft event logs (start, stop, RPM, duration)

**At 10,000 units across Denmark:**
- Densest hyperlocal air quality network in Denmark
- More granular than DMI (Danish Meteorological Institute) station network
- Real-time combustion quality by neighborhood, by hour, by weather condition

**Commercial data buyers:**
| Buyer | What they want | Estimated value |
|-------|---------------|----------------|
| Municipality air quality dept. | Winter wood-smoke monitoring, policy compliance | €5,000–20,000/city/year |
| Danish Meteorological Institute | Hyperlocal pressure/wind ground truth | €50,000–200,000/year |
| Tryg / Topdanmark insurance | Chimney fire risk scoring by address | €100,000–500,000/year |
| Jøtul / Morsø / Scan (stove manufacturers) | Real-world combustion performance data | €30,000–100,000/year |
| Research institutions (DTU, AAU) | Urban microclimate research | €20,000–80,000/year grants |

**Architecture needed:**
- Time-series database: InfluxDB or TimescaleDB on Neon/Supabase
- MQTT broker: HiveMQ Cloud or AWS IoT Core
- Anonymization pipeline: strip device ID, replace with grid cell reference
- Data licensing API: authenticated REST endpoint for B2B buyers

---

## 9. Business Case

### Development Investment (NRE)
| Item | Cost |
|------|------|
| Electronics design (PCB, firmware) | €40,000–60,000 |
| Housing design + tooling | €25,000–40,000 |
| Sensor integration + calibration | €15,000–25,000 |
| App development | €30,000–50,000 |
| Certifications | €25,000–39,000 |
| **Total NRE** | **€135,000–214,000** |

*Recommend Cap Pro as Series A milestone product — develop Product 1 (Solar) first to generate revenue and validate market.*

### Unit Economics at Scale

| Volume | BOM | Landed cost | Retail | Gross margin |
|--------|-----|-------------|--------|-------------|
| 500 | €202 | €242 | €499 | 51% |
| 2,000 | €175 | €210 | €499 | 58% |
| 10,000 | €148 | €178 | €499 | 64% |

### Revenue Projection (hardware only)
- Year 1: 500 units × €499 = €250K
- Year 2: 2,000 units × €499 = €1.0M
- Year 3: 5,000 units × €499 = €2.5M
- Data licensing (Year 3+): €200K–500K additional

### Break-even
At €499 retail and 55% gross margin: break-even at ~1,700 units sold (hardware revenue covers NRE).

---

## 10. Positioning Within ExoTraft Product Line

| Product | Price | Install | Power | Target |
|---------|-------|---------|-------|--------|
| ExoTraft Base | €149–199 | DIY, 30 min | Wall outlet | Budget, base of chimney |
| ExoTraft Solar | €299–399 | DIY, 20 min | Solar + TEG | Mid-market, top of chimney |
| **ExoTraft Cap Pro** | **€499–599** | **DIY/Installer, 30-40 min** | **Solar + optional 12V** | **Premium, data platform** |

Cap Pro is the halo product — it makes the whole line look sophisticated and generates the data that funds the next phase of the business.

---

*White paper prepared by Kira ⚡ · March 2026*
*Technical specifications are design targets — validation required through prototype phase.*

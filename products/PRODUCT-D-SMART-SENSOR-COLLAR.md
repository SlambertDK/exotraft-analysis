# Product D: The Trojan Horse — €79 Retrofit Sensor Collar

**Radical business model concept:** Don't sell a new fan. Sell a sensor collar that clips onto ANY existing fan and puts Exodraft on the chimney when they don't own the hardware.

---

## 1. The Concept

The ExoSense collar is a 150–220mm diameter magnetic-attach ring that clips around any existing chimney fan housing — whether it's an Exodraft EFC15 installed 5 years ago, an Enervex fan installed by a competitor dealer, or a generic Chinese fan bought on Amazon.

**What it adds:**
- Draft pressure sensing (Sensirion SDP810)
- Flue temperature (K-type thermocouple)
- PM2.5 particle probe (Sensirion SPS30 sampling tube into flue exhaust)
- CO concentration (Figaro TGS5042)
- Ambient temperature + humidity (Sensirion SHT40)
- Wind speed + direction (FT Technologies FT742-SM miniature)
- WiFi + Bluetooth 5.0 (ESP32-S3)
- 5W solar panel (flexible, clips to fan housing)
- 10Wh LiFePO4 battery
- OTA firmware updates

**It makes any fan intelligent.** The fan still runs as before. ExoSense just observes, measures, and reports.

---

## 2. The Business Model Flip

Traditional model: sell hardware, make margin on hardware.

ExoSense model: sell hardware at or near cost, make money on everything that follows.

### Revenue Streams Per Collar Customer

| Revenue stream | Amount | Timeline |
|----------------|--------|---------|
| Hardware (one-time) | €79 (BOM: ~€48) | Day 0 |
| Intelligence subscription | €5.49/month = €65.88/year | Months 1–60+ |
| Fan upgrade conversion | €299–399 Exodraft Solar | When existing fan fails (avg 4–7 years) |
| Data licensing (pro-rata share of B2B contracts) | €2–5/year per unit | Year 2+ |

**5-year LTV of a collar customer:**
- Hardware margin: €31
- Subscriptions (25% uptake): 25% × €65.88 × 5 = €82
- Fan upgrade (70% conversion when fan fails, avg year 5): 70% × €299 × 0.55 margin × (1/5 per year) = €23/year
- Data: €2–5/year

**5-year LTV: ~€200–250 per collar customer**

**Compare:** A direct hardware sale of €299 fan at 55% margin = €164 one-time. The collar customer is worth more over 5 years, AND Exodraft is now on a competitor's chimney.

---

## 3. The Trojan Horse Data Play

At 50,000 collars deployed across Europe, Exodraft has real-time data from:
- Their own EFC/Draftbooster customers (baseline)
- Enervex customers
- Generic/Chinese fan customers
- Old hardwired fans from 10–15 years ago

**What this data reveals:**
1. **Competitor failure rates:** When do Enervex bearings typically fail? Compare to Exodraft. If Exodraft fans last 20% longer, that's marketing gold with data to back it.
2. **Optimal replacement timing:** "Your fan was installed in 2019 and is showing early bearing wear. Exodraft Solar is available at €249 today." Sent at exactly the right moment.
3. **Chimney type performance:** Which chimney configurations (height, diameter, material) perform best? Informs new product design.
4. **Competitive intelligence:** What draft pressure levels do competitor fans achieve vs Exodraft? Enables objective performance comparison.

---

## 4. Universal Fit Challenge

### The Problem

Chimney fans come in many sizes. Exodraft EFC15: ~180mm housing diameter. Enervex RS14: ~200mm. Generic Chinese fans: 150–220mm range.

### The Solution: 3-Part Adjustable System

**Component 1: Inner ring**
Rigid 316L stainless ring with radial adjustment screws. Adjusts from 130mm to 180mm internal diameter. Rubber-tipped grippers (EPDM, heat-resistant to 120°C) grip the fan housing.

**Component 2: Outer collar**
The ExoSense electronics module clips onto the inner ring via 3 N52 neodymium magnets (8kg pull each = 24kg total). Secure against wind loads. Tool-free attach/detach for maintenance.

**Component 3: Extended adapters**
For non-standard housings: 3D-printed PETG adapters for common fan models (Exodraft EFC15/18/21, Enervex RS series). Available in Exodraft webshop for €9 each.

### Adjustment Mechanism

Inner ring uses a worm-drive tightening collar (like a hose clamp, but precision-machined). Tightens in 30 seconds with a flat-blade screwdriver. Locks with a small set-screw. Doesn't require drilling or permanent modification to existing fan.

---

## 5. Technical Specification

### Sensor Suite

| Sensor | Parameter | Model | Interface | Cost |
|--------|-----------|-------|-----------|------|
| Differential pressure | Draft Pa | Sensirion SDP810-500Pa | I²C | €6.50 |
| Flue temperature | °C | K-type + MAX31855K | SPI | €4.00 |
| PM2.5 | µg/m³ | Sensirion SPS30 | I²C/UART | €12.00 |
| CO | ppm | Figaro TGS5042 | Analog | €8.00 |
| Temp/humidity | °C / %RH | Sensirion SHT40 | I²C | €2.50 |
| Wind | m/s + direction | FT Technologies FT742-SM | RS232 | €45.00 |

### Power System

| Component | Specification | Cost |
|-----------|---------------|------|
| Solar panel | 5W flexible ETFE, clips to housing | €9.00 |
| MPPT | TI BQ25504 | €2.50 |
| Battery | 10Wh LiFePO4, 2× CATL IFR18650 | €7.00 |
| BMS | Seiko S-8261 | €1.20 |

### Electronics

| Component | Specification | Cost |
|-----------|---------------|------|
| MCU | ESP32-S3-WROOM-1 (8MB) | €3.20 |
| PCB assembled | 4-layer, 70×55mm | €11.00 |

### Mechanical

| Component | Specification | Cost |
|-----------|---------------|------|
| Inner ring (stainless) | 316L, worm-drive, 130–180mm | €6.00 |
| Outer collar housing | PA66-GF30, UV-stabilized | €8.00 |
| N52 magnets (×3) | 30×10×5mm, 8kg pull each | €3.00 |
| Misc (screws, seals, cable) | — | €2.50 |

### **Total BOM: ~€131 — wait, that's too high for €79 retail**

Adjustment: the FT Technologies wind sensor at €45 is the BOM killer for this price point. Two options:

**Option A (Full sensor suite):** Keep wind sensor. Retail €129. Position as premium unit.

**Option B (Core sensors only, no wind):** Remove FT742-SM. BOM: ~€86. Add wind-equivalent via phone GPS/weather API correlation. Retail €79. "ExoSense Core."

**Recommendation:** Launch with Option B at €79. Add Option A as "ExoSense Pro" at €129 for users who want weather station functionality.

### Revised Core BOM (no wind sensor)

| Category | Cost |
|----------|------|
| Sensors (no wind) | €34.20 |
| Power system | €19.70 |
| Electronics | €14.20 |
| Mechanical | €19.50 |
| **Total Core BOM** | **€87.60** |

At 5k volume, BOM reduces to ~€65. Retail €79 = slim margin at launch, improving over time.

---

## 6. Privacy and Data Ethics

**Critical:** Collar observes performance of competitor-manufactured products. This creates potential brand/legal sensitivities.

Mitigations:
1. All telemetry is tied to device ID, not fan brand/model in the public data
2. User Terms of Service explicitly disclose that anonymous performance data is collected and may be used for product improvement and licensed to third parties (with opt-out option)
3. No identifying data about competitor product performance is disclosed publicly — internal use only for Exodraft R&D
4. GDPR: data processing agreement, data minimization, user data export/delete in app

**Patent protection:** File patents on the specific combination of (chimney fan sensor collar + cloud combustion analysis + upgrade conversion algorithm). This is novel as an application even if individual components are not.

---

## 7. Go-to-Market

**Phase 1: Exodraft customers**
Sell ExoSense Core to existing Exodraft customers via email + dealer network. "Make your existing fan smart — €79." High trust, easy sell. Target: 2,000 units in year 1.

**Phase 2: Open market**
Amazon, exodraft.com webshop. Target: chimney fan owners of any brand. "Works with any chimney fan." 

**Phase 3: Installer channel**
Train dealers to recommend ExoSense at every chimney fan service visit — competitor's fan or not. Installer earns €15 fitting fee. Exodraft earns the platform customer.

---

## 8. 5-Year Financial Model

| Year | Units sold | Subscription ARR (25%) | Data licensing | Fan upgrades | Total revenue |
|------|-----------|----------------------|----------------|--------------|---------------|
| 1 | 2,000 | €33,000 | €0 | €0 | €95,000 |
| 2 | 8,000 | €165,000 | €20,000 | €30,000 | €605,000 |
| 3 | 20,000 | €495,000 | €100,000 | €120,000 | €1,430,000 |
| 4 | 35,000 | €990,000 | €250,000 | €280,000 | €2,500,000 |
| 5 | 50,000 | €1,650,000 | €500,000 | €500,000 | €3,900,000 |

*Hardware revenue included in total. Fan upgrade assumes 10% of 4+ year old collar base converts annually.*

**This is the highest-LTV business model in this document.** The hardware is almost free. The value is the platform.

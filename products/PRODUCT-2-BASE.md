# Exodraft Base — Product White Paper
**Version 1.0 | March 2026 | CONFIDENTIAL**

---

## 1. Executive Summary

### Product Vision
Exodraft Base is a chimney draft booster that mounts at the **base** of the chimney — at the stove or fireplace connection point — not on the roof. It pushes air upward from below using inline fan technology, connects to a standard Schuko wall outlet, and can be fully installed by the homeowner in 30 minutes without any tools beyond a screwdriver. No roof access. No roofer. No electrician. No permits.

### Why Base-Mounting Works Aerodynamically
Natural chimney draft depends on buoyancy: hot gas is less dense and rises. A base-mounted fan augments this same upward flow by adding mechanical pressure at the entry point. The Venturi geometry of the Exodraft Base unit accelerates airflow through a narrowed inlet section, reducing static pressure and drawing combustion gases more forcefully into the flue column. This works most effectively in **short-to-medium chimneys** (up to 6–8 meters) where the total pressure deficit is modest and a 100–200 Pa boost resolves the problem entirely.

### Target Customer
- Homeowners with modern stoves or inserts connected to masonry or liner chimneys (6m or less)
- Renovation scenarios where roof access is impractical or expensive
- Customers who want the simplest, cheapest solution to draft problems
- Installers seeking a quick-fix product to offer alongside stove sales

### Key Wins
1. **Normal wall outlet** — Schuko plug, 230V, no electrician, no permit
2. **Ground-level installation** — no ladder, no roofer, no roof insurance issues
3. **€149–199 entry price** — the affordable entry into the Exodraft ecosystem

### Why This Product Wins
Exodraft Base solves the roof access problem completely by moving the technology to where the customer already is. At €149–199 with a Schuko plug and a 30-minute DIY install, it removes every single barrier that prevents a hesitant customer from buying.

---

## 2. Technical Architecture

### 2.1 Aerodynamic Design

**Operating Principle:** The Exodraft Base unit is an inline duct fan with a custom Venturi inlet section. When installed between the stove outlet and the first chimney section, it:
1. Accelerates incoming flue gas via a converging inlet (Venturi effect)
2. Adds kinetic energy via the impeller
3. Delivers pressurized gas into the chimney column
4. The chimney's natural thermal buoyancy amplifies the added pressure

**Geometry Targets:**

| Parameter | Specification |
|-----------|--------------|
| Inlet diameter | 150mm / 175mm / 200mm (3 variants) |
| Venturi contraction ratio | 1.5:1 (inlet area to throat) |
| Target static pressure rise | 100–300 Pa (adjustable via speed control) |
| Target flow rate | 100–300 m³/h depending on chimney size |
| Unit overall length | 180–220mm (inline section) |

**Inline vs. Centrifugal Fan:**

| Parameter | Inline Axial | Centrifugal (radial) |
|-----------|-------------|----------------------|
| Pressure rise | Lower (typically 30–150 Pa) | Higher (100–500 Pa) |
| Flow rate | Higher at low pressure | Lower but more pressure authority |
| Efficiency | Higher for low-pressure applications | Better for high-resistance systems |
| Form factor | Compact, inline (ideal for this application) | Larger, offset discharge |
| Noise | Lower at equal flow | Higher (spiral housing) |
| **Recommendation** | **Primary design choice** | Fallback for deep/narrow chimneys |

**Conclusion:** Inline axial design with Venturi inlet for most installations. A centrifugal variant (higher pressure model) for narrow masonry chimneys can be offered as "Exodraft Base Pro."

**CFD Simulation Requirements:**
- 3D CFD validation required before tooling (estimated: 15–25 hours computational, €3,000–6,000 consultant time)
- Validate: pressure rise curve, flow uniformity, turbulence at impeller inlet
- Critical parameter: avoid flow separation at Venturi throat (radius blend critical)
- Recommended solver: OpenFOAM (open source) or ANSYS Fluent (commercial)

---

### 2.2 Motor Specification

**Motor Requirements:** Unlike Product 1 (battery-powered), Exodraft Base has mains power available. This opens the door to higher-power AC motors or efficient EC (electronically commutated) motors with external drive electronics.

**Option A: 230V AC Induction Motor**
- Simple, cheap, proven
- Fixed speed (without additional electronics)
- ebm-papst K3G200 series or equivalent
- Disadvantage: no variable speed without separate VFD; efficiency lower at partial load

**Option B: 24V DC EC Motor with External PSU (Recommended)**
- Variable speed via PWM — ideal for draft modulation
- Higher efficiency (EC motor = brushless DC = >85% efficiency)
- Lower noise at reduced speed
- Integrated into smart controller

**Recommended Motor — Primary:**

| Parameter | Specification |
|-----------|--------------|
| Supplier | ebm-papst |
| Model series | RG225 or K3G225 (EC inline fan) |
| Voltage | 230V AC input (EC motor with integrated drive) OR 24V DC |
| Power | 45–80W (variable via EC drive) |
| Speed range | 400–2200 RPM |
| Static pressure | Up to 350 Pa |
| Flow range | 100–450 m³/h |
| Efficiency | ≥85% at design point |
| IP rating | IP44 (protected by stainless enclosure in application) |
| Operating temp | Up to 60°C ambient (higher with thermal isolation) |

**Alternative — Ziehl-Abegg:**
- Model: RADAX 180L or FB series inline
- Similar specs; strong European market presence; good for volume OEM
- Slight cost premium vs. ebm-papst; better availability in some markets

**EC Motor advantage:** Built-in speed controller responds to 0–10V or PWM signal from Exodraft Base controller PCB — no need for separate VFD.

---

### 2.3 Heat Management

**Challenge:** The Exodraft Base unit is installed immediately downstream of the stove outlet. Flue gas temperatures at this point can reach **150–350°C** during active burning. The fan motor must be protected.

**Thermal Isolation Strategy:**

| Layer | Material | Function |
|-------|----------|---------|
| Flue-contact surfaces | 316L stainless steel, 2mm | Acid/condensate resistant, 900°C capable |
| Thermal break ring | Calcium silicate board (Skamol), 10mm | 0.2 W/m·K conductivity; blocks heat transfer to motor housing |
| Motor housing | Die-cast aluminum or PA46-GF30 (high-temp nylon) | Max 140°C rated |
| Air gap | 15mm gap between flue duct and motor mounting flanges | Natural convection cooling |
| Motor temp sensor | NTC 100kΩ on motor winding | Triggers speed reduction / shutdown if >100°C winding temp |

**Maximum Operating Temperatures:**

| Component | Max Rated Temp | Design Limit |
|-----------|---------------|--------------|
| 316L stainless flue section | 900°C | 400°C (typical operating) |
| Calcium silicate thermal break | 1000°C | 400°C |
| Motor winding (Class F) | 155°C | 100°C (with thermal shutdown at 115°C) |
| Motor bearing | 120°C | 90°C |
| Controller PCB | 85°C | 70°C |

**Condensation Management:** At base installation, during cold starts, flue gases cool before reaching operating temperature. Condensation (acidic — pH 3–4 from wood combustion) will form on all flue-contact surfaces. All surfaces exposed to condensate must be 316L stainless or equivalent acid-resistant material. A condensate drain port (pluggable) at the lowest point of the unit is recommended.

---

### 2.4 Connection to Chimney System

**Compatibility:**

| Chimney Size | Inner Diameter | Exodraft Base Variant |
|-------------|----------------|----------------------|
| Small (wood stove) | 150mm | Base-150 |
| Standard | 175mm | Base-175 |
| Large (fireplace) | 200mm | Base-200 |
| XL masonry | 250mm | Base-250 (centrifugal variant) |

**Quick-Connect Collar System:**
- Stainless steel bandclamp system (similar to Schiedel/Jeremias liner collars)
- Slides over existing 150/175/200mm flue pipe section
- Tightens with single T-bolt clamp: no tools beyond fingers for initial fit; torque with screwdriver for final
- Sealing: 1000°C-rated ceramic fiber rope gasket
- Installation depth: replaces a single standard 250mm pipe section

**Integration with Existing Liner Systems:**
- Compatible with: flexible stainless liner, rigid stainless sections, ceramic flue liner with adapter
- Not compatible with: pre-1970 unlined masonry without adapter (requires liner installation first — separate product opportunity)

---

### 2.5 Controls

**Control Tiers (two product variants):**

**Tier 1: Exodraft Base Standard (€149)**
- Simple thermostat control: probe inserted into flue, fan starts when flue temp >60°C
- Manual speed dial (3-position: Low / Medium / High)
- No app, no connectivity
- Single LED indicator: Green (running), Yellow (standby), Red (fault/overheat)
- Backdraft protection: pressure switch cuts power if positive pressure detected

**Tier 2: Exodraft Base Smart (€199)**
- ESP32-S3 controller
- Sensirion SDP810 differential pressure sensor (continuous draft monitoring)
- Auto-modulating speed (maintains target draft pressure, -10 to -15 Pa)
- K-type thermocouple for flue temp (accurate to ±1°C)
- Bluetooth 5.0 LE + app integration
- Safety features:
  - Automatic shutdown: motor temp >115°C
  - Backdraft alarm: push notification
  - Low-draft alert: notification if draft falls below threshold
  - Service reminder after 2,000 hours operation

**Backdraft Safety Logic:**
```
Monitor: differential pressure sensor every 100ms
Normal: pressure = -8 to -20 Pa (negative = correct draft direction)
Warning: pressure 0 to -5 Pa for >5 seconds → reduce fan speed
Alarm: pressure >+2 Pa (positive = backdraft) → emergency fan STOP + app alert
Reset: manual reset required after backdraft event
```

---

## 3. Bill of Materials (BOM)

*Pricing: estimated 1,000-unit volume. EUR. 230V EC motor variant (Base Smart).*

| # | Component | Specification | Primary Supplier | Backup Supplier | Unit Cost (€) | Lead Time | Notes |
|---|-----------|--------------|-----------------|-----------------|---------------|-----------|-------|
| 1 | EC inline fan motor | ebm-papst RG225 or K3G225, 230V EC | ebm-papst | Ziehl-Abegg RADAX | 28.00–38.00 | 8–10 wks | Core component; negotiate OEM terms |
| 2 | Impeller (if separate) | PA46-GF30 axial, 6-blade, custom | In-house mold | Outsource | 3.00 | — | Tooling: €6k |
| 3 | Flue section (stainless) | 316L SS, 150/175/200mm × 250mm | Stainless supplier EU | Metal shop local | 8.00–12.00 | 2–3 wks | Hydroform or spin-formed |
| 4 | Thermal break ring | Skamol CalSil board, custom cut | Skamol A/S (Danish!) | Promat | 2.50 | 2 wks | Skamol is Danish — local supplier advantage |
| 5 | Venturi inlet section | 316L SS, custom spun/formed | Metal fabrication | Die-cast zinc option | 6.00–9.00 | 4–6 wks | Venturi geometry critical; validate with CFD first |
| 6 | Controller PCB | ESP32-S3, pressure sensor, relay | JLCPCB / CMC | PCBWay | 9.00 | 3–5 wks | Base Standard: simpler PCB ~€4.00 |
| 7 | MCU | ESP32-S3-WROOM (Smart variant) | Mouser/LCSC | — | 3.50 | 2–4 wks | Omit in Standard variant |
| 8 | Pressure sensor | Sensirion SDP810-500Pa | Sensirion | Mouser | 9.00 | 4–6 wks | Smart variant only |
| 9 | Thermocouple + amp | K-type + MAX31855 SPI amp | Mouser | LCSC | 4.50 | 2 wks | High accuracy for flue temp |
| 10 | Thermostat (Standard) | Honeywell AQ6 or equivalent, 60°C | Honeywell/RS | — | 3.50 | 1 wk | Standard variant only |
| 11 | Motor temp NTC | 100kΩ NTC on motor winding | EPCOS / TDK | — | 0.40 | 1 wk | Safety critical |
| 12 | Bandclamp collar | 316L SS T-bolt clamp, 150/175/200mm | Tridon | Norma Group | 3.50 | 2 wks | One included; customer selects size |
| 13 | Ceramic fiber gasket | 1000°C rope gasket, 10mm dia | Thermal Ceramics | Unifrax | 0.80 | 1 wk | Around flue connection |
| 14 | Motor housing | Die-cast aluminum or PA46-GF30 | China die-cast | — | 5.50 | 6–8 wks | Tooling: €10k |
| 15 | Outer shroud | PA66-GF30 injection mold | China injection | — | 4.00 | — | Tooling shared with housing |
| 16 | Schuko power cord | H05VV-F 3×1.5mm², 1.5m, with plug | Lapp Group | Belden | 2.80 | 1 wk | EU certified; no electrician needed |
| 17 | Backdraft pressure switch | Honeywell C6097, adjustable | RS Components | Honeywell | 4.50 | 1 wk | Standard variant safety backup |
| 18 | LED indicators | 5mm, RGB or tri-color, panel mount | Vishay / Kingbright | — | 0.30 | 1 wk | |
| 19 | Speed control knob | Rotary potentiometer, panel mount | Alps Electric | Bourns | 0.80 | 1 wk | Standard variant |
| 20 | Stainless hardware | M5/M6 screws, 316L, 20-piece kit | Fastenright EU | — | 1.20 | 1 wk | |
| 21 | Condensate drain | Silicone plug, Ø12mm | Generic | — | 0.20 | 1 wk | |
| 22 | Packaging | Recycled cardboard | Local EU | — | 2.20 | 2 wks | |
| 23 | Quick-start guide | 4-page fold, 6 languages | Local print | — | 0.40 | 1 wk | |
| **TOTAL — Base Standard** | | | | | **€62–75** | | Simpler PCB, thermostat only |
| **TOTAL — Base Smart** | | | | | **€75–92** | | Full smart controller |

**BOM Notes:**
- EC motor is the biggest cost driver (~40% of BOM)
- Skamol thermal break is a local Danish supplier — good story for "Danish engineering"
- Standard variant hits BOM target comfortably; enables aggressive €149 price point
- Smart variant at €92 BOM supports €199 retail (>53% gross margin)

---

## 4. Installation Design

### 4.1 Physical Installation

Exodraft Base fits between the stove/fireplace outlet and the first chimney section. It replaces a standard 250mm straight pipe section.

**Installation Steps (Target: ≤30 minutes):**

1. **Ensure fire is fully extinguished and flue is cold** (safety step — written prominently in manual)
2. Remove existing pipe section at stove outlet connection point
3. Slide bandclamp collar onto stove outlet pipe
4. Fit Exodraft Base unit — inlet end to stove outlet, outlet end to chimney pipe above
5. Tighten both bandclamp collars with screwdriver (T-bolt, ¼ turn to secure)
6. Route Schuko power cable to nearest wall outlet (cable included, 1.5m)
7. Light fire (small test fire); verify fan activates and draft is strong
8. For Smart variant: download app, scan QR, configure via Bluetooth

**Tools required:** Flathead screwdriver only. Optionally pliers for cable routing.

### 4.2 Electrical

- **Power supply:** 230V AC, standard Schuko plug (Type F, EU standard)
- **Power consumption:** 45–80W (less than a light bulb)
- **Cable included:** 1.5m H05VV-F 3×1.5mm² with moulded Schuko plug — factory installed, CE certified
- **No electrical work required** — plug into standard wall outlet
- **Current draw:** 0.2–0.35A (trivial; no dedicated circuit needed)

### 4.3 Weight & Dimensions

| Parameter | Value |
|-----------|-------|
| Weight | ~2.8 kg (Base-150); ~3.2 kg (Base-200) |
| Overall length | 220mm (inline) |
| Outer diameter | Varies: 165mm / 190mm / 215mm for 150/175/200mm variants |
| Mounting | Self-supporting via chimney pipe connections (no additional brackets) |

---

## 5. Regulatory Considerations

### CE Marking Requirements

| Directive | Scope | Key Standards |
|-----------|-------|---------------|
| Low Voltage Directive (LVD) 2014/35/EU | 230V electrical device | EN 62368-1; EN 60335-series |
| EMC Directive 2014/30/EU | Electrical interference | EN 55014-1 (emissions); EN 55014-2 (immunity) |
| Radio Equipment Directive 2014/53/EU | Smart variant only (BLE) | EN 300 328 (WiFi/BT); EN 301 489 |
| Ecodesign Regulation (EU) 2019/1781 | Electric motors ≥0.12kW | EC motor meets IE3 equivalent efficiency requirements |
| RoHS Directive 2011/65/EU | Hazardous substances | EN IEC 63000 |

### Chimney System Standards

| Standard | Scope | Impact on Exodraft Base |
|----------|-------|------------------------|
| EN 1856-1:2009 | Chimneys — metal chimney systems | All flue-contact metal components must comply; 316L stainless specified here meets requirements |
| EN 15287-1:2007 | Chimney design and installation | Advisory for installation guidance; not product certification |
| DIN 18160 (Germany) | Chimney systems | German market entry requirement — important for largest EU market |
| NT Fire 027 (Nordic) | Fire safety for chimney components | Nordic-specific; test at SP Technical Research Institute (Sweden) |

### Fire Safety Component Ratings
All components in the flue-contact zone are rated for continuous 400°C operation:
- 316L stainless: continuous 870°C; intermittent 925°C
- CalSil thermal break: continuous 1000°C
- Ceramic fiber gasket: 1260°C rated
- Motor (not in flue contact): IP44, Class F (155°C winding)

### Estimated Certification Cost

| Item | Cost (€) |
|------|----------|
| LVD + EMC testing | 8,000–12,000 |
| RED testing (Smart variant BLE) | 5,000–8,000 |
| EN 1856-1 flue component qualification | 4,000–6,000 |
| NT Fire 027 fire safety test | 2,000–3,500 |
| RoHS compliance | 1,000–2,000 |
| **Total** | **€20,000–31,500** |

*Note: Certification is simpler than Product 1 (no battery, no TEG). Can achieve CE marking in 3–4 months.*

---

## 6. Limitations (Honest Assessment)

Exodraft Base is not the right solution for every chimney. The following limitations must be clearly communicated:

### 6.1 Physics Limitations

**Tall chimneys (>8 meters):** Natural thermal draft increases with chimney height. Very tall chimneys (8+ meters) typically generate sufficient natural draft and rarely need assist. If they do, the pressure requirements exceed what a base-mounted inline fan can cost-effectively provide — top-mounted fan (Product 1) is more effective for tall systems.

**Very narrow masonry chimneys:** Old masonry chimneys may have rough internal surfaces and tight cross-sections. These create high resistance that requires more pressure authority. Base-mounted axial fan may not provide sufficient pressure rise — centrifugal variant (Base Pro) is recommended.

**Sealed combustion appliances:** Some modern high-efficiency stoves use balanced flue (room-sealed) systems with co-axial pipes. Exodraft Base is not compatible with these installations — they are purpose-designed for specific flue arrangements.

### 6.2 Thermal/Condensation Limitations

**Cold climate condensation risk:** In cold climates, base-mounted installation means the entire chimney column is cold at startup. More condensation forms than with top-mounted fans. Customers must:
- Burn dry wood only (moisture <20%)
- Use the condensate drain port during first season
- Consider chimney insulation wrap for very cold outdoor masonry chimneys

**Backpressure from long chimney runs:** Unusually long horizontal runs (>2 meters) between stove and chimney increase resistance significantly. Exodraft Base can compensate up to ~2.5m horizontal run; beyond this, recommend re-routing or Product 1.

### 6.3 Compatibility Limitations

| Chimney Type | Compatible? | Notes |
|-------------|-------------|-------|
| Flexible stainless liner, 150–200mm | ✅ Yes | Primary target |
| Rigid stainless sections | ✅ Yes | Easy install |
| Masonry with liner | ✅ Yes | Adapter may be needed |
| Unlined old masonry | ⚠️ Conditional | Requires liner first |
| Balanced flue (room-sealed) | ❌ No | Not compatible |
| Oil/gas boiler flues | ❌ No | Not designed/certified for this |
| Multi-fuel chimneys | ⚠️ Check | Wood only; verify fuel compatibility |

### 6.4 Marketing Language Guidance
Avoid claiming Exodraft Base "works for all chimneys" — this will generate returns and negative reviews. Instead: "Exodraft Base works for 80% of modern wood-burning installations. Unsure? Use our chimney checker tool." (A 5-question online wizard can qualify/disqualify prospects before purchase.)

---

## 7. Business Case

### Why This Product Should Launch First

Exodraft Base has:
- **Lower NRE cost** (~€90,000–130,000 vs. €215,000 for Solar)
- **Simpler certifications** (no battery, no TEG, no solar)
- **Faster time to market** (12–15 months vs. 18–24 for Solar)
- **Established technology** (inline EC fans are a proven category)
- **Lower retail price** → larger addressable market → faster initial traction

**Use Base to:** Generate cash flow, validate the brand, build installer relationships, and fund Solar development.

### Pricing Model

| SKU | Retail (€) | BOM (1k vol, €) | Gross Margin | Channel |
|-----|-----------|-----------------|--------------|---------|
| Exodraft Base Standard | 149 | 68 | 54% | Online + hardware stores |
| Exodraft Base Smart | 199 | 88 | 56% | Online + stove dealers |
| Upgrade kit (Standard → Smart) | 49 | 15 | 69% | Accessory upsell |

*54–56% gross margin is solid for an entry hardware product. Aim to improve to 60%+ at 5k+ volume.*

### Development Cost (NRE)

| Item | Cost (€) |
|------|----------|
| Electronics design + firmware | 20,000–30,000 |
| Mechanical design (housing, flue section) | 15,000–20,000 |
| Tooling (housing + flue section) | 20,000–30,000 |
| CFD simulation | 4,000–6,000 |
| Prototype builds | 8,000–15,000 |
| Certifications | 20,000–31,500 |
| App (minimal — shared with Solar) | 5,000–10,000 |
| **Total NRE** | **€92,000–142,500** |

**Mid-point: ~€117,000** — achievable with seed funding or early revenue.

### Break-Even Analysis

| Scenario | Units to Break Even |
|----------|---------------------|
| NRE €117k, GM €75/unit (Standard) | **1,560 units** |
| NRE €117k, GM €100/unit (Smart) | **1,170 units** |
| Blended (50/50 mix, GM €87) | **1,345 units** |

Break-even at ~1,200–1,600 units is achievable within Year 2.

### Revenue Projections

| Year | Units | Revenue | Gross Profit |
|------|-------|---------|--------------|
| Year 1 | 400 | €68,000 | €35,000 |
| Year 2 | 1,500 | €255,000 | €130,000 |
| Year 3 | 4,000 | €680,000 | €360,000 |
| Year 4 | 8,000 | €1,360,000 | €740,000 |

*Assumes 60% Smart mix at €199 average. Y3 includes German market launch.*

### Product Ecosystem Position

```
Exodraft Base (€149–199)
  └── Entry product, accessible, high volume
  └── Builds brand, installer base, data
  └── Customer upgrades to Solar or Cap Pro for harder cases
  └── Installer recommends Base for new stove sales

Exodraft Solar (€299–399)
  └── Premium, hard-to-reach chimneys, vacation homes
  └── Buyers: customers who tried Base but need more, or don't want any wiring

Exodraft Cap Pro (€499–599)
  └── New builds, renovations, data-hungry installers
  └── Professional channel — installer-sold
```

### Risk Factors
1. **Competition from HVAC suppliers** — ebm-papst or similar could enter the consumer market directly; Exodraft's advantage is the brand, app, and chimney-specific design
2. **Chimney compatibility returns** — manage with online chimney checker tool and clear compatibility guide; target <5% return rate
3. **Condensation warranty claims** — manage with clear wood moisture requirements in manual
4. **Motor thermal failure** — addressed by multi-layer thermal protection; design conservatively

---

*Document prepared by Exodraft Product Team | Version 1.0 | March 2026*
*Classification: Confidential — Internal Use Only*

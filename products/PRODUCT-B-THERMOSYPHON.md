# Product B: Heat-Driven Draft — TEG Self-Powered Chimney Fan

**The dream:** A chimney fan powered entirely by the chimney's own waste heat. No solar. No battery. No wire. Just physics.
**The reality:** Partial — TEG + solar hybrid is very achievable. Pure TEG self-sufficiency is not (yet).

---

## 1. Honest Thermosyphon Analysis

### Can a Thermosyphon Enhance Chimney Draft?

A thermosyphon is a closed-loop heat transfer system driven by buoyancy: working fluid heats at one end, rises, cools at the other end, falls back. No pump. No moving parts.

**Applied to chimney draft enhancement — the honest answer:**

The chimney flue gas column *is already* a thermosyphon. Hot gas rises because it is less dense than surrounding air. A secondary thermosyphon loop wrapped around the chimney would need to either:

- Extract heat to create a temperature differential → reduces flue gas temperature → reduces buoyancy in primary stack → net effect neutral or negative
- Transfer heat from chimney middle to top → zero-sum: amplifies at top, reduces in middle
- Create pressure differential via fluid velocity → thermosyphon fluid velocity: 0.1–0.5 m/s → far too low for meaningful jet pump action

**Verdict:** A secondary thermosyphon loop cannot meaningfully add to stack draft. The second law prevents more work output than energy input. This is not a viable draft enhancement mechanism.

**What thermosyphons ARE good for:** Heat transfer, not work. The chimney's heat can be harvested and converted to electricity — that's the TEG opportunity.

---

## 2. Thermoelectric Generation (TEG) — The Real Opportunity

### Physics

The Seebeck effect: a temperature differential across a semiconductor junction generates a voltage. TEG modules (bismuth telluride, Bi₂Te₃) are the commercial embodiment.

**Available energy in a residential chimney:**

Flue surface temperature: 150–300°C (wood fire, full burn)
Ambient temperature: −5°C to +20°C (Nordic range)
Available ΔT: 130–305°C

**Heat flux from flue pipe surface:**
For a 150mm diameter flue pipe, natural convection + radiation:
- Heat flux: ~500–800 W/m² at 200°C surface temp
- Available surface area for TEG collar (0.5m length): π × 0.15m × 0.5m = 0.236 m²
- Total available heat: 0.236 × 650 W/m² ≈ 153W thermal

**TEG efficiency at ΔT = 195°C (200°C flue, 5°C ambient):**

Carnot maximum: (473-278)/473 = 41%
Practical TEG efficiency: 5–8%

**TEG electrical output:**
153W thermal × 6% efficiency = ~9W electrical

Wait — that's more than the previously cited 0.6W. The difference: proper collar design with good thermal contact and efficient cold-side heat sinking can achieve much closer to theoretical. The 0.6W figure assumes poor contact and no active cold-side cooling.

**With optimized design:**
- Hot side: direct contact brazing to flue pipe surface (not clamped — braized or pressed with thermal paste)
- Cold side: aluminum heat sink fins with natural convection (or small fan)
- 4× Ferrotec 9500/127/060B modules in parallel
- Realistic output: 3–6W continuous during full-burn operation

**The EFC15 fan draws 40–60W. TEG at 3–6W cannot power it directly.**

However, 3–6W is very significant for:
1. Charging a LiFePO4 battery during fire operation
2. Powering the electronics/sensors continuously (ESP32 + sensors: ~0.8W)
3. Extending battery runtime dramatically in winter when solar is limited

### TEG Power Budget

| Scenario | TEG Output | Fan State | Battery |
|----------|-----------|-----------|---------|
| Full burn, 250°C flue | 5–6W | Running on battery | Charging at 4W net |
| Medium burn, 180°C flue | 2–3W | Sensors only | Charging at 2W net |
| Cold start, 80°C flue | 0.3–0.5W | Sensors only | Discharging slowly |
| No fire | 0W | Standby | Solar only |

With a 40Wh LiFePO4 battery, a 2-hour evening fire at 4W TEG net charging adds 8Wh — covering ~2 additional hours of fan operation after the fire dies down.

---

## 3. Combustion Air Preheating

If pure TEG self-sufficiency isn't achievable, combustion air preheating is a lower-tech approach that genuinely improves draft without any electricity.

**Principle:** Run incoming combustion air through a counter-flow heat exchanger on the flue pipe before it enters the stove. Heated air enters the combustion zone hotter → higher flame temperature → lower flue gas density → stronger natural buoyancy.

**Quantification:**
- Incoming air: 20°C → preheated to 80°C (conservative for a simple copper coil heat exchanger)
- Effect on stack effect: T_flue effectively increases relative to T_intake → ΔP_stack improves ~8–12%
- For a marginal chimney that needs 5–10 Pa additional draft: preheating alone may be sufficient

**Engineering:** Copper coil (8mm soft copper tube) wound around flue pipe inside the room. Connects to stove air intake. No electricity. No moving parts. Negligible installation complexity.

**BOM for preheater option:** 3m copper tube (€8) + 2 push-fit connectors (€4) + insulation wrap (€3) = €15 total. This could be a €49 accessory product.

---

## 4. Real TEG Component Specification

### Primary Module: Ferrotec 9500/127/060 B

| Parameter | Value |
|-----------|-------|
| Dimensions | 40 × 40 × 3.6mm |
| Hot side max temp | 300°C |
| Cold side max temp | 50°C |
| Seebeck coefficient | ~53 mV/K |
| Internal resistance | ~2.0 Ω |
| Peak power (ΔT=200°C) | ~3.5W per module |
| Thermal cycling rating | 200,000+ cycles |
| Unit cost (1k volume) | €12–16 |
| Supplier | Ferrotec GmbH (EU), Kryotherm (RU/EU) |

### Alternative: Hi-Z Technology HZ-14

- Higher output per module (~2.5W at ΔT=150°C)
- Brazed construction — better thermal cycling life
- Unit cost: ~€25
- Better choice for high-cycle residential application

### MPPT Integration

TEG output varies with fire intensity. MPPT controller tracks maximum power point:
- **TI BQ25504:** Ultra-low power boost charger, handles variable input from 50mV up, charges LiFePO4 directly
- Inputs: TEG + solar in parallel on same MPPT bus
- TEG acts as nighttime/winter solar supplement

---

## 5. Product Specification: Exodraft TEG-Solar

The complete self-powered product combining both technologies:

| Component | Specification | Purpose |
|-----------|---------------|---------|
| Solar panel | 10W flexible, ETFE, Solbian | Primary daytime power |
| TEG collar | 4× Ferrotec 9500/127/060B | Evening/winter power from fire heat |
| Battery | 40Wh LiFePO4, CATL IFR26650 | Energy buffer |
| BMS | Seiko S-8261, −20°C rated | Battery protection |
| MPPT | TI BQ25504 | Combined solar+TEG charging |
| Cold-side heatsink | Aluminum finned, 200×150mm | TEG efficiency (critical) |
| Hot-side contact | Thermal paste + compression clamp | Heat transfer to TEG |

**Total BOM for TEG-Solar system:** ~€155–175 (vs €134 for solar-only)
**Retail premium for TEG version:** €50–80 → Retail €349–449

---

## 6. Business Case

| Metric | Solar Only | TEG-Solar |
|--------|-----------|-----------|
| BOM | €134 | €165 |
| Retail | €299 | €399 |
| Gross margin (1k) | 55% | 59% |
| Key advantage | Lower cost | Nordic winter reliability |
| Target market | Central/Southern Europe | Scandinavia, UK, Germany |

**Recommendation:** Launch Solar first. Offer TEG-Solar as Nordic/premium SKU from month 12.

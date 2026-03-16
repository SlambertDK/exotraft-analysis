# Product A: Push Don't Pull — Base-Mounted Combustion Blower

**Radical inversion:** 67 years of Exodraft history assumes suction from the top. This product questions that assumption from first principles.

**The win:** Normal 230V wall outlet. No roofer. No electrician. Self-install in 30 minutes.

---

## 1. The Physics

### Suction vs Positive Pressure

Current Exodraft paradigm: negative gauge pressure at chimney top (Δp ≈ −5 to −20 Pa), pulling flue gases upward. The pressure differential propagates downward — reducing pressure at the combustion zone slightly below atmospheric, which is what draws air into the stove through intake vents.

Positive pressure injection does the opposite: creates a pressure gradient above atmospheric at the base. The central risk: if the stove firebox reaches Δp > 0, flue gas seeks the path of least resistance — which may be the air intake, glass door seals, or ash door.

**Key safety distinction:**
- Suction mode: entire flue column at negative gauge pressure → fail-safe by nature
- Positive pressure mode: requires engineering to maintain negative pressure at appliance

This is solvable. Industrial forced-draft burners have operated on positive pressure principles for 60+ years.

### Ejector/Venturi Principle

If a blower injects a high-velocity jet of air at the chimney base through a nozzle, surrounding flue gas is entrained by viscous drag and the pressure drop at the nozzle throat. This is the jet pump (ejector) principle.

**Ejector efficiency for gas-gas systems:** 10–30% (ratio of entrained mass flow to motive mass flow at the same pressure rise).

For residential chimneys:
- Motive flow (injected air): ~50 m³/h at 20 m/s jet velocity
- Entrained flow (flue gas): ~10–15 m³/h additional draft
- Pressure rise achievable: 5–15 Pa — comparable to natural draft

The ejector effect **can** partially replace suction, but efficiency is low compared to a direct fan on the flue stream. However: the blower handles only ambient-temperature air, not hot corrosive flue gas. This changes the engineering requirements dramatically.

### Stack Effect Calculation

For a 6m chimney with 250°C flue gas at 10°C ambient:

```
ΔP_stack = ρ_ambient × g × H × (1 - T_ambient/T_flue)
         = 1.20 × 9.81 × 6 × (1 - 283/523)
         ≈ 32 Pa natural draft
```

Friction losses in 150mm diameter, 6m chimney at 2 m/s: ~6–8 Pa. Net available: ~24 Pa.

Base injection adds to this if injected air mixes with flue gas. Preheated injection air (even slightly warm from proximity to flue pipe) increases buoyancy in the column. Combined effect: base injection is additive to natural draft, not competing with it.

---

## 2. Engineering Design

### Mounting Position

**Primary: Inside the room, horizontal flue section**
Most residential stoves have 0.5–2m of horizontal flue pipe accessible inside the room before the chimney. Temperatures here: 200–350°C. The blower connects inline in this section:

- Motor handles ambient air (fed from room or outside)
- Nozzle injects air into flue stream at throat
- Venturi effect entrains flue gas upstream
- Motor never contacts hot flue gases → no high-temp motor required

**Alternative: Chimney base exterior**
For properties with accessible external chimney base. Annular nozzle around exterior of flue pipe. Induces draft via temperature differential rather than direct injection.

### Motor Specification

Since the motor handles ambient air (not flue gas), standard IP54 industrial motors are sufficient:

| Parameter | Spec |
|-----------|------|
| Type | Centrifugal or mixed-flow |
| Power | 30–50W |
| Voltage | 230V AC with VFD, or 24V DC |
| Temp rating | 80°C (handles warm room air) |
| IP rating | IP54 |
| Supplier | ebm-papst RG series, Ziehl-Abegg RV series |
| Unit cost | €18–25 at 1k volume |

### Nozzle / Ejector Design

- Material: 316L stainless (contacts hot flue gas at injection point)
- Throat diameter: 20–30mm for target jet velocity
- Compression ratio: 1.05–1.15 (modest — this is assist, not primary drive)
- Collar connection: standard 150/175/200mm flue pipe clamp, tool-free

### Thermal Isolation

Motor at ambient temperature. Nozzle at flue temperature (200–350°C). Isolation:
- Ceramic fibre gasket between motor housing and nozzle collar: Unifrax Isofrax 1260°C rated
- Air gap maintained by stainless standoffs
- Motor housing exterior: max 60°C in normal operation

### Condensate Management

Base of chimney = cooler temperatures = more condensate. Acid condensate from wood combustion: pH 2.5–3.5 (mixture of sulfurous, carbonic, acetic acids).

- All flue-contact surfaces: 316L stainless minimum
- Condensate drain port at lowest point: 10mm diameter with check valve
- Motor housing: PA66-GF30 (acid resistant, no metal contact with condensate)

### Safety System

**Differential pressure sensor:** Sensirion SDP810 mounted at stove flue outlet. Continuously monitors gauge pressure.
- If pressure at appliance > −1 Pa (approaching positive): blower speed reduces
- If pressure at appliance > +0.5 Pa: blower stops immediately, alert sent to app
- System only runs in active assist mode when natural draft is confirmed insufficient

**CO sensor:** Optional add-on at blower intake. If CO detected in blower input air (indicating backdraft), system stops.

---

## 3. Bill of Materials

*1,000 unit volume pricing*

| # | Component | Specification | Supplier | Unit Cost (€) |
|---|-----------|---------------|----------|---------------|
| 1 | Centrifugal motor | ebm-papst RG130/1200, 30W, IP54 | ebm-papst | 22.00 |
| 2 | Nozzle/ejector collar | 316L stainless, 3 sizes (150/175/200mm) | Contract mfg | 12.00 |
| 3 | Thermal isolation kit | Ceramic fibre gasket + stainless standoffs | Unifrax/RS | 4.00 |
| 4 | VFD/speed controller | Simple PWM inverter for AC motor | Generic | 8.00 |
| 5 | MCU | ESP32-S3-WROOM-1 | Mouser | 3.20 |
| 6 | Pressure sensor | Sensirion SDP810-500Pa | Sensirion | 6.50 |
| 7 | Temp sensor | K-type thermocouple + MAX31855K | Mouser | 4.00 |
| 8 | Motor driver | Simple relay + PWM for DC option | — | 2.00 |
| 9 | Housing | PA66-GF30, acid-resistant | Contract mfg | 10.00 |
| 10 | Condensate drain | Check valve + 10mm fitting | Festo | 2.50 |
| 11 | PCB (assembled) | 4-layer, 60×50mm | PCBWay | 10.00 |
| 12 | Misc (seals, fasteners, cable) | — | — | 3.50 |
| **TOTAL** | | | | **~87.70** |

**Target BOM:** €80–95 at 1k volume
**Retail price:** €149–189
**Power:** 230V Schuko plug — no electrician required
**Installation time:** 30 minutes, DIY

---

## 4. Where This Works Best

| Scenario | Suitability | Reason |
|----------|-------------|--------|
| Short chimneys (<6m) | ★★★★★ | Natural draft is weakest — ejector adds proportionally most |
| Accessible horizontal flue section | ★★★★★ | Ideal mounting position |
| New builds | ★★★★ | Can be designed in from start |
| Retrofit with horizontal section | ★★★★ | Most common residential setup |
| Tall chimneys (>8m) | ★★★ | Natural draft usually sufficient — less benefit |
| Straight vertical chimney (no horizontal section) | ★★ | Requires external base mount — more complex |
| Multi-story stove/fireplace | ★★ | Long flue = more natural draft, less need |

---

## 5. Why Exodraft Hasn't Done This Already

Honest assessment:
1. **Channel risk:** Installer channel is built around top-mounted fans. A DIY product undercuts installer revenue.
2. **Efficiency:** Ejector efficiency (10–30%) is genuinely lower than direct fan on flue stream.
3. **Safety perception:** "Pushing into a chimney" sounds dangerous vs. "pulling from chimney top" — even if engineering makes both equally safe.
4. **Condensate complexity:** Base position requires careful drainage design — additional engineering cost.

None of these are insurmountable. The commercial case is the DIY channel: 3x the addressable market if installation cost drops from €1,200 to €200.

---

## 6. Business Case

| Metric | Value |
|--------|-------|
| BOM at 1k units | €88 |
| Landed cost (+20%) | €106 |
| Retail price | €169 |
| Gross margin | 37% |
| Gross margin at 10k units | 48% |
| Break-even units (€150k NRE) | ~3,000 |
| Target segment | DIY residential, new builds |
| Addressable market unlock | +70% of drop-offs due to installation cost |

# Product C: Mount It Anywhere — 5 Radical Installation Alternatives

**The assumption to break:** The fan must go at the chimney top.
**The question:** What if we designed for the installation, not the physics textbook?

---

## Overview

All five positions are technically feasible. They differ in: temperature requirements, maintenance access, efficiency, retrofit vs. new-build suitability, and cost.

**Summary table:**

| Position | Location | Temp | DIY? | Efficiency vs top-mount | Best for |
|----------|----------|------|------|------------------------|----------|
| 1: Inline at 2m | Inside chimney shaft | 150–250°C | No | 85–90% | New builds |
| 2: Horizontal section | Inside the room | 200–350°C | Yes | 90–95% | Most retrofits |
| 3: Firebox air blower | At stove intake | Ambient | Yes | Different mechanism | Combustion optimization |
| 4: Exterior base inducer | Outside chimney base | Ambient | Yes | 70–80% | Rural/farmhouse |
| 5: Drone-deployable | Chimney top | Same as top-mount | Service contract | 100% | Future, premium |

---

## Position 1: Inline Inside the Chimney (2m Height)

### Concept
Mount the fan inside the chimney shaft at ~2m height — accessible from a ladder or inspection hatch, but below the roofline. Functions like an inline duct fan for HVAC, but designed for chimney temperatures.

### Why 2m?
- Above the first joint (less turbulent flow than right at the stove connection)
- Below the roofline (accessible from inside the house with a long pole or from a ladder at the chimney breast)
- Temperature at 2m: typically 150–250°C — manageable with correct motor selection

### Technical Specification

**Motor requirements at 150–250°C:**
Standard HVAC inline fans: rated to 60–80°C. Not suitable.
High-temperature motors required:

| Motor option | Max temp | Cost | Supplier |
|-------------|---------|------|---------|
| ebm-papst K3G200-RD05-03 | 180°C | €85 | ebm-papst |
| Ziehl-Abegg RH50M-ZIK.6F.2R | 200°C | €95 | Ziehl-Abegg |
| Custom wound high-temp BLDC | 250°C | €120 | Specialist |

**Housing:** 316L stainless steel (acid condensate + high temp)
**Impeller:** High-temp glass-reinforced nylon or stainless steel
**Shaft seals:** Graphite or PTFE — no rubber

### Maintenance Access

This is the biggest challenge. Two design approaches:

**Option A: Removable section**
Design a 300mm chimney section that incorporates the fan. Annual maintenance: remove the section, service the fan, reinstall. Requires tool-free quick-release collar (Victaulic-style coupling).

**Option B: Inspection hatch**
Install inspection hatch below fan level. Sweep inserts rod tool to check and clean impeller. Full removal only every 3–5 years.

**Commercial precedent:** Exodraft's own product line includes inline fans for commercial kitchen exhaust. The architecture is proven — it's a scale/cost/material challenge for residential.

### Feasibility: ★★★★☆
High for new builds (design it in from the start). Medium for retrofit (requires access planning). Not suitable for sealed chimney systems without inspection hatch.

---

## Position 2: Horizontal Flue Section (Inside the Room)

### Concept
This is the highest-feasibility position. Most residential stoves have 0.5–2m of horizontal flue pipe inside the room connecting the stove to the chimney. Mount the fan inline in this section.

**Key advantages:**
- Completely accessible at all times — no roof, no ladder
- Motor can be located OUTSIDE the hot flue path with a simple heat shield
- Annual sweep maintenance trivial — same access as stove
- Power: normal wall outlet, 1m from the installation point

### Why This Position Works

Temperature in horizontal section: 200–350°C (hot, but manageable with correct materials).

**The motor-outside design:**
The fan impeller is in the hot flue stream (316L stainless). The motor shaft passes through a ceramic/PTFE bearing seal to a motor housing on the OUTSIDE of the flue pipe. Motor temperature: 60–80°C with heat shield → standard IP54 motor works.

This is exactly how Exodraft's RSHT product (pizza oven, 700°C rated) works. Scale it down for residential.

### Technical Specification

| Component | Specification |
|-----------|---------------|
| Impeller | 316L stainless, 120mm diameter |
| Impeller housing | 316L stainless, standard flue diameter |
| Shaft seal | PTFE + ceramic bearing |
| Motor | ebm-papst standard 25W IP54 (outside flue) |
| Heat shield | Mineral wool insulation wrap, 50mm |
| Connection | Standard 150/175/200mm flue clamp collar |
| Power | 230V Schuko, normal plug |
| App/control | ESP32-S3 + Xzense integration |

**BOM:** ~€75–95 at 1k volume
**Retail:** €149–199
**Installation:** DIY, 45 minutes. Remove horizontal section, install fan section, reconnect.

### Feasibility: ★★★★★
This is arguably the best product in this document for residential retrofit. The engineering is straightforward (Exodraft already does this for commercial), the installation is accessible, and it eliminates both the roofer and the electrician.

---

## Position 3: Combustion Air Blower at Firebox

### Concept
Rather than pulling flue gas out (top-mounted fan) or pushing it up (base blower), this product forces oxygen-rich air INTO the combustion zone via the stove's air intake — improving combustion quality and fire temperature, which in turn increases natural draft.

This is fundamentally what bellows do. A small, controlled, thermostat-driven electric blower replaces the passive air intake.

### How It Works

The stove's air intake vent (typically a sliding plate or rotary valve) is replaced with a small centrifugal blower:
- At low settings: blower runs at minimum speed, fan provides same airflow as open vent
- At lighting: blower at high speed → rapid heat buildup → faster establishment of natural draft
- At full burn: blower at low speed maintains optimal combustion airflow
- Auto control: temperature sensor at flue pipe outlet drives blower speed

### Safety Design

Positive pressure into the combustion zone creates risk if the fan fails or operates in reverse.

Mitigations:
1. **One-way damper:** Spring-loaded damper at blower outlet closes if power fails — prevents smoke backdraft through blower
2. **CO sensor** at blower intake: if CO detected (indicating backdraft), blower stops immediately
3. **Pressure interlock:** Blower only runs when flue temperature confirms fire is established

**This is not unprecedented.** Pellet stoves and modern wood gasification boilers use exactly this architecture — forced combustion air with automated control. The difference is applying it to traditional wood stoves as an aftermarket retrofit.

### Limitations

- Stove-specific fitting: different stoves have different air intake geometries. Requires a library of adapters.
- Not suitable for open fireplaces (no air intake duct)
- User must accept that their manual air control is replaced by automatic control — some users resist this

### Feasibility: ★★★☆☆
High engineering potential, but stove-specific fitting creates channel complexity. Best as an OEM partnership with stove manufacturers (Morsø, Rais, Jøtul) who can integrate it at the factory.

---

## Position 4: Cold Air Induction at Chimney Base (Exterior)

### Concept
For properties where the chimney base is accessible from outside (farmhouses, detached chimneys, rural properties), mount an inducer fan at the exterior chimney base that draws cold ambient air *around* the flue pipe.

The temperature differential between the cold external air moving upward (guided by a shroud) and the hot flue gases inside creates additional buoyancy — the chimney draft is enhanced without putting the fan in the flue gas stream at all.

**This is the principle of induced-draft cooling towers and power plant chimney stacks.** The "induced draft" fan is at the bottom pulling cool air upward. The temperature differential does the rest.

### Technical Specification

| Component | Specification |
|-----------|---------------|
| Fan type | Axial, 300mm diameter |
| Motor | Standard IP55 outdoor rated, 30W |
| Shroud | HDPE or GRP, weather-resistant |
| Mounting | Concrete anchor base or chimney clamp |
| Control | Temperature-differential sensor (inside vs. outside chimney) |
| Power | 230V, standard outdoor cable |

**Key advantage:** Motor is at ambient temperature. No high-temp materials required. Standard outdoor-rated motor at €15–20. Impeller is moving clean air, not corrosive flue gas.

**BOM:** ~€55–70 at 1k volume
**Retail:** €119–149

### Feasibility: ★★★☆☆
Niche market — requires accessible exterior chimney base. Very effective where applicable. Rural Denmark, UK farmhouses, Swedish summer houses.

---

## Position 5: Drone-Deployable Chimney Top Unit

### Concept
Design the fan unit specifically for installation by drone — no human on the roof at any point. The chimney pot has a pre-installed magnetic landing pad (part of the product). The fan unit is dropped onto it by a commercial drone, magnetic lock engages, and installation is complete.

**This eliminates the last remaining barrier for top-mounted fans.**

### Why Now?

Current commercial drone precision landing: ±10cm (DJI Agras, Zipline delivery drones). A 150–250mm chimney pot is 150–250mm in diameter. Landing a 2kg payload with ±10cm precision onto a 200mm target: achievable today.

The barrier is not technical. It is:
1. **Regulatory:** EASA requires operational authorization for commercial drone operations >250g near people/buildings
2. **Commercial model:** Not a DIY product — requires certified drone service operator
3. **Insurance:** Drone installation liability framework is still developing

### Product Specification

| Requirement | Target | Notes |
|-------------|--------|-------|
| Weight | <2.0kg | DJI commercial payload limit |
| Landing pad diameter | 200–300mm | Pre-installed by installer (once) |
| Attachment | N52 neodymium magnets, 12kg pull | Sufficient for 100km/h wind loads |
| Power | Solar + LiFePO4 | No cable required |
| Connectivity | WiFi + BLE | |
| Annual service | Drone swap | Fan unit retrieved and replaced annually |

### Commercial Model

1. Homeowner orders Exodraft Drone
2. Installer visits once to fit landing pad on chimney pot (normal ladder access, 15 minutes)
3. Annual installation/service: certified drone operator flies to property, swaps fan unit
4. Service cost: €79/year (replaces annual sweep-based fan check)

### Timeline: 3–5 Years

EASA is actively expanding autonomous drone operation authorizations. The regulatory path is clear — it just takes time. Exodraft should be designing the product now so it's ready when regulations allow.

### Feasibility: ★★☆☆☆ (today) → ★★★★☆ (2029)

---

## Recommended Priority

1. **Position 2 (horizontal section)** — build this first. Highest feasibility, largest retrofit market, builds on Exodraft's existing RSHT commercial product.
2. **Position 3 (firebox blower)** — pursue as OEM partnership, not standalone product.
3. **Position 1 (inline at 2m)** — new build specification play. Partner with chimney system manufacturers.
4. **Position 4 (exterior inducer)** — niche accessory product, minimal investment.
5. **Position 5 (drone)** — invest in design now, plan for 2029 commercial launch.

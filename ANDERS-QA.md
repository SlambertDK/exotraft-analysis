# ANDERS-QA.md — 25 Hard Technical Questions & Honest Answers

**For:** Exodraft R&D meeting with Anders
**Prepared by:** Henrik Lambert, March 2026
**Updated:** With Teknologisk Institut customer research (July–August 2024)

This document prepares for the hardest questions an R&D engineer will ask.
No spin. No marketing language. Honest engineering answers.

---

## PRODUCT A — The Blower Concept (Push from Below)

### Q1: An ejector-style blower from below — doesn't that completely violate the physics of natural convection?

Not if sized correctly. Natural draft creates a low-pressure zone at the top. A blower at the base can amplify rather than oppose this — if the ejector nozzle velocity matches the chimney's natural draft velocity. The critical design constraint is pressure matching: blow too hard and you backpressure the stove; too soft and you add nothing. This is solvable with VFD speed control and a differential pressure sensor. The honest answer: it's a harder tuning problem than a top-mounted fan, but not physically impossible. It needs empirical testing in a range of chimney geometries.

### Q2: How do you prevent backdraft when the blower shuts off or loses power?

Passive backdraft damper at the base — spring-loaded, normally closed. Opens only when positive pressure is present. This is standard in forced-air systems. On power loss, the damper closes within 200–400ms. Top-mounted fans have the same power-loss problem; they just let the natural draft decay gradually rather than stopping abruptly. The damper adds ~€8 BOM cost.

### Q3: The ejector nozzle will accumulate creosote. What's the service interval?

Honest answer: unknown without field data. Estimate is 12–18 months based on analogous industrial applications. The nozzle must be accessible for cleaning — this is a design constraint, not an afterthought. This connects directly to the TI research finding: "easy to service" is a Tier 2 priority (7.6/10), and "professionals like chimney sweeps need it to be straightforward." A blower nozzle that requires chimney disassembly to clean would fail this requirement immediately.

---

## PRODUCT B — Thermosyphon / TEG Concept

### Q4: What is the realistic power output from a thermoelectric generator on a residential chimney?

Honest numbers: residential flue gas at operating temperature is 200–350°C. Ambient air is 5–20°C outdoors. ΔT across the TEG module is roughly 80–150°C after thermal resistance losses. Commercial TEG modules (e.g., Hi-Z HZ-20) produce ~20W/module at ΔT=200°C. At ΔT=100°C (realistic), output drops to ~5–8W per module. A 4-module array produces 20–32W. A typical EC chimney fan draws 15–40W depending on speed. Conclusion: TEG alone can run a low-speed fan in steady-state but cannot handle startup loads or high-draft demand. It needs either a supercapacitor buffer for startup or a hybrid solar supplement.

### Q5: Isn't this exactly what the market has already rejected? TEG chimney fans have been tried before.

Yes. Thermofire, Warpfive, and others attempted this and failed commercially. The honest reasons: TEG efficiency is 3–8% (Carnot limit, not engineering incompetence), module cost is high, and the ΔT available in residential chimneys is lower than industrial. What changes the equation: (1) EC fans now draw 60–70% less power than the AC motors those products used, (2) solar+TEG hybrid rather than TEG-only, (3) the off-grid use case is now a Tier 2 customer priority (7.3/10 per TI research). The concept is not "TEG fan" — it's "resilient off-grid chimney management." Different value proposition.

### Q6: Solar on a chimney stack — how do you handle shading, weatherproofing, and orientation?

Three real problems. Shading: chimney pots are often on north-facing slopes or shaded by neighbouring structures. Pre-installation assessment is required — this is a sales/survey process question, not purely engineering. Weatherproofing: IP67 panel + UV-stable laminate + 316 stainless mounting hardware. This is solved technology. Orientation: fixed panel on chimney stack will rarely be optimally oriented. Expected output is 60–70% of theoretical maximum in Nordic latitudes (55–57°N). At 10W panel, expect 4–7W average during daylight in winter — sufficient for standby and moderate-speed operation combined with a small LiFePO4 buffer.

---

## PRODUCT C — Alternative Mounting Positions

### Q7: A horizontal through-wall installation — doesn't that destroy the thermal stack that makes the chimney work?

The thermal stack (buoyancy-driven draft) depends on vertical height, not the orientation of the terminal fitting. A fan in a horizontal through-wall elbow at the top of the chimney run can produce equivalent pressure rise to a vertically-mounted unit. The engineering question is sealing and condensate management — horizontal runs collect condensate that vertical runs drain naturally. A condensate trap at the lowest point of the horizontal run is required. This is standard in direct-vent appliance installation.

### Q8: Interior installation in the living space — noise and aesthetics. Isn't this a non-starter?

The TI research ranked noise at 9.6/10 — it's the single most important product requirement. An interior-mounted fan would need to be below 30 dB(A) at 1 meter to be acceptable in a living room. Current EC motors at low speed achieve 35–40 dB(A). Not there yet. However, the concept has a real advantage: the motor is accessible for service without roof access, which directly addresses the Tier 2 "easy to service" requirement. The engineering path: acoustic enclosure + vibration isolation + variable speed optimization. Possible at premium price point (€500+). Not a near-term product, but a valid R&D direction.

### Q9: What certifications would be required for a non-standard mounting position?

EN 13384 (chimney thermal and fluid dynamics) applies regardless of fan position. Non-standard installations require a CE declaration of conformity and likely a Notified Body assessment for the specific configuration. In Germany, chimney installations require Schornsteinfeger (master chimney sweep) approval — non-standard configurations face a higher barrier here. This is a market-access problem as much as an engineering problem.

---

## PRODUCT D — ExoSense Smart Sensor Collar

### Q10: A retrofit sensor on a competitor's fan — won't that void the competitor's warranty and create liability?

The collar attaches externally — no modification to the existing fan. Similar to how a Nest thermostat replaces an existing thermostat without voiding the HVAC system warranty. The sensor measures draft pressure, flue temperature, and RPM (via hall effect sensor reading magnetic field pulses) without physical connection to the fan. Liability is limited to the sensor's own performance, not the host fan. Legal review required for specific markets, but the principle is established in the smart home retrofit space.

### Q11: How do you measure RPM non-invasively on a competitor's fan?

Three methods: (1) Hall effect sensor detecting blade-pass magnetic signature through the housing — works on most AC/EC motors, requires calibration per fan model. (2) Optical interrupt sensor in the airflow — measures blade frequency from airflow pulses. (3) Audio spectral analysis via MEMS microphone — blade pass frequency is identifiable in the acoustic spectrum. Method 1 is most reliable. Accuracy: ±2% after per-model calibration. A library of competitor fan signatures is a real engineering investment — but also a moat.

### Q12: The €79 price point — is it realistic with the sensor suite you're describing?

BOM estimate:
- ESP32-C3 module: €2.80
- MEMS pressure sensor (differential, ±500Pa): €3.50
- NTC temperature probe: €0.80
- Hall effect sensor: €0.40
- LiFePO4 cell (1000mAh): €4.20
- PCB + passives: €3.00
- Enclosure (injection moulded): €6.00
- Assembly + test: €4.00
- **Total BOM: ~€24.70**

At €79 retail with 2.5x wholesale multiplier → wholesale ~€31.60 → gross margin ~22% at launch. Thin but viable. Improves significantly at volume. The subscription revenue (€5.49/month) is the real business case — hardware is customer acquisition.

### Q13: What happens to the service model when Exodraft owns sensor data from competitor fans?

This is the interesting strategic question. Exodraft gains visibility into: where competitor fans are installed, how they perform, when they fail. This is competitive intelligence as a side effect of a customer service. The data also enables a targeted replacement offer: "Your [Competitor X] fan has shown degraded performance for 3 weeks. Here is a trade-in offer for an Exodraft unit." Conversion economics on this would be exceptional — highly qualified, already engaged customer.

---

## PLATFORM & ARCHITECTURE

### Q14: How does the connected platform handle GDPR for installation location data?

Location data is sensitive — a chimney installation uniquely identifies a home address. GDPR Article 6 requires a lawful basis. Options: (1) consent (clear opt-in during onboarding), (2) legitimate interests (performance optimisation, safety alerts). Consent is cleaner. Data minimisation principle: store grid-level location (100m resolution) for aggregate analytics, precise location only for direct service delivery, encrypted at rest, never sold to third parties. Privacy-as-club-value, as we've framed it for MaxBuddy — same principle applies here.

### Q15: What's the connectivity architecture? WiFi means the installer needs to configure it. That's friction.

Agreed — WiFi onboarding friction is the primary reason smart home products get 1-star reviews. Preferred architecture: WiFi with Bluetooth Low Energy provisioning (same as Matter standard). User opens phone, app detects device via BLE, configures WiFi credentials without typing. Fallback: cellular (NB-IoT or LTE-M) for installations without WiFi coverage (many rural properties). NB-IoT module adds €4–6 BOM cost, eliminates WiFi dependency entirely. Given that Exodraft's rural customer base is disproportionate, NB-IoT may be the right primary connectivity.

### Q16: What about firmware OTA updates for a safety-critical device?

Two-partition flash (A/B update scheme). New firmware downloads to inactive partition, verified with SHA-256 hash + manufacturer signature, activated on next safe restart. Rollback on boot failure. Safe restart = fan stopped, no active fire (inferred from flue temperature below threshold). This is established practice in IoT safety devices. Relevant standard: IEC 62443 for industrial IoT security — residential devices aren't formally required to comply but good practice.

---

## BUSINESS MODEL & STRATEGY

### Q17: Why would Exodraft share data with municipalities and insurers? That seems like a privacy and brand risk.

Only aggregate, anonymised data — never individual installation data. "Air quality index in postal code 5550 is currently 47 µg/m³ PM2.5, based on 340 connected chimneys" is valuable data. No individual address is exposed. The brand risk is actually inverse: Exodraft becomes the trusted source of residential air quality data, which elevates the brand beyond "fan manufacturer." The risk to manage is communication — customers need to understand and consent to aggregate data use.

### Q18: The €5.49/month subscription — what does it actually deliver that justifies ongoing payment?

The subscription must deliver tangible value every month or churn is high. Minimum viable subscription includes: (1) combustion efficiency score + improvement suggestions, (2) "your chimney has X weeks before recommended service" predictive maintenance alert, (3) air quality report for your area during heating season, (4) fuel cost savings estimate vs. unoptimised combustion. Nice to have: community benchmarking ("your chimney efficiency is in the top 23% for your region"), integration with smart home systems, winter readiness check. Price-anchored against: Netflix €12.99, Spotify €10.99. At €5.49 it needs to feel like a useful utility, not a luxury.

### Q19: How do you handle the dealer channel? Dealers have relationships. An app could cut them out.

Wrong frame. Dealers are Exodraft's distribution moat — not a legacy cost. The connected platform should give dealers superpowers: (1) remote monitoring of all their installed base from one dashboard, (2) predictive service alerts so they can proactively schedule maintenance visits, (3) faster diagnosis of customer complaints without truck rolls. The dealer who adopts the platform retains customers better and earns more per customer lifetime. The dealer who doesn't gets disintermediated by a dealer who does. Exodraft's role: make the platform dealer-first, not dealer-bypass.

### Q20: The German market is reportedly stagnant. Why spend AI resources there?

The TI research identified Germany as currently stagnant. The strategic response: focus near-term AI resources on Austria (government incentives, active market) and the UK (post-Brexit regulatory alignment creating opportunity). Germany remains important for distribution infrastructure but is not the primary AI-content marketing target in the 12-month horizon. An awareness agent publishing in German still serves Austrian customers.

---

## FUNDAMENTALS

### Q21: Exodraft already has the Xzense controller. Why is this not already smart?

Xzense is a control system, not a sensing and learning platform. It can receive commands and execute them. What it currently lacks: (1) on-board pressure and temperature sensing with sufficient resolution for combustion analysis, (2) cloud connectivity for data aggregation, (3) a software platform for building services on top of the hardware. The gap is not the motor or the fan — it's the sensor suite and the data infrastructure. This is not a criticism; it's the opportunity. The hardware exists. The intelligence layer needs to be added.

### Q22: What's the competitive moat? Amazon will launch a connected chimney fan for €49.

Three genuine moats: (1) Domain knowledge — 67 years of chimney engineering can't be replicated in a product development cycle. (2) Installer network — Exodraft's certified installers are a distribution and service channel Amazon cannot replicate. (3) Data flywheel — once 10,000+ units are connected, the performance dataset creates product advantages (better default tuning, better failure prediction) that a new entrant can't match. The Amazon €49 unit will exist. The question is whether it can earn the trust of a customer who heats their home with an open fire. Trust is the moat.

### Q23: Is the sensor collar (Product D) actually a Trojan Horse that cannibalises new fan sales?

Yes — in the short run, it delays the "replace my fan" decision for some customers. In the long run, the data from the collar shows customers exactly when their existing fan has degraded enough to warrant replacement, and Exodraft is perfectly positioned to make that offer. The cannibalisation concern assumes customers would have bought a new fan anyway. Most customers with a functioning competitor fan would not. The collar converts passive non-customers into active Exodraft subscribers — the net effect is positive.

### Q24: What AI model runs the combustion analysis? You can't run GPT-4 on an ESP32.

The ESP32 does not run the AI model. Architecture: ESP32 collects sensor data (pressure, temperature, RPM) at 1Hz, sends a compact telemetry packet to the cloud (Exodraft's backend) every 60 seconds. The analysis model runs server-side. The ESP32 only needs to execute simple rules: "if pressure drops below X for Y seconds, increase RPM by Z." The intelligence (learning what X, Y, Z should be for this specific chimney) lives in the cloud. This is the standard edge-cloud split for IoT analytics.

### Q25: Why should Exodraft work with an external advisor rather than hire internally?

Honest answer: an internal hire learns AI over 6–18 months while on Exodraft's payroll. An external advisor starts deploying working systems on day one and transfers knowledge as he goes. The 90-day sprint model is explicitly designed to build internal capability, not dependency. At the end of 90 days, the Exodraft team can continue independently with the tools and patterns established. The advisor relationship ends when it is no longer useful — there is no lock-in.

---

*Document prepared for Exodraft R&D meeting, March 2026.*
*All technical specifications are estimates pending engineering validation.*
*Full white papers with BOM estimates available in products/ folder.*

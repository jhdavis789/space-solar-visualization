# Key Findings: Space Solar Power in LEO

## Critical Finding: Eclipse-Free Altitude Threshold

**A dawn-dusk sun-synchronous orbit becomes completely eclipse-free above ~574 km altitude.**

This is perhaps the single most important design constraint for space solar power in LEO. Below this altitude, the satellite periodically passes through Earth's shadow, requiring battery storage and reducing average power output. Above it, the satellite receives continuous 24/7 sunlight year-round.

### The Physics

In a dawn-dusk SSO, the orbital plane is aligned with the solar terminator (the day-night boundary on Earth). The "beta angle" — the angle between the orbital plane and the Sun direction — stays near 90° year-round because SSO precession matches the Earth's orbit around the Sun.

However, Earth's axial tilt (obliquity ε = 23.44°) causes the beta angle to oscillate seasonally:

    β_min = 90° − ε = 66.56°

The satellite enters eclipse when Earth's angular radius (as seen from orbit) exceeds the beta angle:

    arcsin(R_E / r) > β_min

Solving for the critical altitude:

    r > R_E / sin(66.56°)
    r > 6371 km / 0.9171
    r > 6949 km
    altitude > 578 km (theoretical), ~574 km in practice with perturbations

### Eclipse Duration by Altitude

| Altitude | Max Eclipse | Eclipse Days/Year | Battery Needed (Wh/kW) |
|----------|-------------|-------------------|------------------------|
| 300 km   | 21.0 min    | 173               | 350                    |
| 400 km   | 16.3 min    | 131               | 271                    |
| 500 km   | 10.3 min    | 82                | 172                    |
| 550 km   | 5.8 min     | 46                | 96                     |
| 574 km   | 0           | 0                 | 0                      |
| 600+ km  | 0           | 0                 | 0                      |

### Why This Matters for Economics

This creates a natural minimum deployment altitude for solar power:

1. **Below ~574 km**: You need batteries (adding mass and cost), suffer reduced average power, AND pay the highest station-keeping costs due to atmospheric drag. Triple penalty.

2. **574–700 km**: The sweet spot. Eclipse-free, moderate drag (manageable with electric propulsion), reasonable debris environment, and natural orbital decay within decades (regulatory compliance).

3. **Above 800 km**: Eclipse-free with negligible drag, but peak debris density, centuries-long orbital lifetimes (regulatory problem), and proximity to the inner Van Allen belt (more radiation damage).

### Battery Impact at Lower Altitudes

If forced below 574 km, battery requirements are significant:

- At 400 km: ~271 Wh/kW of battery storage needed
- Li-ion batteries: ~250 Wh/kg specific energy
- So ~1.1 kg/kW of battery mass per kilowatt of capacity
- At $200/kg launch cost: additional ~$1.10/kW just for battery launch mass
- Plus the battery hardware cost (~$100–200/kWh at space-grade)
- Plus batteries degrade and need replacement every 5–10 years

This makes the economics of sub-574 km deployment substantially worse.

## Launch Energy vs Altitude

The energy required to reach orbit is dominated by achieving orbital velocity, not by climbing altitude:

| Altitude | Launch ΔV  | Energy (kWh/kg) | Notes |
|----------|-----------|-----------------|-------|
| 400 km   | 9.23 km/s | 12.9            | Minimum practical LEO |
| 600 km   | 9.12 km/s | 13.1            | Eclipse-free threshold |
| 800 km   | 9.02 km/s | 13.2            | Peak debris zone |
| 1200 km  | 8.82 km/s | 13.6            | Upper LEO limit |

The total energy increase from 400 km to 1200 km is only ~5%. This means choosing a higher orbit (to eliminate eclipses and reduce drag) costs very little in additional launch energy — while saving enormously on station-keeping propellant and battery mass.

**The ΔV actually decreases with altitude** because orbital velocity is lower at higher orbits. Total energy still increases because the gravitational potential energy gain outweighs the kinetic energy reduction.

## Implication for Deployment Strategy

The optimal altitude for space solar power sits in a narrow band around **600–700 km**:

- Eclipse-free (no batteries needed)
- Drag is manageable with Hall thrusters (~10–50 m/s/yr station-keeping ΔV)
- Debris density is lower than the 800–900 km peak
- Natural deorbit within 25–50 years (acceptable for compliance)
- Only ~1–2% more launch energy than the minimum practical orbit

This is not a coincidence — it's a fundamental consequence of Earth's geometry and the physics of dawn-dusk SSO orbits.

---

## External Validation: Google's Space AI Infrastructure Paper (Nov 2025)

Source: Agüera y Arcas, Beals, Biggs, Bloom, Fischbacher, Gromov, Köster, Pravahan, Manyika. "Towards a future space-based, highly scalable AI infrastructure system design." arXiv:2511.19468, Nov 2025.

### What They Propose

An 81-satellite cluster at **650 km dawn-dusk sun-synchronous LEO** with Google Trillium TPUs, solar arrays, and free-space optical inter-satellite links. Essentially the end-state we're modeling toward — they skip the solar-only phase and design the full compute system.

### Validates Our Framework

1. **Orbit choice: 650 km dawn-dusk SSO** — dead center of our 600–700 km optimal band. They don't even discuss eclipses, which makes sense: per our analysis, 650 km is well above the 574 km eclipse-free threshold. Our work provides the theoretical justification they don't include.

2. **Launch cost projections align.** Their Wright's Law analysis (20% learning rate across SpaceX history) projects ≤$200/kg by mid-2030s. Breakdown:
   - No reuse: ~$460/kg
   - 10× reuse: <$60/kg (with margins: <$250/kg to customer)
   - 100× reuse: <$15/kg
   - Fuel floor: $8/kg (our range: $10–30)
   - Requires ~1,800 Starship launches / ~370,000 tonnes cumulative mass

3. **Solar constant, overall thesis** — fully consistent with our parameters.

### Challenges to Our Model

1. **Panel efficiency: 22% vs our 30%.** They reference Starlink v2 mini (105 m², 22% efficiency, 28 kW). Real cost-optimized satellites use cheaper, lighter panels, not top-tier triple-junction GaAs. Our model's 30% may be optimistic for actual deployment economics.

2. **Whole-system mass is 8–10× panel-only mass.** Starlink v2 mini: 575 kg for 28 kW = **20.5 kg/kW**. Our model assumes ~2.45 kg/kW (1 kg/m² panels at 30%). A real satellite bus (avionics, propulsion, structure, thermal, compute payload) dominates the mass budget. This is the largest gap in our current economics — we model panel launch cost but not total system launch cost.

3. **Thermal management is unsolved — for both of us.** They call it "a critical optimization challenge" and provide zero numbers. No radiator sizing, no operating temperatures, no waste heat solution. In vacuum, radiation is the only cooling mechanism. For power-dense TPU clusters, this is the #1 engineering problem they haven't solved.

### Key New Data Points

**Radiation environment at 650 km (with 10mm Al shielding):**
- Total dose: ~150 rad(Si)/year
- 5-year mission total: ~750 rad(Si)
- Google Trillium TPUs survived 15 krad cumulative (20× the 5-year requirement) without permanent failure
- HBM memory is the weakest link: 1 uncorrectable ECC error per ~50 rad
- In orbit: ~1 uncorrectable memory error per 10 million inferences
- Silent data corruption: ~1 event per 10⁷ rad
- System-level crash (SEFI): ~1 per 5 krad per chip

**Implication:** Modern silicon (even non-radiation-hardened commercial TPUs) can survive 5+ years in LEO at 650 km with modest shielding. This is a concrete proof point for the data-center end-state.

**Economics crossover threshold:**
- At $200/kg launch cost, Starlink-class power costs ~$810/kW/year
- Terrestrial datacenter power costs $570–3,000/kW/year
- **At $200/kg, space-based power reaches approximate parity with terrestrial**
- This is the critical threshold number for the entire thesis

**Inter-satellite links:**
- Target: 10 Tbps aggregate per link (needed for distributed ML training)
- Demonstrated: 1.6 Tbps bidirectional on bench
- Architecture: 24-channel DWDM at 1.55 μm → 9.6 Tbps per single aperture
- Close proximity (100–200 m between satellites) makes the optics feasible

**Formation flying:**
- 81 satellites in 1 km radius cluster
- J2-perturbation drift correctable to <3 m/s/year per km of cluster radius
- ML-based flight control proposed (not demonstrated in orbit)
- Zero delta-v possible under perfect Keplerian motion; real corrections are modest

### Gaps in Their Paper That Our Work Fills

They do NOT analyze:
- Eclipse geometry or eclipse-free altitude threshold (our core finding)
- Station-keeping ΔV budgets (our drag model with NRLMSISE-00 densities)
- Debris risk by altitude (our NASA90 model + empirical profile)
- Atmospheric density variation with solar cycle
- Orbital lifetime / deorbit regulatory compliance
- Detailed altitude selection trade-offs

Our projects are complementary: we provide the orbital environment analysis they skip; they provide the compute hardware and networking analysis we haven't reached yet.

---

## Open Question: Terrestrial Mega-Solar + Datacenter vs Space

**Core idea:** What if instead of going to space, you find a state with optimal solar coverage (Nevada, Arizona, New Mexico), buy an enormous tract of land, blanket it with solar panels, and build a datacenter underneath the panels? Bypass environmental regulations by paying a high tax to the state as compensation.

### The arithmetic to explore

- **GPU cost trajectory:** If $/FLOP/W continues declining (Moore's Law for AI accelerators), cheap chips change the calculus. If GPUs are cheap enough, you can afford to run them only ~33% of the time (daytime, clear sky) and still come out ahead on capex vs space.
- **Utilization penalty:** Earth gets ~20% capacity factor (best desert) vs ~97%+ in dawn-dusk SSO. But if the chips cost 1/10th what they cost today in 5 years, the idle-time penalty shrinks relative to total system cost. Space has near-100% utilization but enormous delivery cost.
- **No launch cost:** $0/kg to deploy on the ground. Compare: even at $200/kg to LEO, a 20 kg/kW satellite system costs $4,000/kW just to get there. On the ground, the panels + structure + land might be $800–1,500/kW all-in.
- **Cooling advantage:** Ambient air cooling on Earth vs radiation-only cooling in vacuum. This is the unsolved problem in the Google paper. On Earth, you just run industrial HVAC.
- **Regulatory arbitrage:** Pay the state, say, $50M/year in lieu of environmental review for a 10,000-acre solar+datacenter campus. States like Nevada already offer tax incentives for datacenters. Is this cheaper than orbital regulatory compliance (spectrum, debris mitigation, deorbit plans)?
- **Grid independence:** Panels directly power the datacenter. No grid interconnection needed if you size batteries for overnight or just shut down at night. AI inference workloads (unlike training) can be bursty.

### Key variables that determine which wins

1. **GPU $/FLOP/W decline rate** — if it's fast enough, idle chips are cheap chips
2. **Launch cost trajectory** — $200/kg vs $50/kg vs $10/kg completely changes the answer
3. **Thermal dissipation cost in space** — if radiator mass dominates, space compute is heavier than the panels
4. **Land + permitting cost** — how much does it actually cost to cover 10,000 acres of Nevada desert?
5. **Battery vs no-battery** — does the datacenter just shut down at night, or do you need storage?

### Why this might NOT work

- Even the best desert only delivers ~20% capacity factor vs ~97% in space
- Capital tied up in idle hardware earns nothing — opportunity cost matters
- Scale limits: you need ENORMOUS land area to match space power density
- Transmission losses if the datacenter serves remote users
- Water scarcity in deserts (cooling towers need water, though dry cooling exists)

**This deserves a side-by-side model on the dashboard: $/FLOP delivered, space vs terrestrial mega-solar, as a function of GPU cost trajectory and launch cost trajectory.**

# Space Solar Energy Model — Research Questions

End state: solar-powered data centers in space. This phase models energy production only.

**Decided:** Sun-synchronous LEO (dawn-dusk orbit). Target: 1 GW capacity. Focus: next 10 years.

---

## 1. Orbital Geometry & Available Space

- **Orbit type:** Sun-synchronous LEO, dawn-dusk configuration for ~95-100% sunlight
- How large is the usable orbital shell at various altitudes (500-2,000 km)?
- How many km² of solar panels can physically fit at each altitude band? (spacing constraints, collision avoidance, ITU/coordination rules)
- How crowded is sun-sync LEO already? (Starlink, other mega-constellations)
- At what point does congestion at the optimal altitude force deployment to less optimal altitudes?
- How much total capacity (GW) can we fit before running out of "good" orbital slots?

## 2. Solar Panel Physics — Efficiency

- Solar irradiance in LEO: ~1,361 W/m² (AM0 spectrum, no atmospheric filtering)
- How much more efficient vs Earth? (quantify: no atmosphere, no clouds, no night, but different spectrum)
- Best commercially available panel efficiency today? (triple-junction GaAs: ~30-32%)
- Best lab-demonstrated efficiency? (perovskite-silicon tandem: ~33%+, multi-junction concentrators: ~47%)
- Theoretical maximum efficiency? (Shockley-Queisser limit for multi-junction)
- **Degradation:** Science-based model required
  - Radiation dose rate at various LEO altitudes (trapped protons, electrons, solar particle events)
  - Equivalent fluence → efficiency loss curves (from manufacturer/NASA data)
  - Thermal cycling effects (eclipse transitions cause rapid thermal swings)
  - UV degradation of encapsulants and coatings
  - Expected annual efficiency loss rate with confidence intervals

## 3. Solar Panel Physics — Weight & Form Factor

- How thin could solar panels possibly be? (crystalline vs thin-film vs perovskite)
- Mass per m² for:
  - Ultra-thin film only (no structure): what's the floor?
  - Thin film + minimal support (shape maintenance in zero-g)
  - Rigid panels (traditional space-grade)
- Watts per kg for each form factor
- m² per kg for each form factor
- **Rigidity trade-off:** What's the minimum structure needed?
  - Solar radiation pressure tries to push/deform panels
  - Thermal gradients cause warping
  - Attitude control torques
  - Does zero-g mean you need almost no structure, or does SRP make structure essential?
  - How much mass does frame/boom structure add per m²?
  - Can tensioned membrane designs work? (like solar sails)

## 4. Thermal Properties

- Energy balance: panels absorb X W/m², convert Y% to electricity, remaining becomes heat
- Heat dissipation in vacuum is radiation-only (Stefan-Boltzmann law)
- What equilibrium temperature do panels reach in full sun at LEO?
- Temperature coefficient of efficiency: how many %/°C do panels lose?
- Differential heating: sun side can be ~100-150°C, shadow side can be -150°C
  - How does this thermal gradient stress the panels?
  - Does this drive structural requirements?
- Can passive thermal design (coatings, radiators) keep temps in acceptable range?
- Or is active cooling needed? (adds mass, complexity, cost)

## 5. Launch Economics — Energy

- Delta-v budget for sun-sync LEO at various altitudes:
  - Surface → LEO insertion
  - LEO → sun-sync plane change (if needed)
  - Gravity losses, drag losses, steering losses
- Theoretical minimum energy (joules) per kg to each altitude
- How does this compare to chemical propellant energy density?

## 6. Launch Economics — Cost (Starship, Full Spectrum)

**Four cost tiers:**

| Tier | Description |
|------|-------------|
| Floor (marginal) | Fuel + consumables + ground ops labor only. Zero capex. |
| Optimistic | Amortized over high reuse (e.g., 1000 flights). Low refurb. |
| Realistic | Moderate reuse (100-300 flights), realistic refurb & overhead |
| Pessimistic | Current commercial pricing (Falcon 9 / Ariane 6 levels) |

- Starship payload to sun-sync LEO at each altitude? (varies with inclination and altitude)
- Fuel cost per launch (LOX + CH4)
- Consumable/wear parts per launch
- Ground operations cost per launch
- Amortized vehicle cost at various reuse rates
- Resulting $/kg at each tier and each altitude

## 7. Debris & Micrometeoroid Risk Model

**Full probabilistic, size-bucketed model:**

- Micrometeoroid flux at various LEO altitudes (NASA ORDEM or ESA MASTER model data)
  - Size distribution: sub-mm dust, mm-scale, cm-scale, 10cm+
  - Velocity distribution of impactors
  - Directionality (ram direction vs trailing)
- Probability of impact per m² per year at each size class
- Damage per impact at each size class:
  - Sub-mm: surface pitting, gradual efficiency loss
  - mm-scale: panel penetration, localized damage
  - cm-scale: structural failure of a panel segment
  - 10cm+: catastrophic, potential debris generation
- **Scaling with area:** How does total expected damage grow as panel area grows?
  - Linear scaling for small areas
  - Do very large arrays increase local debris density? (self-generated debris from impacts)
- **Debris flux evolution over time:**
  - Current LEO debris growth rate
  - Projected growth with increasing launches
  - Kessler syndrome risk at relevant altitudes
  - How does this affect the 10-year economics?
- **Modularity / damage containment (design variable):**
  - Segment size as a parameter (e.g., 1m² to 100m²)
  - Mass cost of segmentation (frames, electrical isolation, connections)
  - Damage propagation model: does an impact on one segment affect neighbors?
  - Optimal segment size: minimize total cost (segmentation mass + expected damage loss)
  - Can damaged segments be individually replaced? At what cost?

## 8. Station-Keeping & Orbital Mechanics

- **Atmospheric drag** (dominant perturbation in LEO):
  - Atmospheric density vs altitude (exponential decay, but variable with solar cycle)
  - Drag force = ½ρv²C_d·A — directly proportional to panel area
  - For X m² of panels, how much ΔV/year for station-keeping?
  - Propellant mass per year (chemical vs electric propulsion)
  - At what panel area does drag become prohibitive at each altitude?
  - Solar cycle effects: density at 600 km can vary 10-100x between solar min and max
- **Solar radiation pressure:**
  - ~4.6 μN/m² — small per m² but adds up for large arrays
  - Creates torques if center of pressure ≠ center of mass
  - Contributes to orbit perturbation
  - At what array size does SRP become a significant station-keeping driver?
- **Propulsion options:**
  - Chemical: simple but heavy propellant
  - Electric (ion/Hall): high Isp, much less propellant, but low thrust, needs power
  - Could the solar panels power their own electric propulsion?
- **The key constraint:** is there an altitude where drag is low enough that station-keeping costs don't eat the economics, but launch costs are still reasonable?

---

## Model Outputs

**Primary:** $/W deployed in orbit (fully burdened: launch + panels + structure + station-keeping NPV)

**Secondary:**
- Total available capacity at optimal altitudes (how many GW can fit?)
- Annual operating cost (station-keeping + degradation replacement + debris damage)
- Effective $/W over asset lifetime (accounting for degradation and damage)
- Sensitivity analysis: which parameters move $/W the most?

**Scale target:** What does it take to get 1 GW into space?

---

## 9. Compute Hardware in LEO (from Google paper, arXiv:2511.19468)

Now that Google has published radiation test data for Trillium TPUs at 650 km SSO:

- **Radiation budget:** At 150 rad(Si)/year (10mm Al shielding), how does this map to our altitude bands?
  - What is the dose rate at 600 km vs 700 km vs 800 km?
  - How does shielding mass trade against altitude choice?
  - Inner Van Allen belt proximity: where does dose spike?
- **Compute density:** How many PFLOPS per kg can current TPUs/GPUs achieve?
  - At what $/kg launch cost does space compute become cheaper than terrestrial per FLOP?
  - How does compute-per-kg improvement rate compare to launch cost decline rate?
- **Thermal limits on compute density:**
  - TPUs dissipate ~200-400W per chip. In vacuum, only radiative cooling works
  - Stefan-Boltzmann: P = εσT⁴A — what radiator area per chip at 80°C? At 100°C?
  - Does radiator mass dominate the mass budget for compute payloads?
  - Google acknowledges this as unsolved. Can we model the thermal constraint?
- **Memory as the weak link:**
  - HBM: 1 uncorrectable error per 50 rad → ~3 per year at 650 km
  - Is ECC sufficient? What about silent data corruption during training?
  - Does this favor inference-only workloads in space?

## 10. Whole-System Mass Budget

Our current model uses panel-only mass (1 kg/m²). Real satellites are much heavier:

- Starlink v2 mini: 575 kg / 28 kW = 20.5 kg/kW (total system)
- What fraction is panels vs bus vs propulsion vs thermal vs payload?
- For a power-only satellite (no compute payload), what's the minimum $/kW?
- Should the dashboard have a "system overhead multiplier" on top of panel mass?

## 11. Launch Cost Learning Curves

Google uses Wright's Law (20% learning rate) to project $200/kg by mid-2030s:

- Is 20% the right learning rate? Historical SpaceX data: Falcon 1 ($30K/kg) → Falcon Heavy ($1.8K/kg)
- What cumulative mass is needed? They say ~370,000 tonnes (~1,800 Starship launches)
- How sensitive is the crossover date to learning rate? (18% vs 24%)
- Should we add a Wright's Law projection to the dashboard alongside our static tiers?

## 12. Formation Flying vs Monolithic Arrays

Google proposes 81-satellite clusters (1 km radius, 100-200m spacing):

- What are the advantages over monolithic large arrays?
  - Modularity, redundancy, incremental deployment
  - Each satellite is Starship-compatible mass
- What are the disadvantages?
  - Inter-satellite link overhead, formation-keeping fuel
  - Mutual thermal occlusion (IR radiation blocking between satellites)
  - Complexity of coordination
- How does the cluster architecture change our $/W calculation?

---

## Requirements

- Every assumption must cite its source (paper, dataset, manufacturer spec)
- Model extreme-optimistic, realistic, and pessimistic values for each parameter
- Focus on next 10 years, but asset lifetime derived from physics (degradation, damage)
- Functional dashboard UI (sliders, charts, tables — not polished, just usable)
- Single HTML file deployment (client-side computation)

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

## Requirements

- Every assumption must cite its source (paper, dataset, manufacturer spec)
- Model extreme-optimistic, realistic, and pessimistic values for each parameter
- Focus on next 10 years, but asset lifetime derived from physics (degradation, damage)
- Functional dashboard UI (sliders, charts, tables — not polished, just usable)
- Single HTML file deployment (client-side computation)

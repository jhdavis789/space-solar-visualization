# Space Solar Power — Equations, Constants & Data

All equations and constants used across the project files:
- `Altitude vs Sun.html` — 3D orbital visualization
- `altitude-selection.html` — altitude selection dashboard (sunshine, debris, drag)
- `dashboard.html` — economics dashboard ($/W)

Every assumption is documented with its source or derivation.

---

## 1. Physical Constants

| Constant | Symbol | Value | Unit | Source |
|----------|--------|-------|------|--------|
| Earth radius | R_E | 6,371 | km | WGS-84 mean |
| Gravitational parameter | μ | 398,600.4 | km³/s² | GM_Earth |
| J2 perturbation | J₂ | 1.08263 × 10⁻³ | — | EGM96 |
| Solar constant (AM0) | S₀ | 1,361 | W/m² | SORCE/TIM, Kopp & Lean 2011 |
| Sun radius | R_☉ | 696,340 | km | IAU 2015 |
| Astronomical Unit | AU | 149,597,870.7 | km | IAU 2012 |
| Earth obliquity | ε | 23.44 | ° | J2000 epoch |
| Atmospheric scale height | H | 8.5 | km | US Standard Atmosphere |
| Stefan-Boltzmann constant | σ | 5.670 × 10⁻⁸ | W/m²·K⁴ | CODATA 2018 |
| Speed of light | c | 299,792,458 | m/s | exact |
| Solar radiation pressure | P_SRP | ~4.6 | μN/m² | S₀/c at 1 AU |

---

## 2. Orbital Mechanics

### 2.1 Orbital Period

```
T = 2π √(r³ / μ)

where r = R_E + altitude [km]
```

### 2.2 Orbital Velocity

```
v = √(μ / r)
```

### 2.3 Sun-Synchronous Inclination

For the orbit's nodal precession rate to match Earth's revolution around the Sun (~0.9856°/day):

```
cos(i) = -Ω̇_target / (1.5 × J₂ × n × (R_E / r)²)

where:
  Ω̇_target = 1.991 × 10⁻⁷ rad/s  (= 360°/365.25 days)
  n = √(μ / r³)  [mean motion, rad/s]
```

Dawn-dusk configuration: RAAN = 90° (LTAN 18:00).

### 2.4 Launch Delta-V Budget

| Altitude | Total ΔV | Energy (kWh/kg) | Notes |
|----------|----------|-----------------|-------|
| 400 km | 9.23 km/s | 12.9 | Minimum practical LEO |
| 600 km | 9.12 km/s | 13.1 | Eclipse-free threshold |
| 800 km | 9.02 km/s | 13.2 | Peak debris zone |
| 1,200 km | 8.82 km/s | 13.6 | Upper LEO limit |

**Key insight:** Only ~5% energy increase from 400 km to 1,200 km. Altitude choice is nearly free in launch energy terms. ΔV *decreases* with altitude (lower orbital velocity), but total energy increases due to gravitational potential.

### 2.5 Station-Keeping (Atmospheric Drag)

```
F_drag = ½ ρ v² C_d A

where:
  ρ = atmospheric density [kg/m³] (exponential decay with altitude)
  v = orbital velocity [m/s]
  C_d ≈ 2.2 (flat plate, typical for solar arrays)
  A = cross-sectional area [m²]

ΔV/year = (F_drag / m) × seconds_per_year
        = ½ρv²C_d × (A/m) × 31,557,600
```

### 2.6 Atmospheric Density (NRLMSISE-00)

| Altitude (km) | Solar Min (kg/m³) | Solar Max (kg/m³) | Ratio |
|----------------|-------------------|-------------------|-------|
| 200 | 2.5×10⁻¹⁰ | 4.0×10⁻¹⁰ | 1.6× |
| 300 | 2.0×10⁻¹¹ | 6.0×10⁻¹¹ | 3× |
| 400 | 2.0×10⁻¹² | 1.0×10⁻¹¹ | 5× |
| 500 | 2.5×10⁻¹³ | 2.5×10⁻¹² | 10× |
| 600 | 4.0×10⁻¹⁴ | 7.0×10⁻¹³ | 18× |
| 700 | 6.0×10⁻¹⁵ | 1.8×10⁻¹³ | 30× |
| 800 | 1.0×10⁻¹⁵ | 5.0×10⁻¹⁴ | 50× |
| 1000 | 5.0×10⁻¹⁷ | 5.0×10⁻¹⁵ | 100× |

Source: NRLMSISE-00 model. Solar min ≈ F10.7 = 70, solar max ≈ F10.7 = 140.

Key: density varies **10–100× between solar min and max** at 500–1000 km due to thermospheric heating. This is the dominant source of station-keeping uncertainty.

**ΔV budget for station-keeping** (10,000 m² array at 1 kg/m²):
- 400 km: ~1,000–10,000 m/s/year (impossible)
- 500 km: ~50–500 m/s/year (marginal at solar min only)
- 600 km: ~5–100 m/s/year (manageable with Hall thrusters)
- 700 km: ~0.5–25 m/s/year (easy)
- 800+: < 1 m/s/year (negligible)

Hall thruster practical limit: ~300 m/s/year with reasonable propellant budget.

---

## 3. Solar Geometry & Eclipse Model

### 3.1 Sun Declination (Day of Year)

```
δ_☉ = 23.5° × sin(2π(DoY − 80) / 365.25)
```

### 3.2 Beta Angle (Sun Elevation Above Orbital Plane)

**Sun-synchronous orbit (dawn-dusk):**
```
sin(β) = sin(i + δ_☉)
```

**General orbit (e.g. ISS):**
```
sin(β) = sin(i)·cos(δ)·sin(Ω − α_☉) + cos(i)·sin(δ)

where:
  i = inclination
  Ω = RAAN (right ascension of ascending node)
  α_☉ = Sun's right ascension
  δ = Sun's declination
```

### 3.3 Shadow Half-Angle (Earth's Angular Radius)

```
ρ = arcsin(R_E / (R_E + alt))
```

### 3.4 Sunlit Fraction (Per Orbit)

```
If |β| ≥ ρ:  sunlit = 1.0  (continuous sunlight, no eclipse)
If |β| < ρ:  sunlit = 1 − arccos(cos(ρ) / cos(β)) / π
```

### 3.5 Eclipse-Free Altitude Threshold

**The critical finding: dawn-dusk SSO becomes completely eclipse-free above ~574 km.**

Derivation:
```
Eclipse occurs when: arcsin(R_E / r) > β_min
Minimum beta angle: β_min = 90° − ε = 90° − 23.44° = 66.56°
Solving: r > R_E / sin(66.56°) = 6,371 / 0.9171 = 6,949 km
Altitude > 6,949 − 6,371 = 578 km (theoretical)
~574 km in practice with perturbations
```

### 3.6 Eclipse Duration by Altitude

| Altitude | Max Eclipse | Eclipse Days/Year | Battery Needed (Wh/kW) |
|----------|-------------|-------------------|------------------------|
| 300 km | 21.0 min | 173 | 350 |
| 400 km | 16.3 min | 131 | 271 |
| 500 km | 10.3 min | 82 | 172 |
| 550 km | 5.8 min | 46 | 96 |
| 574 km | 0 | 0 | 0 |
| 600+ km | 0 | 0 | 0 |

---

## 4. Atmospheric Limb Attenuation Model

When a satellite is near the terminator, sunlight passes through the atmosphere at a grazing angle (limb path). This attenuates the received flux.

### 4.1 Tangent Height

```
d_⊥ = |r⃗_sat × ŝ_sun|  (perpendicular distance from Earth center to sun ray)
h_t = d_⊥ − R_E  [km above surface]
```

If h_t < 0: satellite is in geometric shadow.
If h_t > ~150 km: negligible atmospheric attenuation.

### 4.2 Chapman Function (Limb Air Mass)

For an exponential atmosphere viewed along a tangent ray:

```
AM_limb(h_t) = exp(−h_t / H) × √(2π(R_E + h_t) / H)

where H = 8.5 km (scale height)
```

### 4.3 Broadband Transmittance (Meinel Model)

```
T = 0.7^(AM^0.678)

where 0.678 accounts for wavelength-dependent Rayleigh + Mie scattering
```

### 4.4 Incident Flux at Satellite

```
Sun-side (no limb):     F = 1,361 W/m²
Penumbra (limb path):   F = 1,361 × T(h_t) W/m²
Geometric shadow:        F = 0
```

---

## 5. Ground Solar Irradiance (Comparison Baseline)

Calculated for Los Angeles (34°N) as reference.

### 5.1 Solar Elevation Angle

```
sin(α) = sin(lat)·sin(δ) + cos(lat)·cos(δ)·cos(HA)

where HA = (hour − 12) × 15°  [hour angle in degrees]
```

### 5.2 Air Mass (Kasten-Young 1989)

```
AM = 1 / (sin(α) + 0.50572 × (α_deg + 6.08)^−1.6364)
```

### 5.3 Ground Clear-Sky Flux

```
If α > 0:  F_ground = 1,361 × sin(α) × 0.7^(AM^0.678)
If α ≤ 0:  F_ground = 0  (nighttime)
```

**Note:** This is clear-sky only. Real ground solar includes cloud cover losses (~20–40% depending on location), which are NOT modeled here.

---

## 6. Solar Panel Physics

### 6.1 Efficiency Tiers

| Technology | Efficiency | Status | Source |
|------------|-----------|--------|--------|
| Triple-junction GaAs | 30–32% | Commercial space-grade | Spectrolab, Azur Space |
| Perovskite-silicon tandem | ~33%+ | Lab demonstrated | Oxford PV, HZB |
| Multi-junction concentrator | ~47% | Lab record | Fraunhofer ISE, NREL |
| Shockley-Queisser (single) | ~33.7% | Theoretical limit | Shockley & Queisser 1961 |
| Multi-junction (∞ junctions) | ~68% | Thermodynamic limit | — |

### 6.2 Mass per Area

| Form Factor | Mass (kg/m²) | W/kg | Notes |
|-------------|-------------|------|-------|
| Ultra-thin film (no structure) | 0.1–0.3 | ~4,500 | Theoretical floor |
| Thin film + minimal support | 0.5–1.5 | ~900–2,700 | Tensioned membrane |
| Rigid panels (traditional) | 2–5 | ~270–680 | Heritage space-grade |

### 6.3 Degradation Model

**Radiation degradation:**
- Trapped protons + electrons (Van Allen belts, inner edge)
- Equivalent 1 MeV electron fluence → efficiency loss curves
- Annual loss rate: ~1–3%/year in LEO (altitude dependent)

**Thermal cycling:**
- Eclipse transitions cause rapid swings: +100°C → −150°C
- Above 574 km (eclipse-free): thermal cycling eliminated

**Temperature coefficient:**
- GaAs: ~−0.2%/°C relative to 25°C
- Equilibrium temp in full sun at LEO: ~60–80°C (with radiative cooling)

### 6.4 Panel Thermal Equilibrium

```
Energy balance: S₀ × α_abs × A = ε × σ × T⁴ × 2A + P_electrical

where:
  α_abs = solar absorptance (~0.85–0.92)
  ε = thermal emittance (~0.80–0.85)
  σ = Stefan-Boltzmann constant
  T = equilibrium temperature [K]
  Factor of 2: both sides radiate
  P_electrical = converted power (reduces thermal load)
```

---

## 7. Launch Economics

### 7.1 Cost Tiers (Starship Baseline)

| Tier | Description | $/kg to SSO LEO |
|------|-------------|----------------|
| Floor (marginal) | Fuel + consumables + ground ops only. Zero capex. | ~$10–30 |
| Optimistic | Amortized over 1,000 flights. Low refurb. | ~$50–100 |
| Realistic | 100–300 flights. Realistic refurb & overhead. | ~$200–500 |
| Pessimistic | Current commercial pricing (Falcon 9 level) | ~$2,000–5,000 |

### 7.2 Starship Fuel Cost per Launch

```
LOX + CH4 propellant: ~$500K–$1M per launch (full stack)
Payload to LEO: ~100–150 tonnes
Marginal fuel cost: ~$3–10/kg
```

### 7.3 Deployed Cost Formula

```
$/W_deployed = ($/kg_launch × kg/W_panel) + $/W_panel_hardware

where kg/W depends on panel form factor and efficiency
```

---

## 8. Debris & Micrometeoroid Risk

### 8.1 NASA90 Analytical Debris Model (SPENVIS)

Cumulative flux (impacts/m²/year for objects ≥ d_cm):

```
F(d, h, i, t, S) = Φ(h,S) × Ψ(i) × [F₁(d)·g₁(t) + F₂(d)·g₂(t)]
```

**Size distribution:**
```
F₁(d) = 1.22×10⁻⁵ × d^(−2.5)
F₂(d) = 8.1×10¹⁰ × (d + 700)^(−6)
```

**Altitude factor:**
```
Φ₁(h, S) = 10^(h/200 − S/140 − 1.5)
Φ(h, S) = Φ₁ / (1 + Φ₁)

where S = F10.7 solar flux (~70 solar min, ~110 avg, ~140 solar max)
```

**Inclination factor Ψ(i):**

| Inclination (°) | 0–28.5 | 50 | 60 | 70 | 80 | 90 | 100 |
|------------------|--------|-----|-----|-----|-----|-----|------|
| Ψ(i) | 0.91 | 1.0 | 1.07 | 1.2 | 1.4 | 1.6 | 1.78 |

**Time growth (debris accumulation):**
```
g₁(t) = (1 + q)^(t − 1988)    q = 0.02 (pre-2011), 0.04 (post-2011)
g₂(t) = 1 + p × (t − 1988)    p = 0.05
```

Source: [SPENVIS](https://www.spenvis.oma.be/help/background/metdeb/metdeb.html)

**Caveats:** NASA90 pre-dates Fengyun-1C ASAT (2007, +25% tracked population at ~850 km), Iridium-Cosmos collision (2009, +2300 fragments at ~780 km), and Cosmos 1408 ASAT (2021, fragments at ~500 km). Empirical calibration required.

### 8.2 Empirical Altitude Profile

Relative spatial density (normalized to peak at 800 km = 1.0):

| Altitude (km) | Relative Density | Source/Anchor |
|----------------|-----------------|---------------|
| 200 | 0.01 | Rapid atmospheric clearing |
| 300 | 0.02 | NRC: 50× less than 1000 km |
| 400 | 0.05 | ISS altitude, validated |
| 500 | 0.10 | Atmospheric drag still significant |
| 600 | 0.25 | Hubble altitude |
| 700 | 0.60 | Sharply rising |
| 800 | **1.00** | Peak zone (reference) |
| 850 | **1.05** | Highest point |
| 900 | 0.95 | Near peak |
| 1000 | 0.80 | Still very high |
| 1100 | 0.50 | Declining |
| 1200 | 0.30 | Moderate |
| 1400 | 0.40 | Secondary peak (nav sats) |
| 1500 | 0.35 | Declining from secondary |
| 1800 | 0.10 | Low |
| 2000 | 0.05 | Edge of LEO |

Sources: NASA ORDEM 3.2, ESA MASTER-8, NRC 1995 (50× ratio), HAX radar (10× ratio)

### 8.3 ESA Population Counts (Aug 2024)

| Size Class | Count | Damage Mode |
|------------|-------|-------------|
| > 10 cm | 54,000 | Catastrophic destruction |
| 1–10 cm | 1,200,000 | Structural failure |
| 1 mm–1 cm | 140,000,000 | Panel penetration, localized |
| Sub-mm | Billions | Surface pitting, gradual efficiency loss |

Source: ESA MASTER-8, ESA Space Environment Report 2025

### 8.4 Shielding & Mitigation

- **Whipple shields** effective up to ~1 cm at LEO collision velocities (~10 km/s)
- Objects ≥ 10 cm are tracked and can be avoided via maneuver
- The **1–10 cm gap** is the most dangerous: too small to track, too large to shield
- Modular panel design limits damage propagation from individual impacts

### 8.5 Orbital Lifetime (Natural Deorbit)

| Altitude (km) | Approx. Lifetime | Regulatory Status |
|----------------|-----------------|-------------------|
| 200 | Days | N/A |
| 300 | Months | Easy compliance |
| 400 | ~1 year | Easy |
| 500 | ~5 years | Compliant |
| 600 | ~25 years | 25-year rule OK |
| 700 | ~100 years | Marginal |
| 800 | ~500 years | Non-compliant |
| 1000 | ~2,000 years | Problem |
| 1200 | ~5,000 years | Active deorbit required |

Source: NASA Orbital Debris Program Office

---

## 9. Station-Keeping & Propulsion

### 9.1 Propulsion Options

| Type | I_sp (s) | Thrust | Propellant Mass | Power Need |
|------|---------|--------|-----------------|------------|
| Chemical (hydrazine) | 220–230 | High | Heavy | Low |
| Hall thruster | 1,500–3,000 | Low | ~10× less | Moderate (~1 kW) |
| Ion thruster | 3,000–10,000 | Very low | ~20× less | High |

**Key advantage:** Solar panels can power their own electric propulsion for station-keeping.

### 9.2 Solar Radiation Pressure

```
P_SRP = S₀ / c ≈ 4.56 μN/m²

F_SRP = P_SRP × A × (1 + r)  [for reflective surface, r = reflectivity]
```

Creates torques when center of pressure ≠ center of mass. Becomes significant for arrays > 10,000 m².

---

## 10. Optimal Deployment Zone

Based on all the above, the optimal altitude band is **600–700 km**:

| Factor | 400–574 km | 574–700 km | 700–800 km | 800+ km |
|--------|-----------|-----------|-----------|---------|
| Eclipse | Yes (needs batteries) | **None** | None | None |
| Drag | Severe | **Moderate** | Low | Negligible |
| Debris | Moderate | **Lower** | Rising | Peak |
| Radiation | Moderate | **Moderate** | Higher | Inner belt edge |
| Natural deorbit | < 5 years | **25–50 years** | 100+ years | 500+ years |
| Regulatory | Easy | **Compliant** | Marginal | Problem |

---

## 11. Numerical Integration Parameters (Visualization)

Used in `index.html` for computing averages:

| Calculation | Sampling |
|-------------|----------|
| Sunlit fraction (per orbit) | 360 points around orbit |
| Annual space flux | 365 days |
| Ground daily flux | 240 points per 24h (6-min intervals) |
| Ground annual flux | 876 points (365 × 24 ÷ 0.5h) |
| GEO constellation | 3 satellites, 120° spacing |
| ISS RAAN sweep | 36 steps (10°/step) per day |

---

## 12. Key Conversion Factors

| From | To | Factor |
|------|-----|--------|
| 100% solar | W/m² | × 1,361 |
| Degrees | Radians | × π/180 |
| km/s² | kWh/kg (kinetic) | ½v² / 3,600,000 |
| Earth radii | km | × 6,371 |

---

## 13. Scale Target

**1 GW deployed capacity** in sun-synchronous LEO at 600–700 km.

At 30% panel efficiency and 1,361 W/m² irradiance:
```
Power per m² = 1,361 × 0.30 = 408 W/m²
Area needed = 1 GW / 408 W/m² ≈ 2.45 million m² ≈ 2.45 km²
```

At 1 kg/m² (thin film + structure):
```
Mass = 2.45 million kg = 2,450 tonnes
Starship launches needed = 2,450 / 100 ≈ 25 launches
```

At $200/kg launch cost:
```
Launch cost alone = 2,450,000 kg × $200/kg = $490 million
= $0.49/W launch cost
```

---

## 14. Launch Cost Learning Curve (Wright's Law)

Source: Agüera y Arcas et al., arXiv:2511.19468, Nov 2025.

### 14.1 Wright's Law Model

```
C(Q) = C₁ × Q^(log₂(1 − r))

where:
  C(Q) = cost per kg at cumulative quantity Q (tonnes)
  C₁ = cost at first unit
  r = learning rate (fraction cost drops per doubling)
  log₂(1 − r) = experience exponent
```

### 14.2 SpaceX Historical Data

| Vehicle | $/kg to LEO | Cumulative Mass |
|---------|-------------|-----------------|
| Falcon 1 | ~$30,000 | ~few tonnes |
| Falcon 9 v1 | ~$5,000 | ~100 tonnes |
| Falcon Heavy | ~$1,800 | ~400 tonnes |

Learning rate: ~20% (price falls ~20% for every doubling of cumulative mass launched).
Range: 18–24% depending on assumptions.

### 14.3 Projections to $200/kg

- Requires ~370,000 tonnes additional cumulative mass
- At 200 tonnes/launch (Starship): ~1,800 launches
- At ~180 launches/year: achievable by mid-2030s
- If only 104,000 tonnes (72% reduction): $300/kg by mid-2030s

### 14.4 Starship Cost Breakdown

| Reuse Level | $/kg (cost) | $/kg (with 75% margin) |
|-------------|-------------|------------------------|
| No reuse | ~$460 | — |
| 10× reuse | <$60 | <$250 |
| 100× reuse | <$15 | ~$38 |
| Fuel floor | $8 | — |

---

## 15. Radiation Environment for Compute Hardware

Source: Google Trillium TPU radiation testing, UC Davis Crocker Nuclear Lab, 67 MeV protons.

### 15.1 Orbital Dose Rate

```
At 650 km SSO with 10 mm Al equivalent shielding:
  Annual dose: ~150 rad(Si)/year
  5-year total: ~750 rad(Si)
```

Primarily penetrating protons + galactic cosmic rays. Proton range at 67 MeV: ~18 mm Al, 6.5 mm Cu.

### 15.2 TPU Error Rates (Trillium)

| Component | Event Type | Rate | Cross-section |
|-----------|-----------|------|---------------|
| HBM memory | Uncorrectable ECC (UECC) | 1 per 50 rad | 2×10⁻⁹ cm²/chip |
| HBM memory | Silent data corruption | 1 per 10⁷ rad | 8.3×10⁻¹⁰ cm²/assembly |
| Core logic/SRAM | SEE failure | 1 per 150 rad | 7×10⁻¹⁰ cm²/chip |
| CPU | Crash (SEFI) | 1 per 450 rad | — |
| RAM | SEFI | 1 per 400 rad | — |
| System-level | SEFI | 1 per 5 krad | 2×10⁻¹¹ cm²/chip |

### 15.3 Survival Limits

- **Permanent failure**: None observed up to 15 krad (20× the 5-year requirement)
- **HBM stress anomalies**: Begin at 2 krad (2.7× the 5-year minimum)
- **Key vulnerability**: HBM memory, not logic — ECC helps but uncorrectable errors occur

### 15.4 Operational Impact (at 650 km)

At 150 rad/year:
- HBM UECC: ~3 events/year/chip
- Core logic SEE: ~1 event/year/chip
- System crash: ~1 event per 3 years per chip
- **~1 inference failure per 10 million inferences**

---

## 16. Space vs Terrestrial Power Economics

Source: arXiv:2511.19468 analysis of launched power cost.

### 16.1 Launched Power Cost

```
$/kW/year = (mass_satellite [kg] × launch_cost [$/kg]) / (power [kW] × lifespan [years])
```

| System | Mass (kg) | Power (kW) | Life (yr) | At $3,600/kg | At $200/kg |
|--------|-----------|------------|-----------|--------------|------------|
| Starlink v2 mini | 575 | 28 | 5 | $14,700/kW/yr | $810/kW/yr |
| Starlink v1 | 260 | 7 | 5 | $26,600/kW/yr | $1,470/kW/yr |
| OneWeb | 150 | 0.8 | 5 | $135,800/kW/yr | $7,500/kW/yr |

### 16.2 Terrestrial Datacenter Power

- US power cost: $0.06–0.25/kWh
- PUE (power usage effectiveness): 1.09–1.4
- Annualized: **$570–3,000/kW/year**

### 16.3 Crossover

**At $200/kg launch cost, optimized space power ($810/kW/yr) reaches approximate parity with terrestrial datacenter power ($570–3,000/kW/yr).**

This is the key economic threshold for the space solar thesis.

---

*Last updated: 2026-02-26*
*Sources: NASA ORDEM 3.2, ESA MASTER-8, ESA Space Environment Report 2025, SPENVIS/NASA90, NRLMSISE-00, NRC 1995, SORCE/TIM, Kopp & Lean 2011, Kasten & Young 1989, Meinel & Meinel, CODATA 2018, WGS-84, EGM96*

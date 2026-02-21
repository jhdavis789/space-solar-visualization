# Space Solar Power — Equations, Constants & Data

All equations and constants used across the visualization (`index.html`) and economics dashboard (`dashboard.html`). Every assumption is documented with its source or derivation.

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
```

Atmospheric density varies 10–100× between solar minimum and solar maximum at 600 km due to solar cycle heating of the thermosphere.

**ΔV budget for station-keeping:**
- 600 km: ~10–50 m/s/year (manageable with Hall thrusters)
- Below 500 km: drag becomes prohibitive for large arrays

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

### 8.1 Flux by Size Class (LEO)

| Size Class | Flux (impacts/m²/year) | Damage Mode |
|------------|----------------------|-------------|
| Sub-mm dust | ~10⁻¹ to 10⁰ | Surface pitting, gradual efficiency loss |
| mm-scale | ~10⁻³ to 10⁻² | Panel penetration, localized damage |
| cm-scale | ~10⁻⁵ to 10⁻⁴ | Structural failure of segment |
| 10 cm+ | ~10⁻⁷ to 10⁻⁶ | Catastrophic, debris generation |

Source: NASA ORDEM (Orbital Debris Engineering Model), ESA MASTER

### 8.2 Debris Density by Altitude

Peak debris density: **800–900 km** (worst zone — legacy debris from Cosmos-Iridium collision, Chinese ASAT test).

600–700 km: significantly lower debris flux than 800–900 km band.

### 8.3 Kessler Syndrome Risk

Current LEO debris growth rate is accelerating. Over a 10-year horizon, debris flux at all altitudes is expected to increase, particularly at 700–1,000 km.

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

*Last updated: 2026-02-20*
*Sources: NASA ORDEM, SORCE/TIM, Kopp & Lean 2011, Kasten & Young 1989, Meinel & Meinel, CODATA 2018, WGS-84, EGM96*

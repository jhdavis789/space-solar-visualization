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

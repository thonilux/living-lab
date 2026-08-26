# Sensing Layer

ESP node firmware/config for reading all sensors and forwarding to Raspberry Pi.

**Indoor sensors**
- Air Temperature (Ta) — 3x
- Relative Humidity (RH) — 3x
- Illuminance (Lux) — 3x
- Air Velocity (v) — 1x
- Occupancy (camera, occupant counting) — 1x

**Energy meters**
- AC Energy Meter (P_AC, E_AC)
- Total Lighting Energy Meter (P_light, E_light)

**Outdoor sensors**
- Outdoor Temperature, Outdoor RH, Solar Irradiance (G), Outdoor Illuminance (optional)

**Reference instrument**
- Black Globe Thermometer (portable, periodic use for T_r estimator training/validation)

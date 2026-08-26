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

**Sensor models (RS485 Modbus RTU)**
- Solar Irradiance (G): Halisense HLS-SI — [manual](../docs/manuals/HLS-SI-solar-irradiance-sensor-manual.zip) ([source](https://wiki20210805.oss-cn-hongkong.aliyuncs.com/halisense/HLS-SI%20Solar%20irradiance%20sensor%20manual.zip))
- Air Velocity (v): Halisense HLS-DTAS — [manual](../docs/manuals/HLS-DTAS-air-velocity-sensor-manual.rar) ([source](https://wiki20210805.oss-cn-hongkong.aliyuncs.com/halisense/HLS-DTAS%20air%20velocity%20sensor%20manual.rar))

Both wired to ESP node via RS485-to-TTL (MAX485) transceiver; read over Modbus RTU using `ModbusMaster` lib (see [platformio.ini](platformio.ini)).

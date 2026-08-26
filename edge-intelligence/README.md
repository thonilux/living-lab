# Edge Intelligence (Raspberry Pi 5)

Core intelligence layer running on Raspberry Pi 5. Per architecture diagram:

- **Data Management** — time-series database, logs all sensor/prediction/control data
- **Occupant Counting** — computer vision (real-time + prediction input)
- **Prediction Inference** — predicts indoor temperature, illuminance, occupancy, energy (AC/lighting)
- **T_r Estimation** — Random Forest / XGBoost / regression model estimating mean radiant temperature from Ta, RH, G, v
- **Comfort Calculation** — PMV/PPD model (ISO 7730 / ASHRAE 55)
- **MOPSO Optimization** — Multi-Objective Particle Swarm Optimization; minimizes energy, maximizes comfort; outputs T_set*, PWM_1*, PWM_2*
- **Control Command Generator** — produces optimal control commands, sent to ESP node for actuation

Communicates with sensing/actuation ESP nodes via Wi-Fi/MQTT.

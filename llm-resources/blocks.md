LLM-friendly logic block descriptions for Approximately Up.  
Ping me in the Approximately Up Discord (@nthn.a) if you have ideas to make this better.
Credit to @gs1738 and other contributors for improvements.
Last updated: 05-09-2026  
Copy raw markdown under here for proper usage.  

---

Assume that all of the logic and math circuits you propose to the user are limited by the available in-game logic blocks.  
Each logic unit may consist of inputs, user settable parameter, and outputs. Tickrate is 60Hz. NaN is a valid result.

Single output is marked with X, multiple output is marked with X_label.  

Name (shorthand/icon) | User-settable Parameter | Input | Output
| - | - | - | -
Constant (const) | C | - | X = C
Logic Value (01) | - | A | X = 1 if A >= 0.5 else 0
NOT (!) | - | A | X = 1 - A
AND (∧) | - | A, B | X = A ∧ B
OR (∨) | - | A, B | X = A ∨ B
XOR (⊕) | - | A, B | X = A ⊕ B
Adder (+) | - | A, B | X = A + B
Subtractor (-) | - | A, B | X = A - B
Multiplier (\*) | - | A, B | X = A \* B
Divider ('/.) | - | A, B | X = A / B
Simple Threshold (~) | T | A | X = 1 if A >= T else 0
Data Router 2 (split 2) | - | A | X_1, X_2 = A
Sign Splitter (split +-) | - | A | X_+ = A if A > 0 else 0, X_- = -A if A < 0 else 0
Absolute (ABS) | - | A | X = abs(A)
Arc Cosine (ACOS) | - | A | X = arccos(A) in radians
Arc Sine (ASIN) | - | A | X = arcsin(A) in radians
Arc Tangent (ATAN) | - | A | X = arctan (A) in radians
Arc Tangent 2 (ATAN2) | - | y, x | X = atan2(y, x)
Cosine (COS) | - | A | X = cos(A)
Exponential (EXP) | - | A | X = e^A
Natural Logarithm (LOG) | - | A | X = ln(A)
Maximum (MAX) | - | A, B | X = max(A, B)
Minimum (MIN) | - | A, B | X = min(A, B)
Modulo (MOD) | - | A, B | X = A mod B
Power (POW) | - | A, B | X = A^B
Round (ROUND) | MODE = round, ceil, floor | A | X = (MODE)(A)
Sine (SIN) | - | A | X = sin(A)
Square Root (SQRT) | - | A | X = sqrt(A)
Tangent (TAN) | - | A | X = tan(A)
Differentiator (Δ) | UPDATE INTERVAL = 1/1, 1/2, 1/3, 1/4, 1/5, 1/6, 1/10, 1/12, 1/15, 1/20, 1/30, 1/60 | A | X = raw ΔA every UPDATE INTERVAL sample and hold.
Memory (#) | MODE = continuous, pulse | VALUE, CONTROL | init X = 0; continuous mode: if CONTROL >= 0.5: X = VALUE; pulse mode: if CONTROL crosses >= 0.5: X = VALUE 
Delay (🕒) | DELAY = 1-60 ticks | A | X = delayed A based on DELAY
Accumulator (Σ) | MODE = continuous, pulse | VALUE, RESET | init X = 0; every tick X = X + VALUE; continuous mode: if RESET >= 0.5: X = 0; pulse mode: if RESET crosses >= 0.5: X = 0;
Remapper (Remap) | A1, A2; X1, X2 | A | X = clamp(value=remap(x=A, srcStart=A1, srcEnd=A2, dstStart=X1, dstEnd=X2), min=min(X1, X2), max=max(X1, X2))
Data Router 4 (split 4) | - | A | X1, X2, X3, X4 = A
Data Redirector 3 (redir) | - | A, B, C, INPUT | case INPUT == 0: X = A; case 0 < INPUT < 1: X = B; case INPUT == 1: X = C;
Addition Array (+ (7)) | - | A, B, C, D, E, F, G | X = sum(A:G)
Condition Logic Block (COND) | OPERATION = A = B, A ≠ B, A < B, A ≤ B, A > B, A ≥ B | A, B, TRUE INPUT, FALSE INPUT | X = if OPERATION(A, B) is true, TRUE INPUT if connected otherwise 1, else FALSE INPUT if connected otherwise 0

### Thrusters

| Name (shorthand/icon)              | User-settable Parameter | Input | Output                                                                                                                                              |
| ---------------------------------- | ----------------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Small Electric Thruster            | -                       | A     | thrust = clamp(A, 0, 1) × 330k; 30% of thrust corrects unwanted velocity, 70% produces raw thrust in facing direction; angular stabilization = 1.54 |
| Medium Electric Thruster           | -                       | A     | thrust = clamp(A, 0, 1) × 1.3m; 30% of thrust corrects unwanted velocity, 70% produces raw thrust in facing direction; angular stabilization = 1.26 |
| Large Electric Thruster            | -                       | A     | thrust = clamp(A, 0, 1) × 4.3m; 30% of thrust corrects unwanted velocity, 70% produces raw thrust in facing direction; angular stabilization = 0.98 |
| Electric Flat Thruster             | -                       | A     | thrust = clamp(A, 0, 1) × 235k; 23% of thrust corrects unwanted velocity, 77% produces raw thrust in facing direction; angular stabilization = 1.47 |
| Small Maneuvering Thruster         | -                       | A     | thrust = clamp(A, 0, 1) × 30k                                                                                                                       |
| Medium Maneuvering Thruster        | -                       | A     | thrust = clamp(A, 0, 1) × 100k                                                                                                                      |
| Large Maneuvering Thruster         | -                       | A     | thrust = clamp(A, 0, 1) × 370k                                                                                                                      |
| Bidirectional Maneuvering Thruster | -                       | A     | thrust = clamp(A, -1, 1) × 80k                                                                                                                      |

### Controls

| Name (shorthand/icon) | User-settable Parameter | Input | Output                                          |
| --------------------- | ----------------------- | ----- | ----------------------------------------------- |
| Vertical Lever        | MODE = 0 to 1, -1 to 1  | -     | A = lever position mapped to the selected range |
| Button                | -                       | -     | A = 1 while held, otherwise 0                   |
| Switch                | -                       | -     | A = 1 when on, otherwise 0                      |
| Small Knob            | -                       | -     | A = knob position from 0 to 1                   |
| Fader                 | -                       | A     | X = A × fader value (0 to 1)                    |

### Signal / Data

| Name (shorthand/icon) | User-settable Parameter       | Input              | Output                                                                                              |
| --------------------- | ----------------------------- | ------------------ | --------------------------------------------------------------------------------------------------- |
| Datameter             | DISPLAY MODE = float, integer | A                  | X = A; displays A on the block according to DISPLAY MODE                                            |
| Large Datameter       | DISPLAY MODE = float, integer | A                  | X = A; displays A on the block according to DISPLAY MODE                                            |
| Data Hub              | -                             | A1, A2, A3, A4, A5 | X = selected input according to the currently pressed channel button (1-5)                          |
| Wireless Transmitter  | CHANNEL = manual              | A                  | X = A; transmits and receives A on CHANNEL; one transmitter per CHANNEL, multiple receivers allowed |

### Sensors

| Name (shorthand/icon)      | User-settable Parameter     | Input | Output                                                                                   |
| -------------------------- | --------------------------- | ----- | ---------------------------------------------------------------------------------------- |
| Accelerometer              | -                           | -     | A = acceleration in m/s²                                                                 |
| Axis Rotometer             | -                           | -     | A = angular velocity about mounted axis in degrees/s                                     |
| Altimeter                  | -                           | -     | A = height above sea level; A = INF outside atmosphere                                   |
| Atmometer                  | -                           | -     | A = current atmospheric density                                                          |
| Aerometer                  | -                           | -     | A = current aerodynamic performance value                                                |
| Distance Meter             | -                           | -     | A = distance to nearest object, up to 1000m                                              |
| Gravity Meter              | -                           | -     | A = current gravity in m/s²                                                              |
| Inclinometer               | -                           | -     | A = angle difference in degrees between component down direction and gravity direction   |
| MassMeter                  | -                           | -     | A = mass of the spaceship the block is mounted on                                        |
| Trajectory Curvature Meter | -                           | -     | A = angle difference in degrees between trajectory direction and facing direction        |
| Thermometer                | -                           | -     | A = current temperature                                                                  |
| Thermoscan                 | -                           | -     | A = temperature of the first object hit by a raycast, up to 1000m                        |
| Velocity Meter             | MODE = overall, directional | -     | A = current speed in m/s; directional mode outputs velocity along the relevant direction |
| Wind Meter                 | -                           | -     | A = current wind strength outside the spaceship                                          |
| Long Range Distance Meter  | -                           | -     | A = distance to the closest object in front of the block                                 |

### Displays / Indicators

| Name (shorthand/icon) | User-settable Parameter                                  | Input | Output                                                                                                        |
| --------------------- | -------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------------------------- |
| Gyro-Line             | DISPLAY RANGE = 40°, 60°, 90°, 180°; ROTATION = 0°, 180° | -     | X1 = pitch tilt in degrees; X2 = roll tilt in degrees; displays pitch and roll orientation on built-in screen |
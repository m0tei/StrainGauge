# Amplifier calculations

U2 is an MCP6N16-100 instrumentation amplifier. It takes the bridge signal and sets the output with two resistors. The figures below use gain 151.4.

$$V_{\text{OUT}} = V_{\text{REF}} + G \times V_{\text{IN}} \qquad G = 1 + \frac{R2}{R1}$$

## Choosing the gain

The biggest input is 3000 µε × 4.29 µV per µε = 12.87 mV.

| Limit | Calculation | Gain |
|---|---|---|
| Lowest stable gain (datasheet) | Fixed by the 100 version of the part | 100 |
| Highest gain before the output clips | (3.25 V − 0.825 V) / 12.87 mV | 188 |
| Our choice, R2 = 100 kΩ, R1 = 665 Ω | 1 + 100k / 665 | 151.4 |

We sit between the two limits, so the output has room left for bridge imbalance.

## Output

| Input | Calculation | Output |
|---|---|---|
| Per µε | 4.29 µV × 151.4 | 0.649 mV |
| 3000 µε compression | 0.825 V + 12.87 mV × 151.4 | 2.77 V |
| 300 µε extension | 0.825 V − 1.29 mV × 151.4 | 0.63 V |

## Checks against the datasheet

| Check | Calculation | Result | Limit |
|---|---|---|---|
| Input size | 12.87 mV | 12.9 mV | 27 mV |
| Bandwidth | 350 kHz × 100 / 151.4 | 231 kHz | Above the 480 Hz filter |
| R2 stability | 0.25 × 151.4² / (2π × 35 MHz × 16 pF) | 1.6 MΩ limit | R2 is 100 kΩ |
| Noise | 45 nV/√Hz × √(1.57 × 480 Hz) | 1.24 µV rms, 0.29 µε | Below one ADC count |
| Offset | 17 µV / 4.29 µV per µε | 4 µε maximum | Removed by calibration |
| Gain error | 0.05% + 0.02% + 0.15% | 0.22% maximum | Removed by calibration |

The 16 pF is 14 pF of amplifier input capacitance plus 2 pF of board stray, which is our estimate. The 4.29 µV per µε assumes 3.3 V excitation, gauge factor 2, and Poisson ratio 0.3.

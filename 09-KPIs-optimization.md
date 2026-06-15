# 09 — KPIs & Network Optimization

> **Links:** [← 6G](./08-6G.md) | [README](./README.md) | [Quick Reference →](./10-quick-reference.md)

---

## Why KPIs Matter for NDO

An NDO engineer's daily job is:
1. Monitor KPIs across a live network
2. Identify cells/sites below threshold
3. Diagnose root cause
4. Implement fix (parameter change, hardware fix, configuration change)
5. Verify KPI improvement

---

## Core KPIs

### 1. RSRP — Reference Signal Received Power

| Attribute | Detail |
|---|---|
| What it measures | **Signal strength** — power of LTE reference signal at UE |
| Unit | dBm |
| Good | > -80 dBm (excellent) |
| Acceptable | -80 to -100 dBm |
| Poor | < -100 dBm |
| Critical | < -110 dBm (coverage hole) |

**What RSRP tells you:** Whether the UE can physically receive the signal. Low RSRP = coverage problem.

**Root causes of low RSRP:**
- UE too far from base station (coverage hole)
- Physical obstruction (building, terrain)
- Antenna tilt too aggressive
- Antenna failure or feeder loss

**Fix:** Increase antenna height, reduce tilt, add new cell site, increase transmit power

---

### 2. RSRQ — Reference Signal Received Quality

| Attribute | Detail |
|---|---|
| What it measures | **Signal quality** accounting for interference from other cells |
| Formula | RSRQ = N × RSRP / RSSI (N = number of RBs) |
| Unit | dB |
| Good | > -10 dB |
| Acceptable | -10 to -15 dB |
| Poor | < -15 dB |

**RSRP vs RSRQ:** You can have good RSRP (strong signal) but bad RSRQ (strong signal + even stronger interference from neighboring cell).

**Root causes of poor RSRQ:**
- High interference from neighboring cells (co-channel interference)
- Pilot pollution (too many cells visible at same strength)
- High cell load

**Fix:** Adjust frequency plan, modify neighbor list, antenna tilt optimization, increase PCI reuse distance

---

### 3. SINR — Signal to Interference and Noise Ratio

| Attribute | Detail |
|---|---|
| What it measures | **Link quality** — how much stronger signal is vs interference + noise |
| Unit | dB |
| Excellent | > 20 dB (256-QAM possible) |
| Good | 10–20 dB |
| Acceptable | 0–10 dB (QPSK to 16-QAM range) |
| Poor | < 0 dB (QPSK with heavy coding) |

**SINR directly determines:**
- Which MCS (Modulation and Coding Scheme) can be used → throughput
- Whether HARQ retransmissions are needed → latency
- Cell edge performance

---

### 4. CSSR — Call Setup Success Rate

| Attribute | Detail |
|---|---|
| What it measures | % of call/session setup attempts that succeed |
| Formula | Successful setups / Total setup attempts × 100% |
| Target (SLA) | **> 99%** |
| Warning | < 98% |
| Critical | < 95% |

**Root causes of low CSSR:**
- RACH congestion (too many UEs trying to access simultaneously)
- Signaling channel congestion (PDCCH, PUCCH)
- Backhaul congestion
- Hardware fault at eNB
- Coverage hole causing failed RRC setup

**Diagnosis steps:**
1. Check RACH success rate separately
2. Check RRC setup success rate
3. Check S1 (eNB to MME) link quality
4. Check hardware alarms
5. Correlate with time of day (peak hour issue → congestion; all hours → hardware/coverage)

---

### 5. CDR — Call Drop Rate

| Attribute | Detail |
|---|---|
| What it measures | % of successfully connected calls/sessions that drop before intended completion |
| Formula | Dropped calls / (Successful setups + Incoming handovers) × 100% |
| Target | **< 0.5%** |
| Warning | > 1% |
| Critical | > 2% |

**Root causes:**
- Coverage hole during mobility (handover too late)
- Failed handover (X2 link down, target cell congested)
- Rapid signal drop (fast-moving UE)
- RLF (Radio Link Failure) — SINR drops below threshold with no HO alternative

**Diagnosis:**
1. Check handover success rate (if low → HO problem → check X2, neighbor lists)
2. Check coverage maps — drop in which area?
3. Check if drops correlate with speed (drive test data)

---

### 6. HOSR — Handover Success Rate

| Attribute | Detail |
|---|---|
| What it measures | % of handover attempts that complete successfully |
| Target | **> 98%** |
| Warning | < 95% |

**Root causes of low HOSR:**
- Target cell doesn't respond (hardware fault, backhaul issue)
- Too-late handover (UE signal already too low by time HO executes)
- Too-early handover (UE handed to a cell that's too weak → ping-pong → HO failure)
- Missing neighbor relation (cell not in neighbor list → UE can't be handed to best cell)
- X2 interface failure

**SON feature:** ANR (Automatic Neighbour Relation) — automatically detects and adds missing neighbours.

---

### 7. PRB Utilization — Physical Resource Block Utilization

| Attribute | Detail |
|---|---|
| What it measures | % of resource blocks in use out of total available |
| Unit | % |
| Normal | <70% |
| High | 70–85% — monitor closely |
| Congested | **>85%** — action needed |

**Why it matters:** When PRB utilization is consistently high, new users can't get resources → poor throughput for everyone → effectively congestion.

**Fix:** Add capacity (carrier aggregation, new cell site, small cells), offload traffic to WiFi, optimize scheduler settings.

---

### 8. Throughput

| Metric | Typical Benchmark |
|---|---|
| Cell average DL throughput | Varies by band/BW — compare against design target |
| Cell edge throughput (5th percentile) | Should be >25% of cell average |
| Peak throughput per UE | Depends on MIMO config and bandwidth |

**Throughput degradation diagnosis:**
1. Low SINR → UE using low MCS → low throughput
2. High PRB utilization → congestion → UE gets fewer resources
3. MIMO rank reduction → signal conditions don't support multiple streams
4. High HARQ retransmission rate → effective throughput drops

---

## Optimization Concepts

### ANR — Automatic Neighbour Relation (SON)
- eNB automatically detects neighbors via UE measurement reports
- Automatically adds missing neighbors to neighbor list
- Avoids manual configuration — NDO automation

### MLB — Mobility Load Balancing (SON)
- When one cell is overloaded, SON shifts UEs to lightly loaded adjacent cells
- Adjusts HO parameters (A3 offset threshold)

### MRO — Mobility Robustness Optimization (SON)
- Automatically fixes handover problems:
  - Too-early HO → increase A3 offset
  - Too-late HO → decrease A3 offset
  - Wrong-cell HO → fix neighbor list

### CCO — Coverage and Capacity Optimization (SON)
- Adjusts transmit power and antenna tilt automatically
- Balance coverage vs capacity based on traffic patterns

---

## Link Budget and Cell Range

### Typical LTE Link Budget (Simplified)

```
Tx Power (eNB):      46 dBm
Antenna Gain:        +18 dBi
Feeder Loss:         -2 dB
EIRP:                62 dBm
Path Loss:           -[depends on range]
Penetration Loss:    -20 dB (indoor)
Rx Antenna Gain:     0 dBi (UE)
Noise Figure (UE):   -9 dB
Rx Sensitivity:      -100 dBm

Max Path Loss = 62 + 0 - 20 - (-100) - 9 = 133 dB
```

### Path Loss Models

| Model | Formula | Applicable Range |
|---|---|---|
| Free Space | 20 log(d) + 20 log(f) + 32.4 | Open, LOS |
| Okumura-Hata | Complex formula, 150–1500 MHz | Urban/suburban |
| COST-231-Hata | Extended Hata for 1.5–2 GHz | Urban |

Higher frequency → more path loss → smaller cell range at same power.

---

## NDO Diagnostic Framework

When a KPI is degraded, use this systematic approach:

```
1. SCOPE: Which cells? How many? Geographic pattern?
2. TIME: When did it start? Constant or intermittent? Time of day?
3. CORRELATE: Other KPIs affected? Alarms? Recent changes?
4. ISOLATE: Hardware fault? Coverage issue? Congestion? Config error?
5. FIX: Parameter change / hardware repair / config update
6. VERIFY: Did the KPI recover? Any side effects on adjacent cells?
```

---

> **Next:** [10 — Quick Reference →](./10-quick-reference.md)

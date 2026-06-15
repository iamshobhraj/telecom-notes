# 10 — Quick Reference Cheat Sheet

> **Links:** [← KPIs & Optimization](./09-KPIs-optimization.md) | [README](./README.md)

> Use this for final revision. All important tables in one place.

---

## Generation Overview

| Gen | Year | Speed | Latency | Multiple Access | Key Technology |
|---|---|---|---|---|---|
| 1G | 1979 | Analogue voice | — | FDMA | Analogue |
| **2G GSM** | 1991 | 9.6 kbps | ~500 ms | TDMA + FDMA | Digital voice, SMS |
| **2G GPRS** | 2000 | 172 kbps | ~300 ms | TDMA + FDMA | Packet data (2.5G) |
| **2G EDGE** | 2003 | 384 kbps | <100 ms | TDMA + FDMA | 8PSK modulation (2.75G) |
| **3G UMTS** | 2003 | 2 Mbps | 150 ms | CDMA (WCDMA) | Wideband CDMA, 5 MHz |
| **3G HSPA** | 2005 | 14.4 Mbps DL | 100 ms | CDMA | HSDPA, shared channel, AMC |
| **3G HSPA+** | 2008 | 28–84 Mbps | 50 ms | CDMA | MIMO, 64-QAM, DC |
| **4G LTE** | 2009 | 100 Mbps | 10 ms | OFDMA / SC-FDMA | All-IP, flat architecture |
| **4G LTE-A** | 2014 | 1 Gbps | <5 ms | OFDMA / SC-FDMA | Carrier aggregation, CoMP |
| **5G NR** | 2018 | 20 Gbps | 4 ms | OFDMA (flexible) | Massive MIMO, slicing, mmWave |
| 6G | ~2030 | TBD | <1 μs | TBD | THz, full duplex |

---

## Node Name Evolution

| Function | 2G GSM | 3G UMTS | 4G LTE | 5G NR |
|---|---|---|---|---|
| **Base Station** | BTS | NodeB | eNodeB (eNB) | gNodeB (gNB) |
| **Controller** | BSC | RNC | ❌ (merged into eNB) | ❌ |
| **RAN Group** | BSS | UTRAN | E-UTRAN | NG-RAN |
| **Mobility/Session** | MSC | MSC + SGSN | MME | AMF + SMF |
| **User Plane GW** | ❌ | GGSN | S-GW + P-GW | UPF |
| **Subscriber DB** | HLR | HLR | HSS | UDM |
| **Auth Centre** | AuC | AuC | HSS (integrated) | AUSF |
| **Policy** | ❌ | ❌ | PCRF | PCF |

---

## Interface Names

| Interface | Between | Generation |
|---|---|---|
| Abis | BTS ↔ BSC | 2G |
| A | BSC ↔ MSC (voice) | 2G |
| Gb | BSC ↔ SGSN (data) | 2G/GPRS |
| Iub | NodeB ↔ RNC | 3G |
| Iur | RNC ↔ RNC | 3G |
| Iu-cs | RNC ↔ MSC | 3G voice |
| Iu-ps | RNC ↔ SGSN | 3G data |
| S1-MME | eNB ↔ MME | 4G control |
| S1-U | eNB ↔ S-GW | 4G user data |
| **X2** | eNB ↔ eNB | 4G HO coordination |
| N1 | UE ↔ AMF | 5G NAS |
| N2 | gNB ↔ AMF | 5G control |
| N3 | gNB ↔ UPF | 5G user data |

---

## Handover Types Summary

| Generation | Handover Type | Notes |
|---|---|---|
| 2G GSM | Hard (always) | 4 types: intra-BTS, inter-BTS/BSC, inter-BSC, inter-MSC |
| 3G UMTS | Hard + **Soft + Softer** | Soft = 2 NodeBs simultaneously; Softer = 2 sectors same NodeB |
| 4G LTE | Hard (X2-based) | Fast X2 HO; data buffered at S-GW; no dropped packets |
| 5G NR | Hard (Xn-based) | Similar to X2; beam-aware handover added |

---

## KPI Quick Reference

| KPI | Good | Warning | Critical | Measures |
|---|---|---|---|---|
| **RSRP** | > -80 dBm | -80 to -100 | < -110 dBm | Coverage / Signal strength |
| **RSRQ** | > -10 dB | -10 to -15 | < -15 dB | Quality (interference-aware) |
| **SINR** | > 20 dB | 0–10 dB | < 0 dB | Link quality → MCS selection |
| **CSSR** | > 99% | < 98% | < 95% | Session setup success |
| **CDR** | < 0.5% | > 1% | > 2% | Call drop rate |
| **HOSR** | > 98% | < 95% | — | Handover success |
| **PRB Util** | < 70% | 70–85% | > 85% | Capacity/congestion |

---

## Modulation and Spectral Efficiency

| Modulation | Bits/Symbol | Required SINR (approx) | Used Where |
|---|---|---|---|
| BPSK | 1 | ~0 dB | Control channels |
| QPSK | 2 | ~4 dB | Poor coverage, control |
| 8PSK | 3 | ~8 dB | EDGE only |
| 16-QAM | 4 | ~12 dB | Mid-range |
| 64-QAM | 6 | ~20 dB | Good link |
| 256-QAM | 8 | ~25 dB | Excellent link (LTE-A, 5G) |

---

## LTE Resource Grid Quick Facts

| Item | Value |
|---|---|
| Subcarrier spacing | 15 kHz |
| 1 Resource Block | 12 subcarriers × 0.5 ms slot |
| 1 Subframe | 1 ms (2 slots) = smallest scheduling unit |
| 1 Frame | 10 ms = 10 subframes |
| RBs in 20 MHz | 100 |
| RBs in 10 MHz | 50 |
| RBs in 5 MHz | 25 |

---

## 5G Use Cases at a Glance

| Use Case | Target Latency | Target Speed | Examples |
|---|---|---|---|
| eMBB | ~4 ms | 20 Gbps peak | Video, AR/VR |
| URLLC | **< 1 ms** | Moderate | Autonomous vehicles, surgery |
| mMTC | Relaxed | Very low | IoT sensors, smart city |

---

## Key Full Forms — Learn These Cold

| Acronym | Full Form |
|---|---|
| RAN | Radio Access Network |
| UE | User Equipment |
| BTS | Base Transceiver Station |
| BSC | Base Station Controller |
| MSC | Mobile Switching Centre |
| HLR | Home Location Register |
| VLR | Visitor Location Register |
| AuC | Authentication Centre |
| EIR | Equipment Identity Register |
| SGSN | Serving GPRS Support Node |
| GGSN | Gateway GPRS Support Node |
| RNC | Radio Network Controller |
| WCDMA | Wideband Code Division Multiple Access |
| HSDPA | High Speed Downlink Packet Access |
| HSUPA | High Speed Uplink Packet Access |
| eNB | Evolved NodeB (eNodeB) |
| gNB | Next-Gen NodeB (gNodeB) |
| EPC | Evolved Packet Core |
| MME | Mobility Management Entity |
| S-GW | Serving Gateway |
| P-GW | PDN Gateway |
| HSS | Home Subscriber Server |
| PCRF | Policy and Charging Rules Function |
| AMF | Access and Mobility Management Function |
| SMF | Session Management Function |
| UPF | User Plane Function |
| OFDMA | Orthogonal Frequency Division Multiple Access |
| SC-FDMA | Single Carrier FDMA |
| MIMO | Multiple Input Multiple Output |
| RSRP | Reference Signal Received Power |
| RSRQ | Reference Signal Received Quality |
| SINR | Signal to Interference and Noise Ratio |
| CSSR | Call Setup Success Rate |
| CDR | Call Drop Rate |
| HOSR | Handover Success Rate |
| PRB | Physical Resource Block |
| SON | Self-Organizing Network |
| ANR | Automatic Neighbour Relation |
| MLB | Mobility Load Balancing |
| MRO | Mobility Robustness Optimization |
| CCO | Coverage and Capacity Optimization |
| HARQ | Hybrid Automatic Repeat reQuest |
| AMC | Adaptive Modulation and Coding |
| VoLTE | Voice over LTE |
| VoNR | Voice over New Radio |
| IMS | IP Multimedia Subsystem |
| SRVCC | Single Radio Voice Call Continuity |
| NSA | Non-Standalone (5G) |
| SA | Standalone (5G) |
| NATS | National Apprenticeship Training Scheme |
| GSD | Global Service Delivery |
| NDO | Network Design and Optimisation |
| SLA | Service Level Agreement |

---

## One-Liners for Interview

- **"Why SC-FDMA on LTE uplink?"** → Lower PAPR than OFDMA → handset power amplifier more efficient → better battery life
- **"Difference between RSRP and RSRQ?"** → RSRP = signal strength; RSRQ = quality (factors in interference)
- **"Why soft handover only in 3G?"** → CDMA allows simultaneous links on same frequency; TDMA/OFDMA don't
- **"What does an MME do?"** → Control plane for LTE EPC: authentication, NAS signaling, paging, handover coordination — no user data flows through it
- **"What is SON?"** → Self-Organizing Network: self-configuration, self-optimization, self-healing of network parameters without manual intervention
- **"NSA vs SA in 5G?"** → NSA uses LTE core (EPC); SA uses 5GC — SA supports full 5G features like network slicing and URLLC
- **"Why does 5G use flexible subcarrier spacing?"** → Different use cases need different slot durations: eMBB works fine with 15/30 kHz; URLLC needs 120 kHz (very short slot = low latency); mMTC needs efficiency at lower SCS
- **"What is a link budget?"** → Calculation of maximum path loss that still allows reliable communication → determines maximum cell range

---

> [← Back to README](./README.md)

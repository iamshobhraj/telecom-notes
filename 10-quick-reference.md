# 10 — Quick Reference Cheat Sheet

> **Links:** [← KPIs & Optimization](./09-KPIs-optimization.md) | [README](./README.md)

> Use this for final revision before your interview. All important tables, comparisons, and one-liners in one place.

---

## Generation Overview

| Gen | Year | Speed | Latency | Multiple Access | Key Technology / Notes |
|---|---|---|---|---|---|
| 1G | 1979 | Analogue | — | FDMA | Analogue voice |
| **2G GSM** | 1991 | 9.6 kbps | ~500 ms | TDMA + FDMA | Digital voice, SMS, circuit-switched |
| **2.5G GPRS** | 2000 | 172 kbps | ~300 ms | TDMA + FDMA | Introduced packet data (always-on) |
| **2.75G EDGE** | 2003 | 384 kbps | <100 ms | TDMA + FDMA | 8PSK modulation introduced |
| **2G CDMA** | 1995 | 115 kbps | — | CDMA | IS-95 / cdmaOne (Qualcomm alternative to GSM) |
| **3G UMTS** | 2003 | 2 Mbps | 150 ms | CDMA (WCDMA) | 5 MHz wideband CDMA, soft handover |
| **3.5G HSPA** | 2005 | 14.4 Mbps DL | 100 ms | CDMA | HSDPA, shared channel, AMC, HARQ |
| **3G HSPA+** | 2008 | 28–84 Mbps | 50 ms | CDMA | MIMO, 64-QAM, Dual-Carrier |
| **4G LTE** | 2009 | 100 Mbps | 10 ms | OFDMA / SC-FDMA | All-IP, flat architecture (no RNC) |
| **4G LTE-A** | 2014 | 1 Gbps | <5 ms | OFDMA / SC-FDMA | Carrier aggregation, CoMP, true 4G |
| **5G NR** | 2018 | 20 Gbps | <4 ms | OFDMA (flexible) | Massive MIMO, slicing, mmWave, SBA |
| **6G** | ~2030 | TBD | <1 μs | TBD | THz, full duplex, RIS, AI-native |

---

## Node Name Evolution

| Function | 2G GSM | 3G UMTS | 4G LTE | 5G NR |
|---|---|---|---|---|
| **Base Station** | BTS | NodeB | eNodeB (eNB) | gNodeB (gNB) |
| **Controller** | BSC | RNC | ❌ (merged to eNB) | ❌ |
| **RAN Group** | BSS | UTRAN | E-UTRAN | NG-RAN |
| **Mobility/Auth** | MSC | MSC + SGSN | MME | AMF |
| **User Plane GW** | ❌ | GGSN | S-GW + P-GW | UPF |
| **Subscriber DB** | HLR | HLR | HSS | UDM |
| **Auth Centre** | AuC | AuC | HSS (integrated) | AUSF |
| **Policy** | ❌ | ❌ | PCRF | PCF |

---

## Key Interfaces

| Interface | Between Nodes | Generation |
|---|---|---|
| **Abis** | BTS ↔ BSC | 2G |
| **A** | BSC ↔ MSC (voice) | 2G |
| **Gb** | BSC ↔ SGSN (data) | 2G/GPRS |
| **Iub** | NodeB ↔ RNC | 3G |
| **Iur** | RNC ↔ RNC | 3G (for soft handover) |
| **Iu-cs / Iu-ps** | RNC ↔ MSC / SGSN | 3G (circuit / packet) |
| **S1-MME** | eNB ↔ MME | 4G (control plane) |
| **S1-U** | eNB ↔ S-GW | 4G (user plane) |
| **X2** | eNB ↔ eNB | 4G (direct handover coordination) |
| **N2 / N3** | gNB ↔ AMF / UPF | 5G (control / user) |
| **Xn** | gNB ↔ gNB | 5G (direct coordination) |

---

## Handover Types Summary

| Generation | Handover Types | Notes |
|---|---|---|
| **2G GSM** | Hard (always) | 4 types: intra-BTS, inter-BTS/BSC, inter-BSC, inter-MSC. Break before make. |
| **3G UMTS** | Hard + **Soft + Softer** | Soft = UE connected to 2 NodeBs simultaneously. Softer = 2 sectors same NodeB. Make before break. |
| **4G LTE** | Hard (X2-based) | Very fast X2 HO. Data is buffered at S-GW and forwarded via X2 to prevent dropped packets. |
| **5G NR** | Hard (Xn-based) | Beam-aware handover. UE switches between fine beams rather than just entire cells. |

---

## KPI Quick Reference

| KPI | Excellent | Acceptable | Critical | What it Measures |
|---|---|---|---|---|
| **RSRP** | > -80 dBm | -80 to -100 dBm | < -110 dBm | Signal strength / Coverage |
| **RSRQ** | > -10 dB | -10 to -15 dB | < -15 dB | Quality (factors in interference) |
| **SINR** | > 20 dB | 0–10 dB | < 0 dB | Link quality (determines MCS) |
| **CSSR** | > 99% | 98–99% | < 95% | Session setup success |
| **CDR** | < 0.5% | 0.5–1% | > 2% | Call drop rate |
| **PRB Util** | < 70% | 70–85% | > 85% | Cell capacity / Congestion |

---

## Modulation and Required SINR

| Modulation | Bits/Symbol | Required SINR | Used Where | Concept |
|---|---|---|---|---|
| **BPSK** | 1 bit | ~0 dB | Control channels | Robust, very slow |
| **QPSK** | 2 bits | ~4 dB | Cell edge, control | Basic data |
| **16-QAM** | 4 bits | ~12 dB | Mid-cell | Fast data |
| **64-QAM** | 6 bits | ~20 dB | Close to tower | Very fast data |
| **256-QAM** | 8 bits | ~25 dB | Excellent link (LTE-A, 5G) | Ultra fast data |

---

## 5G Use Cases (The Triangle)

| Use Case | Target Latency | Target Speed | Examples |
|---|---|---|---|
| **eMBB** (Enhanced Mobile Broadband) | ~4 ms | 20 Gbps peak | 4K/8K Video, AR/VR, Fixed Wireless |
| **URLLC** (Ultra-Reliable Low-Latency) | **< 1 ms** | Moderate | Autonomous vehicles, remote surgery, factory robotics |
| **mMTC** (Massive Machine-Type Comms) | Relaxed | Very low | IoT sensors, smart city, smart agriculture (1M devices/km²) |

---

## LTE Resource Grid Quick Facts

| Item | Value |
|---|---|
| **Subcarrier spacing** | 15 kHz |
| **1 Resource Element (RE)** | 1 subcarrier × 1 OFDM symbol |
| **1 Resource Block (RB)** | 12 subcarriers × 0.5 ms slot (84 REs total) |
| **1 Subframe** | 1 ms (2 slots) = smallest scheduling unit (TTI) |
| **1 Frame** | 10 ms = 10 subframes |
| **RBs in 20 MHz bandwidth** | 100 RBs |

---

## 🎯 High-Yield One-Liners for Interview

- **"Why SC-FDMA on LTE uplink?"** → OFDMA has a high PAPR (Peak-to-Average Power Ratio) which drains battery. SC-FDMA lowers PAPR, making the handset power amplifier more efficient and saving battery life.
- **"Difference between RSRP and RSRQ?"** → RSRP is pure signal strength. RSRQ is signal quality, which factors in interference from neighboring cells. You can have good RSRP but terrible RSRQ.
- **"Why is soft handover only in 3G?"** → CDMA allows a UE to be connected to two cells simultaneously on the *same frequency* using different spreading codes. TDMA (2G) and OFDMA (4G) cannot do this because slots/subcarriers would clash.
- **"What does an MME do?"** → It is the "brain" of the 4G core. It handles authentication, NAS signaling, paging, and handover coordination. **No user data flows through the MME.**
- **"What is SON?"** → Self-Organizing Network. It automates NDO tasks like self-configuration (adding neighbors via ANR), self-optimization (load balancing via MLB), and self-healing.
- **"NSA vs SA in 5G?"** → NSA (Non-Standalone) uses 5G radios bolted onto a 4G EPC core for a quick rollout. SA (Standalone) uses a proper 5G Core (5GC), enabling advanced features like network slicing and URLLC.
- **"Why does 5G use flexible subcarrier spacing (numerology)?"** → Different use cases need different timings. eMBB works fine with 15 kHz. URLLC needs 120 kHz because wider subcarriers mean shorter time slots, enabling sub-millisecond latency.
- **"CDMA vs OFDMA?"** → CDMA separates users by giving them unique mathematical codes on the same wide frequency band. OFDMA separates users by assigning them non-overlapping, orthogonal narrow frequency subcarriers.
- **"What is Carrier Aggregation?"** → Combining multiple fragmented frequency bands (Component Carriers) into one virtual wideband channel to achieve gigabit speeds in LTE-A.
- **"What is CoMP?"** → Coordinated Multipoint. Multiple base stations coordinate their transmissions to serve a cell-edge user simultaneously, turning interference into useful signal.

---

## Key Full Forms — Learn These Cold

| Acronym | Full Form |
|---|---|
| **UE / MS** | User Equipment / Mobile Station |
| **BTS / eNB / gNB** | Base Transceiver Station / eNodeB / gNodeB |
| **RAN / EPC / 5GC**| Radio Access Network / Evolved Packet Core / 5G Core |
| **MME / AMF** | Mobility Management Entity / Access & Mobility Mgmt Function |
| **S-GW / P-GW** | Serving Gateway / PDN Gateway |
| **UPF / SMF** | User Plane Function / Session Management Function |
| **WCDMA / OFDMA** | Wideband CDMA / Orthogonal Frequency Division Multiple Access |
| **HSDPA / HSUPA** | High Speed Downlink/Uplink Packet Access |
| **MIMO** | Multiple Input Multiple Output |
| **RSRP / RSRQ** | Reference Signal Received Power / Quality |
| **SINR** | Signal to Interference and Noise Ratio |
| **CSSR / CDR** | Call Setup Success Rate / Call Drop Rate |
| **PRB** | Physical Resource Block |
| **SON / ANR** | Self-Organizing Network / Automatic Neighbour Relation |
| **HARQ / AMC** | Hybrid Automatic Repeat reQuest / Adaptive Modulation & Coding |
| **VoLTE / VoNR** | Voice over LTE / Voice over New Radio |
| **NSA / SA** | Non-Standalone / Standalone (5G deployments) |
| **NDO** | Network Design and Optimisation |

---

> [← Back to README](./README.md)

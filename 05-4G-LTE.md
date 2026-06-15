# 05 — 4G LTE

> **Links:** [← 3G HSPA](./04-3G-HSPA.md) | [README](./README.md) | [LTE Advanced →](./06-4G-LTE-Advanced.md)

---

## Overview

| Parameter | Value |
|---|---|
| Full Name | Long Term Evolution |
| Launched | ~2009 (basic); ~2014 LTE-Advanced |
| Standard | 3GPP Release 8 (basic LTE) |
| Multiple Access | OFDMA (DL) / SC-FDMA (UL) |
| Max DL Speed | 100 Mbps (SISO), 172 Mbps (2×2 MIMO), 326 Mbps (4×4 MIMO) |
| Max UL Speed | 50 Mbps |
| Latency | ~10 ms (idle to connected <100 ms) |
| Channel BW Options | 1.4, 3, 5, 10, 15, **20 MHz** |
| Duplex | FDD and TDD |
| Data Type | **All packet switched** — no circuit switched even for voice |
| IP Version | IPv4 + IPv6 |

**Key shift:** LTE is the first generation with **no circuit switched domain at all**. Voice is handled as VoIP (VoLTE).

---

## Architecture — Evolved Packet System (EPS)

```
UE → eNodeB (eNB) → EPC → Internet
                      |
               MME | S-GW | P-GW | HSS | PCRF
```

### E-UTRAN (Radio Access Network)

| Node | Full Name | Role |
|---|---|---|
| **eNodeB (eNB)** | Evolved NodeB | Air interface; scheduling (AMC); RRC; HARQ; handover decisions; no separate controller needed |

**Key architectural change:** eNodeB is "intelligent" — no separate BSC/RNC needed. eNodeBs connect directly to each other via **X2 interface** for handover coordination.

eNodeB ↔ EPC: **S1 interface** (S1-MME for control plane, S1-U for user plane)

### EPC — Evolved Packet Core

| Node | Full Name | Role |
|---|---|---|
| **MME** | Mobility Management Entity | NAS signaling; UE authentication (with HSS); paging; handover coordination; tracking area management |
| **S-GW** | Serving Gateway | User plane anchor point; routes data during intra-LTE handovers; local packet buffering for paging |
| **P-GW** | PDN Gateway | Connects to internet and other PDNs; IP address allocation; policy enforcement; DPI |
| **HSS** | Home Subscriber Server | Evolved HLR; master subscriber database; authentication vectors |
| **PCRF** | Policy and Charging Rules Function | Decides QoS policies; controls P-GW/S-GW for each user session |

### What Handles What

| Function | Node |
|---|---|
| Authentication | MME + HSS |
| Mobility (control) | MME |
| Data routing (user plane) | S-GW → P-GW |
| IP address assignment | P-GW |
| Policy (QoS, charging) | PCRF |
| Radio scheduling | eNodeB |

---

## Air Interface — OFDM Basics

### OFDM Concept
- Data split across **many narrow subcarriers** (15 kHz spacing)
- Each subcarrier carries a low-rate data stream using QPSK or QAM
- Subcarriers are **orthogonal** — no mutual interference despite close spacing
- **Cyclic Prefix (CP)** added before each symbol → eliminates Inter-Symbol Interference (ISI) from multipath
- CP acts as a guard interval; chosen to be longer than max expected delay spread

### Why OFDM for LTE?
- Resilient to multipath (CP handles it)
- Simple frequency-domain equalization (one tap per subcarrier)
- Flexible resource allocation (assign subcarriers to different users)
- Enables MIMO efficiently

---

## OFDMA vs SC-FDMA

| | OFDMA (Downlink) | SC-FDMA (Uplink) |
|---|---|---|
| Multiple users | Different subcarrier sets per user | Same — but single carrier |
| PAPR (Peak to Average Power Ratio) | **High** | **Low** |
| Why it matters | Base station can handle high PAPR | UE battery life — power amplifier efficiency |
| Complexity | Same | Slightly more complex equalization |
| Data rate | Same for both | Same |

**The single reason SC-FDMA is used on uplink:** Lower PAPR → handset transmitter can operate more efficiently → better battery life.

---

## Resource Grid

| Unit | Definition |
|---|---|
| **Subcarrier** | 15 kHz wide (standard) |
| **Slot** | 0.5 ms |
| **Subframe** | 1 ms = 2 slots (smallest schedulable unit for data) |
| **Frame** | 10 ms = 10 subframes |
| **Resource Block (RB)** | 12 subcarriers × 1 slot (0.5 ms) = 84 resource elements |
| **Resource Element (RE)** | 1 subcarrier × 1 OFDM symbol |

**Scheduler allocates Resource Blocks to UEs** every subframe (1 ms). This is how multiple users share the channel simultaneously.

### Bandwidth vs Number of Resource Blocks

| Channel BW | Number of RBs |
|---|---|
| 1.4 MHz | 6 |
| 3 MHz | 15 |
| 5 MHz | 25 |
| 10 MHz | 50 |
| 15 MHz | 75 |
| 20 MHz | 100 |

---

## MIMO in LTE

| Mode | Description | Benefit |
|---|---|---|
| **SISO** | 1 Tx, 1 Rx | Baseline |
| **Receive Diversity** | 1 Tx, 2 Rx | Better SNR, no rate increase |
| **Transmit Diversity (SFBC)** | 2 Tx, 1 Rx | Reliability at cell edge |
| **Spatial Multiplexing** | 2×2, 4×2, 4×4 | Multiple independent streams → higher throughput |
| **Beamforming** | Multiple Tx antennas focused toward UE | Better SINR |
| **MU-MIMO** | Multiple UEs served simultaneously on same resource | System-level capacity gain |

**Peak data rates:**
- SISO: 100 Mbps
- 2×2 MIMO: 172 Mbps
- 4×4 MIMO: 326 Mbps

---

## Protocol Stack

| Layer | Protocol | Function |
|---|---|---|
| 3 | **RRC** | Radio Resource Control — connection setup/release, mobility, measurement configuration, system info broadcast |
| 3 | **NAS** | Non-Access Stratum — between UE and MME; authentication, session management |
| 2 | **PDCP** | Packet Data Convergence Protocol — IP header compression (ROHC), ciphering, integrity protection |
| 2 | **RLC** | Radio Link Control — segmentation/reassembly, ARQ (retransmission) |
| 2 | **MAC** | Medium Access Control — scheduling, HARQ, multiplexing |
| 1 | **PHY** | Physical layer — modulation, coding, OFDM |

**Remember the order:** RRC → PDCP → RLC → MAC → PHY (top to bottom)

---

## Channels

### Downlink Physical Channels

| Channel | Purpose |
|---|---|
| **PDSCH** | Physical Downlink Shared Channel — user data; adaptive MCS |
| **PDCCH** | Physical Downlink Control Channel — scheduling assignments (tells UE which RBs are for it) |
| **PBCH** | Physical Broadcast Channel — MIB (Master Information Block); cell-level system info |
| **PCFICH** | Physical Control Format Indicator Channel — tells UE how many symbols are used for PDCCH |
| **PHICH** | Physical HARQ Indicator Channel — ACK/NACK for uplink HARQ |

### Uplink Physical Channels

| Channel | Purpose |
|---|---|
| **PUSCH** | Physical Uplink Shared Channel — user data uplink |
| **PUCCH** | Physical Uplink Control Channel — CQI, HARQ ACK/NACK, scheduling requests |
| **PRACH** | Physical Random Access Channel — initial access, synchronization |

### Reference Signals

| Signal | Purpose |
|---|---|
| **CRS (Cell-specific Reference Signal)** | Channel estimation; RSRP measurement |
| **DM-RS (Demodulation Reference Signal)** | Per-UE reference for data demodulation |
| **SRS (Sounding Reference Signal)** | Uplink channel quality estimation |

---

## LTE Handover

### X2-based Handover (intra-LTE, most common)
1. Source eNB detects handover need (based on UE measurement reports)
2. Source eNB sends **Handover Request** to target eNB via X2
3. Target eNB admits or rejects → sends **Handover Request ACK**
4. Source eNB sends **RRC Connection Reconfiguration** to UE
5. UE detaches from source → attaches to target → sends **RRC Connection Reconfiguration Complete**
6. Data path switched: MME/S-GW updated
7. Source eNB resources released

**No circuit-switched break:** Data packets buffered at S-GW during handover → no dropped calls (unlike 2G hard HO)

---

## VoLTE — Voice over LTE

### Why Needed?
- LTE has no circuit switched voice
- Before VoLTE: LTE data + fallback to 3G/2G for calls (CSFB — Circuit-Switched Fallback)
- VoLTE allows voice as a **VoIP application** carried over the LTE data network

### IMS Architecture for VoLTE

| Node | Role |
|---|---|
| **P-CSCF** | Proxy Call Session Control Function — first IMS contact point for UE |
| **S-CSCF** | Serving CSCF — handles call routing, session control |
| **I-CSCF** | Interrogating CSCF — entry point for incoming calls |
| **AS** | Application Server — handles voice as an application |
| **HSS (IMS)** | Subscriber database for IMS |

### Codec
- **AMR (Adaptive Multi-Rate)** — same codec as 2G/3G voice for interoperability
- AMR-WB (Wideband) for HD voice quality

### SRVCC — Single Radio Voice Call Continuity
- When UE moves out of LTE coverage mid-call → needs to hand to 3G/2G circuit-switched
- SRVCC transfers the IMS session to legacy CS network
- Only **one active radio** in UE required (battery efficient)
- Requires software upgrade to MSC (legacy) + IMS + EPC
- Process: IMS CSCF sends simultaneous commands to LTE and legacy network → coordinated switch

---

## LTE vs UMTS Summary

| | 3G UMTS | 4G LTE |
|---|---|---|
| Radio interface | WCDMA (CDMA) | OFDMA / SC-FDMA |
| Base station | NodeB + RNC | eNodeB (standalone) |
| Max DL speed | 2 Mbps (14 with HSDPA) | 100 Mbps (326 with MIMO) |
| Latency | 150 ms | ~10 ms |
| Voice | Circuit switched | Packet (VoLTE) |
| Architecture | Hierarchical (RNC) | Flat (X2 between eNBs) |
| Core | UMTS CN (SGSN/GGSN) | EPC (MME/S-GW/P-GW) |

---

> **Next:** [06 — LTE Advanced →](./06-4G-LTE-Advanced.md)

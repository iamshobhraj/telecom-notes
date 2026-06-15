# 00 — Cellular Fundamentals

> **Links:** [README](./README.md) | [2G GSM →](./01-2G-GSM.md)

---

## What is Cellular Communications?

- Large number of **base stations**, each covering a small area called a **cell**
- Each base station serves a limited number of users → system scales to millions
- Core concept: **frequency reuse** — same frequencies used in non-adjacent cells without interference
- Proposed conceptually in the 1940s; deployed from 1980s onward

---

## The Cell Concept

- Coverage area divided into hexagonal cells (theoretical model; real-world is irregular)
- Each cell has a **base station** at its centre (or edge)
- Adjacent cells use **different frequency groups** to avoid interference
- **Cluster** = group of cells using all available frequencies once
  - Cluster size N = 3, 4, 7, 12 (common values)
  - Larger N → more separation → less interference but **less capacity**
  - Smaller N → more reuse → more **capacity** but higher interference risk

### Frequency Reuse Formula

```
D/R = √(3N)
```
- D = distance between co-channel cells
- R = cell radius
- N = cluster size

---

## Network Elements

### 1. User Equipment (UE)
- The mobile device (phone, tablet, IoT sensor)
- Connects to the network via the radio interface
- Contains SIM/USIM to identify the subscriber

### 2. Radio Access Network (RAN)
- "Edge" of the network — connects UE to the core
- Includes: base station + controller (varies by generation)
- **Open RAN (O-RAN):** vendor-neutral RAN; allows different vendors' equipment to interoperate

### 3. Core Network
- Brain of the system
- Functions: routing, authentication, billing, subscriber management, handover coordination, connection to the internet/PSTN

---

## Multiple Access Technologies

| Technology | Full Form | How it Works | Used In |
|---|---|---|---|
| FDMA | Frequency Division Multiple Access | Each user gets a unique frequency | 1G |
| TDMA | Time Division Multiple Access | Users share a frequency, each gets a time slot | 2G GSM |
| CDMA | Code Division Multiple Access | All users share same frequency, separated by unique codes | 3G |
| OFDMA | Orthogonal Frequency Division Multiple Access | Users share subcarriers across time and frequency | 4G DL, 5G |
| SC-FDMA | Single Carrier FDMA | Like OFDMA but single carrier — lower PAPR | 4G UL |

---

## Modulation Schemes

| Scheme | Bits per Symbol | Use Case |
|---|---|---|
| BPSK | 1 | Robust, low SNR environments |
| QPSK | 2 | Control channels, poor link quality |
| 8PSK | 3 | EDGE uplink |
| 16-QAM | 4 | Mid-range link quality |
| 64-QAM | 6 | Good link quality |
| 256-QAM | 8 | Excellent link quality (close to base station) |

**Adaptive Modulation:** System automatically picks the modulation order based on current SINR. Higher modulation = faster data but needs better signal.

---

## Duplex Methods

| Type | How | Used In |
|---|---|---|
| FDD (Frequency Division Duplex) | UL and DL on separate frequencies | GSM, UMTS, LTE (most common) |
| TDD (Time Division Duplex) | UL and DL alternate in time on same frequency | Some UMTS, LTE-TDD, 5G NR |

**FDD advantage:** Simultaneous TX/RX, no guard time needed  
**TDD advantage:** Can adjust UL/DL ratio dynamically — useful when DL >> UL (internet browsing)

---

## Handover (Handoff) Basics

- Process of transferring an active call/connection from one cell to another as UE moves
- Triggered by: signal strength drop, interference, cell load balancing
- **Hard handover:** Break old link, then establish new one (2G, 4G, 5G)
- **Soft handover:** UE connected to two base stations simultaneously — only possible in CDMA (3G)
- **Inter-RAT handover:** Switching between different technologies (e.g., 4G → 2G fallback)

---

## Cell Planning Concepts

### Coverage vs Capacity Tradeoff
- **Larger cells** → more coverage, fewer base stations, less capacity per area
- **Smaller cells** → more capacity, more base stations needed, better for dense urban areas

### Cell Splitting
- Divide one large cell into multiple smaller cells
- Increases capacity in high-demand areas
- Requires more base stations

### Sectorization
- Single cell divided into 3 sectors (each 120°) using directional antennas
- Triples capacity vs omnidirectional antenna
- Standard for all modern base stations

### Antenna Tilt
- **Mechanical tilt:** Physically rotate antenna downward
- **Electrical tilt:** Adjust phase of antenna elements — changes radiation pattern electronically
- **Purpose:** Reduce interference to neighboring cells, improve SINR at cell edge
- More downtilt → smaller coverage footprint, less co-channel interference

---

## Link Budget

A calculation to determine maximum allowable path loss and therefore maximum cell range.

```
Link Budget = Tx Power (dBm) 
            + Tx Antenna Gain (dBi)
            - Feeder Loss (dB)
            - Path Loss (dB)
            + Rx Antenna Gain (dBi)
            - Rx Noise Figure (dB)
            ≥ Rx Sensitivity (dBm)
```

### Path Loss Models

| Model | Best For |
|---|---|
| Free Space | Open, line-of-sight |
| Okumura-Hata | Urban/suburban, 150–1500 MHz |
| COST-231 | 1500–2000 MHz (extended Hata) |

---

## Mobile Backhaul

Connection between the base station and the core network (the "backhaul" link).

| Technology | Notes |
|---|---|
| E1/T1 circuits | Legacy, low capacity |
| Carrier Ethernet | High capacity, standard protocol, widely used |
| Microwave | Ideal for remote sites, no fibre/copper needed |
| Fibre | Highest capacity, most reliable |

### Approximate Backhaul Requirements

| System | Required Bandwidth |
|---|---|
| GSM | ~1.3 Mbps |
| EDGE | ~6 Mbps |
| HSPA | ~21 Mbps |
| LTE 5 MHz | ~40 Mbps |
| LTE 10 MHz | ~80 Mbps |

---

> **Next:** [01 — 2G GSM →](./01-2G-GSM.md)

# 04 — 3G HSPA / HSPA+

> **Links:** [← 3G UMTS](./03-3G-UMTS.md) | [README](./README.md) | [4G LTE →](./05-4G-LTE.md)

---

## What Is HSPA?

- **HSPA = High Speed Packet Access** = HSDPA + HSUPA
- An evolution of 3G UMTS to dramatically increase data speeds
- **HSDPA:** High Speed Downlink Packet Access (DL enhancement)
- **HSUPA:** High Speed Uplink Packet Access (UL enhancement)
- Often deployed as a **software upgrade** to existing UMTS base stations
- Bridged the gap between basic 3G and 4G LTE

---

## Speed Evolution

| 3GPP Release | Technology | Max DL Speed | Max UL Speed |
|---|---|---|---|
| Rel 99 | UMTS (WCDMA) | 384 kbps | 128 kbps |
| **Rel 5** | **HSDPA** | **14.4 Mbps** | 384 kbps |
| **Rel 6** | **HSUPA** | 14.4 Mbps | **5.74 Mbps** |
| Rel 7 | HSPA+ (MIMO + 64-QAM) | 28 Mbps | 11 Mbps |
| Rel 8 | DC-HSDPA (dual carrier) | 42 Mbps | 11 Mbps |
| Rel 9 | DC-HSPA + MIMO | 84 Mbps | 23 Mbps |
| Rel 10 | 4-carrier HSPA | 168 Mbps | 23 Mbps |
| Rel 11 | 8-carrier + 4×4 MIMO | 336–672 Mbps | 70 Mbps |

---

## HSDPA — High Speed Downlink Packet Access (Rel 5)

### Key New Mechanisms (What Made It Faster)

**1. Higher Order Modulation**
- UMTS used QPSK (2 bits/symbol)
- HSDPA added **16-QAM** (4 bits/symbol) → 2× more bits per symbol on downlink

**2. Shorter Transmission Time Interval (TTI)**
- UMTS TTI: 10–80 ms
- HSDPA TTI: **2 ms**
- Shorter TTI → faster adaptation to channel changes → lower latency

**3. Adaptive Modulation and Coding (AMC)**
- Modulation (QPSK / 16-QAM) and coding rate selected based on current channel quality
- UE reports **CQI (Channel Quality Indicator)** to NodeB
- NodeB schedules accordingly — users with good channel get high MCS, cell-edge users get lower MCS

**4. Shared Channel (HS-DSCH)**
- New channel: **HS-DSCH (High Speed Downlink Shared Channel)**
- Shared dynamically among multiple users in each TTI
- Replaces dedicated channels for data → much more efficient

**5. Fast Node B Scheduling**
- Scheduling moved from RNC down to **NodeB** (closer to radio)
- Can react to channel variations in 2 ms instead of waiting for RNC
- Enables "proportional fair" scheduling — balances throughput and fairness

**6. HARQ — Hybrid Automatic Repeat Request**
- UE requests retransmission of failed blocks
- Combines retransmission with original → "Chase combining" or "incremental redundancy"
- HARQ handled in NodeB (not RNC) for speed
- Faster retransmission → reduces latency, improves robustness

---

## HSUPA — High Speed Uplink Packet Access (Rel 6)

Applies similar principles to the **uplink**:

| Feature | Detail |
|---|---|
| New channel | E-DCH (Enhanced Dedicated Channel) |
| Max speed | 5.74 Mbps |
| Modulation | QPSK (uplink only — handset power limitation) |
| Scheduling | NodeB-controlled (like HSDPA) |
| HARQ | Yes, in NodeB |
| TTI | 2 ms (10 ms also supported) |

**Why no 16-QAM in HSUPA original?** Handset battery life — higher order modulation requires more transmit power.

---

## HSPA+ — Further Enhancements (Rel 7 onward)

### Key Additions

**MIMO (Rel 7)**
- 2×2 MIMO on downlink → spatial multiplexing → up to 2× throughput
- Combined with 16-QAM: 28 Mbps peak DL

**64-QAM (Rel 7)**
- 6 bits/symbol on downlink (instead of 4 with 16-QAM)
- Requires very good SINR (close to NodeB)

**Dual Carrier HSDPA — DC-HSDPA (Rel 8)**
- UE receives data across **two 5 MHz carriers simultaneously**
- Doubles peak throughput
- Joint scheduling across carriers
- Particularly improves performance at cell edge (where MIMO isn't effective)

**Multi-carrier HSPA (Rel 10+)**
- Up to 4 carriers → 20 MHz effective bandwidth
- 168 Mbps peak (similar to basic LTE)

---

## HSDPA Channels

| Channel | Direction | Purpose |
|---|---|---|
| **HS-DSCH** | DL | High Speed Downlink Shared Channel — user data |
| **HS-SCCH** | DL | HS Shared Control Channel — tells UE which TTI is allocated to it |
| **HS-DPCCH** | UL | UE sends CQI and HARQ ACK/NACK feedback to NodeB |

---

## Comparison: HSPA vs Basic UMTS

| | 3G UMTS (Rel 99) | HSDPA (Rel 5) |
|---|---|---|
| DL max speed | 384 kbps | 14.4 Mbps |
| Modulation | QPSK | QPSK + 16-QAM |
| TTI | 10–80 ms | **2 ms** |
| Scheduling | RNC | **NodeB** |
| HARQ | No | **Yes** |
| Channel type | Dedicated | **Shared** |

---

## Why HSPA Matters for NDO

- HSPA networks still exist and require optimization
- CQI reporting mechanism is the direct predecessor of what's used in LTE
- AMC/HARQ/scheduling concepts carry over unchanged to LTE
- Understanding HSPA helps explain why cell-edge UEs always get lower throughput

---

> **Next:** [05 — 4G LTE →](./05-4G-LTE.md)

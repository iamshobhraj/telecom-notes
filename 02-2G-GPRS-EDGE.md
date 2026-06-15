# 02 — 2G GPRS & EDGE

> **Links:** [← 2G GSM](./01-2G-GSM.md) | [README](./README.md) | [3G UMTS →](./03-3G-UMTS.md)

---

## GPRS — General Packet Radio Service

### What It Is
- **First packet data capability** for GSM networks (2.5G)
- Evolved from circuit-switched GSM to **packet-switched** data
- Could be deployed as a **software upgrade** to existing GSM networks
- Introduced "always-on" connectivity concept
- First phones to use it: PDAs (Blackberry era — mobile email + basic web)

### Key Parameters

| Parameter | Value |
|---|---|
| Channel Bandwidth | 200 kHz (same as GSM) |
| Modulation | GMSK (same as GSM) |
| Max Theoretical Speed | 172 kbps (all 8 slots used) |
| Real-World Speed | 15–40 kbps typical |
| Data Type | Packet switched |

---

## Packet vs Circuit Switching

| | Circuit Switched (GSM) | Packet Switched (GPRS) |
|---|---|---|
| Channel allocation | Dedicated for duration of call | Shared between many users |
| Billing | Per minute of connection | Per MB of data transferred |
| Connectivity | Set up and torn down per call | "Always on" |
| Data type | Continuous stream | Bursts of packets |
| Efficiency | Poor for bursty data | High for internet-type traffic |

**Why packet switching is better for data:** Internet traffic is "bursty" — you request a page, download it, read it (no data flowing), then request next page. Circuit switching wastes capacity during the reading gaps.

---

## GPRS Network Architecture

GPRS adds **two new nodes** to the GSM core network. GSM voice still flows through the existing path.

```
UE → BTS → BSC → SGSN → GGSN → Internet
            ↓
           MSC (for voice, unchanged)
```

### New GPRS Nodes

| Node | Full Name | Role |
|---|---|---|
| **SGSN** | Serving GPRS Support Node | Packet-switched equivalent of MSC; tracks UE location; mobility management for data; authentication for data sessions |
| **GGSN** | Gateway GPRS Support Node | Gateway to the internet and other packet networks; IP address allocation; connects GPRS network to PDN (Packet Data Network) |

**Interface BSC ↔ SGSN:** Gb  
**Interface SGSN ↔ GGSN:** Gn (internal GPRS backbone)  
**Interface GGSN ↔ Internet:** Gi

---

## GPRS Mobile States

| State | Description |
|---|---|
| **Idle** | Mobile turned on, performs GPRS attach, registers with SGSN; monitors paging |
| **Standby** | Attached but no active data transfer; SGSN knows approximate location; mobile wakes periodically for paging |
| **Ready** | Active data transfer in progress; precise location known by SGSN |

**GPRS Attach:** Different from GSM location update — specifically registers UE for data services with SGSN.  
**PDP Context:** Virtual connection/tunnel established between UE and GGSN when data session starts — gives UE an IP address.

---

## GPRS Protocol Stack

| Layer | Protocol |
|---|---|
| 7 | Application |
| 6 | IP / X.25 |
| 5 | SNDCP (SubNetwork Dependent Convergence Protocol) |
| 4 | LLC (Logical Link Control) |
| 3 | RLC (Radio Link Control) |
| 2 | MAC (Medium Access Control) |
| 1 | GSM RF |

**SNDCP:** Header compression, multiplexing — transparent to BSS  
**LLC:** Error correction, retransmissions  
**RLC/MAC:** Radio resource allocation, access control

---

## EDGE — Enhanced Data rates for GSM Evolution

### What It Is
- Evolved GPRS to provide **higher data rates** (2.75G)
- Same infrastructure as GPRS with one key change: **new modulation (8PSK)**
- Often implemented as software/hardware upgrade to existing GPRS network
- Provided basic 3G-like data rates without full 3G deployment cost

### Key Parameters

| Parameter | Value |
|---|---|
| Multiple Access | FDMA / TDMA (same as GSM) |
| Duplex | FDD |
| Channel Spacing | 200 kHz |
| Modulation | **GMSK (same as GSM) + 8PSK (new)** |
| Max Data Rate | 384 kbps (ITU IMT-2000 pedestrian definition) |
| Theoretical Max | 473.6 kbps (all 8 slots, MCS-9) |
| Latency | <100 ms |

---

## 8PSK Modulation — The Key EDGE Innovation

| | GMSK (GSM/GPRS) | 8PSK (EDGE) |
|---|---|---|
| Bits per symbol | 1 | **3** |
| Data rate | Baseline | 3× higher |
| SNR required | Low | Higher |
| Resilience to noise | Better | Worse |

**Adaptive use:** When signal is poor → falls back to GMSK; when signal is good → uses 8PSK. This is the Modulation and Coding Scheme (MCS) system.

---

## EDGE Modulation and Coding Schemes (MCS)

9 MCS levels, each balancing error protection vs throughput:

| MCS | Modulation | Coding Rate | Data Rate/slot |
|---|---|---|---|
| MCS-1 | GMSK | 0.53 | 8.8 kbps |
| MCS-2 | GMSK | 0.66 | 11.2 kbps |
| MCS-3 | GMSK | 0.80 | 14.8 kbps |
| MCS-4 | GMSK | 1.00 | 17.6 kbps |
| MCS-5 | 8PSK | 0.37 | 22.4 kbps |
| MCS-6 | 8PSK | 0.49 | 29.6 kbps |
| MCS-7 | 8PSK | 0.76 | 44.8 kbps |
| MCS-8 | 8PSK | 0.92 | 54.4 kbps |
| **MCS-9** | 8PSK | 1.00 | **59.2 kbps** |

**MCS families (A, B, C):** Grouped so retransmissions can use a compatible scheme. E.g., if MCS-7 fails, retransmit as two MCS-5 blocks.

---

## EDGE Network Architecture

Same as GPRS — same SGSN and GGSN. Only change is at the **air interface** (8PSK modulation in the BTS) and **new EDGE-capable handsets** required.

```
UE (EDGE) → BTS (8PSK capable) → BSC → SGSN → GGSN → Internet
```

---

## Evolved EDGE (E-EDGE)

An optional further evolution — not widely deployed:
- **16-QAM and 32-QAM** modulation (4 and 5 bits/symbol)
- Receiver diversity (two receive chains)
- Simultaneous TX+RX (all 8 slots usable)
- Theoretical speeds ~1 Mbps
- **Not widely adopted** — operators moved to 3G UMTS instead

---

## 2G Evolution Summary

```
GSM (9.6 kbps, circuit switched)
  └─ GPRS (172 kbps, packet switched) — added SGSN + GGSN
      └─ EDGE (384 kbps, 8PSK modulation) — changed air interface
          └─ E-EDGE (1 Mbps, 16/32-QAM) — barely deployed
```

**Key takeaway:** GPRS + EDGE were stepping stones — both the core network nodes (SGSN, GGSN) and concepts (packet data, always-on) were reused and evolved into 3G UMTS.

---

> **Next:** [03 — 3G UMTS →](./03-3G-UMTS.md)

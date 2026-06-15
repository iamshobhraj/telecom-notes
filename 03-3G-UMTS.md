# 03 — 3G UMTS / WCDMA

> **Links:** [← GPRS & EDGE](./02-2G-GPRS-EDGE.md) | [README](./README.md) | [3G HSPA →](./04-3G-HSPA.md)

---

## Overview

| Parameter | Value |
|---|---|
| Full Name | Universal Mobile Telecommunications System |
| Standard Body | 3GPP (Third Generation Partnership Project) |
| Launched | ~2001 |
| Radio Interface | WCDMA (Wideband Code Division Multiple Access) |
| Channel Bandwidth | **5 MHz** (vs 1.25 MHz for CDMA2000 — hence "wideband") |
| Multiple Access | **CDMA** |
| Duplex | **FDD** (primary) and TDD |
| Max Data Rate | 2 Mbps (original), 14.4 Mbps with HSDPA |
| Simultaneous voice calls | 100+ on one 5 MHz carrier |
| Latency | ~150 ms |

**Key difference from 2G:** Completely new radio interface (CDMA instead of TDMA). Core network largely reused from GPRS.

---

## Architecture

```
UE → NodeB → RNC → MSC (voice, circuit switched)
                └─→ SGSN → GGSN → Internet (data, packet switched)
```

### RAN — UTRAN (UMTS Terrestrial Radio Access Network)

| Node | 2G Equivalent | Role |
|---|---|---|
| **NodeB** | BTS | Air interface radio; signal spreading/despreading |
| **RNC** | BSC | Radio resource management; handover control; handles Iub from multiple NodeBs |

**Interface NodeB ↔ RNC:** Iub  
**Interface RNC ↔ MSC:** Iu-cs (circuit switched voice)  
**Interface RNC ↔ SGSN:** Iu-ps (packet switched data)  
**Interface RNC ↔ RNC:** Iur (soft handover coordination between RNCs)

### Core Network
- **Same as GPRS core:** MSC, HLR, VLR, AuC, EIR (voice); SGSN, GGSN (data)
- **USIM:** Upgraded SIM — more secure authentication, supports new services

---

## CDMA — The Core Technology

### How CDMA Works
1. All users transmit on the **same frequency** simultaneously
2. Each user assigned a **unique spreading code** (orthogonal to others)
3. Transmitter multiplies data signal by spreading code → **spreads** the signal across wide bandwidth
4. Receiver knows the code → **despread** the signal → recovers original data
5. Other users' signals appear as low-level noise (below interference floor)

### Key Parameters

| Term | Definition |
|---|---|
| **Chip rate** | 3.84 Mcps (chips per second) in UMTS |
| **Chip** | Individual bit of the spreading code |
| **Spreading factor (SF)** | Ratio of chip rate to data rate; SF = 4 to 512 |
| **Processing gain** | 10 × log(SF) — how much the spreading improves SNR |

**Spreading factor tradeoff:**
- High SF → lower data rate, more spreading, better range
- Low SF → higher data rate, less spreading, needs better SNR

### Why CDMA Is Different from TDMA
| | TDMA (2G GSM) | CDMA (3G UMTS) |
|---|---|---|
| Separation method | Time slots | Unique codes |
| Frequency | Different per group | **Same for all users** |
| Capacity limit | Fixed (8 slots) | **Soft** — more users = slightly more noise |
| Interference | Adjacent slot/channel | Own-cell + other-cell interference |

---

## RAKE Receiver

### Problem It Solves
- Multipath propagation: signal arrives via multiple paths (reflections from buildings), causing delay spread and interference

### How It Works
1. Multiple "fingers" — each locks onto a different multipath component
2. Each finger despreads its respective path using the known code
3. Outputs from all fingers combined using **Maximum Ratio Combining (MRC)**
4. Result: multipath signals add constructively → **improves SNR**

**In 2G GSM:** Multipath is a problem → equalizer needed  
**In 3G CDMA:** Multipath is turned into an advantage → RAKE receiver exploits it

**Also used for soft handover:** UE uses RAKE fingers to simultaneously receive from two different NodeBs.

---

## Power Control

Critical in CDMA because all users share the same frequency:
- A strong signal from one UE "drowns out" weaker signals from others → "near-far problem"
- Solution: **tight power control** adjusts each UE's transmit power 1500 times/second

| Type | Direction | Rate | Purpose |
|---|---|---|---|
| **Inner loop (fast)** | UL + DL | 1500 Hz | Track fast fading |
| **Outer loop (slow)** | UL | ~10–100 Hz | Set SIR (signal to interference ratio) target |
| **Open loop** | UL | Initial | Set initial power before inner loop locks |

---

## UMTS Channels

### Physical Channels (downlink)

| Channel | Purpose |
|---|---|
| **CPICH** | Common Pilot Channel — reference signal for measurements (like BCCH in 2G) |
| **SCH** | Synchronization Channel — helps UE find and synchronize with a cell |
| **CCPCH** | Common Control Physical Channel — carries BCH (system info) |
| **DPCH** | Dedicated Physical Channel — carries user data + control |
| **AICH** | Acquisition Indicator Channel — acknowledges RACH attempts |

### Transport Channels (logical)

| Channel | Direction | Purpose |
|---|---|---|
| **BCH** | DL | Broadcast — system information |
| **PCH** | DL | Paging |
| **FACH** | DL | Forward access channel — small amounts of data |
| **RACH** | UL | Random access — UE requests connection |
| **DCH** | Both | Dedicated — user traffic |
| **DSCH** | DL | Downlink shared channel (HSDPA precursor) |

---

## FDD vs TDD in UMTS

| | FDD | TDD |
|---|---|---|
| UL/DL separation | Different frequencies | Different time slots |
| Guard time | No | Yes (small) |
| Paired spectrum | Required | Not required |
| UL/DL asymmetry | Fixed | **Adjustable** |
| Use case | Voice, symmetric data | IP data, small cells |
| Deployment | Widespread | Limited |

**TDD advantage for IP data:** Web browsing generates much more DL traffic than UL. TDD lets you allocate more slots to DL → better spectrum efficiency.

---

## UMTS Handover Types

### Hard Handover
- Old link **broken first**, then new link established
- Used when: moving to different frequency, FDD→TDD change, capacity shortage
- Risk: if new link fails to establish → dropped call

### Soft Handover ⭐ (unique to CDMA)
- UE **simultaneously connected to two NodeBs**
- Both must be on the **same frequency** (mandatory — UE has only one TX/RX chain)
- UL combining: best frames selected from both NodeBs (RNC compares)
- DL combining: UE RAKE receiver combines signals from both NodeBs
- ~5–10% of connections in soft HO at any time — uses more resources but more reliable

### Softer Handover
- Special case of soft HO: two sectors of the **same NodeB**
- Combining done inside the NodeB (simpler, more efficient)
- Triggered by: overlapping sectors, multipath reflections

### Soft vs Hard Summary

| | Hard | Soft | Softer |
|---|---|---|---|
| Break before make? | Yes | No | No |
| Multiple simultaneous links? | No | Yes (2 NodeBs) | Yes (2 sectors) |
| Where combining happens | N/A | RNC | NodeB |
| Frequency | Can change | Must be same | Must be same |

---

## Inter-RAT Handover (UMTS ↔ GSM)

Happens when:
- UMTS coverage ends (most common)
- UMTS congested but GSM has capacity

**UMTS → GSM:**
- **Compressed mode:** UMTS RNC creates gaps in TX/RX → UE scans GSM cells during gaps → selects best → hands over
- **Blind:** Network picks best GSM cell, UE switches without prior scanning

**GSM → UMTS:**
- Uses "neighbour list" maintained in GSM network
- Less common (GSM coverage is usually wider)
- Happens to improve data performance

---

## UMTS vs GSM Summary

| | 2G GSM | 3G UMTS |
|---|---|---|
| Radio interface | TDMA | CDMA (WCDMA) |
| Channel BW | 200 kHz | 5 MHz |
| Max data rate | 9.6 kbps | 2 Mbps (base) |
| Voice capacity | 8 slots/carrier | ~100 calls/5MHz |
| Soft handover | No | Yes |
| Core network | GSM core | Same GPRS core + USIM |
| Base station | BTS | NodeB |
| Controller | BSC | RNC |

---

> **Next:** [04 — 3G HSPA →](./04-3G-HSPA.md)

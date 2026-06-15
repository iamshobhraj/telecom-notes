# 06 — 4G LTE Advanced

> **Links:** [← 4G LTE](./05-4G-LTE.md) | [README](./README.md) | [5G NR →](./07-5G-NR.md)

---

## Overview

| Parameter | Value |
|---|---|
| 3GPP Release | **Release 10** |
| Initial Deployment | ~2014–2015 |
| IMT Standard | IMT-Advanced |
| Max DL Speed | **1 Gbps** |
| Max UL Speed | **500 Mbps** |
| Latency | **<5 ms** |
| Spectral Efficiency | 3× LTE Rel 8 |

LTE-A is backward compatible with LTE — an LTE device works on LTE-A network, just without the advanced features.

---

## Key Technologies

### 1. Carrier Aggregation (CA) ⭐ Most Important Feature

**Problem:** Operators have spectrum in multiple non-contiguous bands. Each fragment alone is too narrow for high speeds.

**Solution:** Aggregate multiple carriers (component carriers, CCs) as if they were a single wide channel.

**Rules:**
- Up to **5 component carriers** (CCs) per direction
- Each CC: up to 20 MHz
- Total: up to **100 MHz** aggregated
- CCs can be in the same band (**intra-band**) or different bands (**inter-band**)
- Can also be contiguous or non-contiguous

**Types of Carrier Aggregation:**

| Type | Description |
|---|---|
| **Intra-band Contiguous** | Multiple CCs in same band, adjacent frequencies |
| **Intra-band Non-contiguous** | Same band, but with gap between CCs |
| **Inter-band Non-contiguous** | CCs in completely different frequency bands |

**UE perspective:** Single UE sees one wideband channel — complexity hidden by the system.

---

### 2. Enhanced MIMO

| LTE (Rel 8) | LTE-A (Rel 10) |
|---|---|
| Max 4×4 MIMO | Up to **8×8 MIMO** |
| Max 4 spatial layers | Max **8 spatial layers** DL |
| 2×2 UL | Up to **4×4 UL** |

**More antennas → more spatial streams → higher throughput**, but with diminishing returns per additional antenna.

**Beamforming** at scale: more antennas = finer beam, higher spatial resolution, better SINR per user.

---

### 3. CoMP — Coordinated Multipoint

**Problem:** Cell-edge users receive poor SINR because they're far from their serving eNB and close to neighboring ones (causing interference).

**Solution:** Multiple eNBs **coordinate** to jointly serve cell-edge users.

**CoMP Types:**

| Mode | Description |
|---|---|
| **Joint Processing (JP)** | Data transmitted from multiple eNBs simultaneously to one UE — UE benefits from all signals |
| **Coordinated Scheduling/Beamforming (CS/CB)** | Only one eNB transmits, but others coordinate to avoid interference |

**Key requirement:** Low-latency backhaul between eNBs (ideally fibre) for tight coordination.

---

### 4. Relaying

**Problem:** Coverage gaps in areas where deploying a full eNB is too expensive.

**Solution:** **Relay Node (RN)** — an intermediate node between eNB and UE.

```
eNB → [backhaul link (Uu)] → RN → [access link (Uu)] → UE
```

| Relay Type | Cell ID | Duplex | Notes |
|---|---|---|---|
| **Type 1** | Yes (own cell ID) | Inband half-duplex | Most common; UE sees it as separate cell |
| **Type 1.a** | Yes | Outband full-duplex | Can TX and RX simultaneously |
| **Type 1.b** | Yes | Inband full-duplex | Requires high antenna isolation |
| **Type 2** | No (same as eNB) | Inband full-duplex | UE can't distinguish from main eNB |

**Use case:** Rural areas, building coverage extension, temporary coverage.

---

### 5. Device to Device (D2D)

- Direct communication between two LTE UEs **without going through eNB**
- Uses licensed LTE spectrum (unlike WiFi Direct or Bluetooth)
- Primary use case: **emergency services** (police, fire, ambulance) who need communication even without network

**Advantages over traditional cellular:**
- Works when network is down (disasters)
- Lower power (short range, direct)
- Lower interference (fewer hops)
- Higher data rates at short range

**Network role:** May assist with peer discovery, synchronization, security even for D2D.

---

## LTE-A vs LTE Performance Comparison

| | LTE (Rel 8) | HSPA+ | LTE-A (Rel 10) |
|---|---|---|---|
| Max DL speed | 100 Mbps | 28–84 Mbps | **1 Gbps** |
| Max UL speed | 50 Mbps | 11 Mbps | **500 Mbps** |
| Latency | ~10 ms | ~50 ms | **<5 ms** |
| Cell edge throughput | Baseline | — | **2× LTE** |
| Average user throughput | Baseline | — | **3× LTE** |
| Spectral efficiency | Baseline | — | **3× LTE** |

---

## LTE Pro (Rel 13+)

Further evolution beyond LTE-A:
- **License Assisted Access (LAA):** Use unlicensed spectrum (5 GHz WiFi band) as secondary CC
- **NB-IoT (Narrowband IoT):** Low-power IoT devices on 180 kHz band
- **eMTC (LTE-M):** Enhanced machine type communications
- **4×4 MIMO for small cells**

These features are stepping stones toward 5G use cases.

---

> **Next:** [07 — 5G NR →](./07-5G-NR.md)

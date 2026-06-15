# 01 — 2G GSM

> **Links:** [← Fundamentals](./00-fundamentals.md) | [README](./README.md) | [GPRS & EDGE →](./02-2G-GPRS-EDGE.md)

---

## Overview

| Parameter | Value |
|---|---|
| Launched | 1991 (Europe first) |
| Full Name | Global System for Mobile Communications (originally "Groupe Speciale Mobile") |
| Multiple Access | TDMA + FDMA combined |
| Channel Bandwidth | 200 kHz |
| Slots per Channel | 8 time slots |
| Modulation | GMSK (Gaussian Minimum Shift Keying) |
| Voice Codec | Various vocoders (AMR etc.) |
| Max Data Rate | 9.6 kbps (circuit switched) |
| Duplex | FDD |
| Frequency Bands | 900 MHz, 1800 MHz (also 850, 1900 in some regions) |

**Milestone:** By 2004, over 1 billion GSM subscribers — took only 12 years (landlines took 100+ years for same number).

---

## Architecture

```
UE (Mobile) → BTS → BSC → MSC ←→ PSTN/Internet
                              ↕
                         HLR | VLR | AuC | EIR
```

### Radio Access Network (BSS — Base Station Subsystem)

| Node | Full Name | Role |
|---|---|---|
| **BTS** | Base Transceiver Station | Air interface radio; communicates directly with UE |
| **BSC** | Base Station Controller | Controls multiple BTSs; manages radio resources, handover within BSC area |
| **BSS** | Base Station Subsystem | BTS + BSC together |

- Interface BTS ↔ BSC: **Abis**
- Interface BSC ↔ MSC (voice): **A interface**
- Interface BSC ↔ SGSN (data/GPRS): **Gb interface**

### Core Network (NSS — Network Switching Subsystem)

| Node | Full Name | Role |
|---|---|---|
| **MSC** | Mobile Switching Centre | Main switch; routes calls; manages handover between BSCs |
| **HLR** | Home Location Register | Master database of all subscribers; stores permanent data (IMSI, subscribed services) |
| **VLR** | Visitor Location Register | Temporary database; stores info about roaming mobiles in current MSC area |
| **AuC** | Authentication Centre | Generates authentication triplets; verifies subscriber identity |
| **EIR** | Equipment Identity Register | Checks IMEI; can blacklist stolen/unauthorized phones |
| **GMSC** | Gateway MSC | Entry point for calls coming into the GSM network from PSTN |

**Key relationship:** When a call comes in, GMSC queries HLR → HLR asks current VLR for roaming number → call routed to correct MSC.

---

## Air Interface

### TDMA Frame Structure
- 1 frame = **4.615 ms**
- 1 frame = **8 time slots**
- Each user assigned 1 slot out of 8 (can be multiple for high-speed services)
- While not transmitting/receiving, UE scans neighboring cell signal strengths (used for handover decisions)

### Multiframe Structure
```
1 hyperframe = 2048 superframes
1 superframe = 51 multiframes (control) OR 26 multiframes (traffic)
1 multiframe = multiple frames
1 frame = 8 time slots
1 time slot = 1 burst (~0.577 ms)
```

---

## Channels

### Logical Channel Types

**Broadcast Channels (downlink only — base station to all UEs):**

| Channel | Full Name | Purpose |
|---|---|---|
| **BCCH** | Broadcast Control Channel | Carries cell identity, frequency info, network params — UE reads this to camp on a cell |
| **FCCH** | Frequency Correction Channel | Helps UE synchronize frequency with BTS |
| **SCH** | Synchronization Channel | Frame synchronization; also carries BSIC (Base Station Identity Code) |

**Common Control Channels:**

| Channel | Direction | Purpose |
|---|---|---|
| **RACH** | UL | UE sends random access request to get a channel |
| **AGCH** | DL | Network assigns a dedicated channel to UE in response to RACH |
| **PCH** | DL | Paging — network calls UE when there's an incoming call/SMS |

**Dedicated Control Channels:**

| Channel | Purpose |
|---|---|
| **SDCCH** | Signaling before TCH is assigned (call setup, SMS, location update) |
| **SACCH** | Slow control; carries measurement reports from UE; sent every frame |
| **FACCH** | Fast control; used during handover; "steals" TCH frames |

**Traffic Channel:**

| Channel | Purpose |
|---|---|
| **TCH/F** | Full rate traffic (voice or 9.6 kbps data) |
| **TCH/H** | Half rate traffic (lower quality, doubles capacity) |

---

## Key Services

- **Voice calls:** Primary service; digitally encoded using vocoder (AMR)
- **SMS (Short Message Service):** Bi-directional, store-and-forward, alphanumeric
- **Circuit-switched data:** Up to 9.6 kbps — very slow, only for basic services
- **No packet data:** GPRS and EDGE added packet data on top of GSM later

---

## SIM — Subscriber Identity Module

- **First introduced in GSM** — a major innovation
- Stores: IMSI (International Mobile Subscriber Identity), authentication keys, phonebook
- Allows easy phone switching without changing identity
- **IMSI format:** MCC (3 digits) + MNC (2-3 digits) + MSIN (up to 10 digits)
- **IMEI:** Device identifier (not on SIM) — can be used to block stolen phones

---

## GSM Handover

### Why Handover?
- UE moves between cells → signal from current BTS weakens → need to switch to stronger BTS
- Also triggered by: interference, cell congestion

### MAHO — Mobile Assisted Hand Over
- UE continuously scans beacon frequencies (BCCH) of neighboring cells during idle slots
- Reports signal strengths to BTS via SACCH
- **Network makes the decision** based on UE measurements
- This is the "mobile assisted" part — UE assists but doesn't decide

### Four Types of GSM Handover

| Type | What Changes | Who Controls |
|---|---|---|
| **Intra-BTS** | Slot or frequency within same BTS | BTS |
| **Inter-BTS Intra-BSC** | Different BTS, same BSC | BSC |
| **Inter-BSC** | Different BSC, same MSC area | MSC |
| **Inter-MSC** | Different MSC (different network area) | Both MSCs negotiate |

### Synchronization in Handover
- **Synchronized BTSs:** Direct handover — smooth
- **Time offset between BTSs:** Offset provided to UE — still smooth
- **Non-synchronized:** UE transmits 64 access bursts to help new BTS establish timing

### Inter-RAT Handover (GSM ↔ UMTS)
- UMTS → GSM: **Compressed mode** (UE uses gaps to scan GSM cells) or **Blind handover**
- GSM → UMTS: Uses neighbour list; occurs to improve performance, not coverage

---

## GSM Design Goals (Why It Was Designed This Way)

- Good subjective speech quality
- Low terminal cost — affordable handsets
- Handheld terminals — compact enough to carry
- International roaming capability
- Good spectral efficiency (TDMA gave 8× over analogue)
- ISDN compatibility for integration with fixed networks

---

## GSM Limitations

- Only circuit switched → inefficient for data (channel held even when idle)
- Max data rate 9.6 kbps → too slow for internet browsing
- Led to development of GPRS (packet data) and eventually 3G

---

> **Next:** [02 — 2G GPRS & EDGE →](./02-2G-GPRS-EDGE.md)

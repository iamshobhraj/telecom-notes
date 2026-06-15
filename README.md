# 📡 Telecom Notes — Ericsson NDO Prep

> Atomic notes on cellular technology from 2G to 6G.  
> Every file stands alone but links to related notes.  
> Start here → follow links → use Quick Reference for revision.

---

## 📂 Contents

| File | What's Inside |
|---|---|
| [00 — Fundamentals](./00-fundamentals.md) | Cells, frequency reuse, multiple access, generations overview |
| [01 — 2G GSM](./01-2G-GSM.md) | Architecture, channels, handover, SIM, MAHO |
| [02 — 2G GPRS & EDGE](./02-2G-GPRS-EDGE.md) | Packet data, SGSN/GGSN, 8PSK, MCS schemes |
| [03 — 3G UMTS](./03-3G-UMTS.md) | WCDMA, CDMA basics, RAKE receiver, soft handover |
| [04 — 3G HSPA](./04-3G-HSPA.md) | HSDPA/HSUPA, AMC, HARQ, DC-HSPA evolution |
| [05 — 4G LTE](./05-4G-LTE.md) | OFDMA/SC-FDMA, EPC, protocol stack, VoLTE |
| [06 — 4G LTE Advanced](./06-4G-LTE-Advanced.md) | Carrier aggregation, CoMP, relaying, D2D |
| [07 — 5G NR](./07-5G-NR.md) | eMBB/URLLC/mMTC, NSA vs SA, 5GC, beamforming |
| [08 — 6G](./08-6G.md) | Future concepts, THz, research projects |
| [09 — KPIs & Optimization](./09-KPIs-optimization.md) | RSRP, RSRQ, SINR, CSSR, CDR — thresholds and diagnosis |
| [10 — Quick Reference](./10-quick-reference.md) | All comparison tables, cheat sheet for revision |

---

## 🔁 Evolution at a Glance

```
1G (1979) → Analogue voice
2G (1991) → Digital voice + SMS + basic data
  └─ GPRS: packet data (2.5G)
  └─ EDGE: higher data rate (2.75G)
3G (2001) → Mobile broadband
  └─ HSPA: faster 3G data (3.5G)
  └─ HSPA+: even faster (3.75G)
4G (2009) → All-IP, high speed
  └─ LTE Advanced: carrier aggregation, CoMP
  └─ LTE Pro: further enhancements
5G (2018) → Ubiquitous connectivity (eMBB + URLLC + mMTC)
6G (~2030) → THz, sub-microsecond latency
```

---

## 🧠 Node Name Evolution

| Function | 2G | 3G | 4G | 5G |
|---|---|---|---|---|
| Base Station | BTS | NodeB | eNodeB (eNB) | gNodeB (gNB) |
| Controller | BSC | RNC | — (merged into eNB) | — |
| Mobility Mgmt | MSC | MSC / SGSN | MME | AMF |
| User Plane GW | — | GGSN | P-GW + S-GW | UPF |
| Session Mgmt | — | — | P-GW (control) | SMF |
| Subscriber DB | HLR | HLR | HSS | UDM |

---

## ⚡ Quick Numbers

| Generation | Max Speed | Latency | Multiple Access |
|---|---|---|---|
| 2G GSM | 9.6 kbps | ~500 ms | TDMA + FDMA |
| 2G GPRS | 172 kbps | ~300 ms | TDMA + FDMA |
| 2G EDGE | 384 kbps | <100 ms | TDMA + FDMA |
| 3G UMTS | 2 Mbps | ~150 ms | CDMA (WCDMA) |
| 3G HSPA | 14.4 Mbps DL | ~100 ms | CDMA |
| 3G HSPA+ | 28–84 Mbps | ~50 ms | CDMA |
| 4G LTE | 100 Mbps | ~10 ms | OFDMA / SC-FDMA |
| 4G LTE-A | 1 Gbps | <5 ms | OFDMA / SC-FDMA |
| 5G NR | 20 Gbps | <4 ms | OFDMA (flexible) |

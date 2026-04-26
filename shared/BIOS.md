# BIOS Settings — ROG Strix Z790-A Gaming WiFi D4

## System

- **CPU**: Intel Core i5-14600K (SP 66)
- **Motherboard**: ASUS ROG Strix Z790-A Gaming WiFi D4
- **BIOS**: 3107 or newer (microcode 0x12F — fixes 13/14th gen Vmin Shift degradation)
- **Cooler**: be quiet! Dark Rock Pro 5
- **RAM**: Kingston Fury Beast DDR4 2×32GB 3200 CL16 (dual-rank)

## Targets

| Setting | Value |
|---|---|
| P-core 1-core boost | 5.7 GHz (5.6 if SP66 unstable) |
| P-core multi-core | 5.5 GHz |
| E-core all-core | 4.3 GHz |
| Ring/Cache | 4.7 GHz |
| RAM | DDR4-3600 CL16 Gear 1 |
| PL1 / PL2 | 181W / 200W |
| IccMax | 200A |

---

## Phase 0 — Prep

1. Update BIOS to 3107 or newer. Use BIOS FlashBack via USB (rear I/O button) if needed.
2. Clear CMOS after flash (rear I/O button).
3. Boot, F2/Del → press **F7** for Advanced Mode.
4. Save baseline: `Tool → ASUS User Profile → Save to Profile 1` ("stock_baseline").

---

## Phase 1 — CPU OC

### Ai Tweaker

```
Ai Overclock Tuner                          XMP I
ASUS MultiCore Enhancement                  Auto - Let BIOS Optimize
SVID Behavior                               Trained
BCLK Frequency                              Auto

Performance Core Ratio                      By Core Usage
  1-Core Ratio Limit                        57   (drop to 56 if unstable)
  2-Core Ratio Limit                        56
  3-Core Ratio Limit                        55
  4-Core Ratio Limit                        55
  5-Core Ratio Limit                        55
  6-Core Ratio Limit                        55

Efficient Core Ratio                        Sync All Cores
  All-Core Ratio Limit                      43

CPU Cache Ratio (Min/Max)                   47 / 47
```

### Ai Tweaker → DIGI+ VRM

```
CPU Load-line Calibration                   Level 4
CPU Current Capability                      140%
VRM Spread Spectrum                         Disabled
CPU Power Phase Control                     Extreme
CPU Power Duty Control                      T.Probe
```

### Ai Tweaker → Internal CPU Power Management

```
Long Duration Power Limit (PL1)             181
Package Power Time Window                   56
Short Duration Power Limit (PL2)            200
CPU Core/Cache Current Limit Max (IccMax)   200
IA AC Load Line                             Auto
IA DC Load Line                             Auto
```

### Ai Tweaker → bottom (voltages)

```
Actual VRM Core Voltage                     Auto
Global Core SVID Voltage                    Adaptive Mode
  Offset Mode Sign                          -
  Offset Voltage                            0.030
  Additional Turbo Mode CPU Core Voltage    Auto

Cache SVID Voltage                          Adaptive Mode
  Offset Mode Sign                          -
  Offset Voltage                            0.020

CPU System Agent Voltage                    Auto
CPU Input Voltage                           Auto
BCLK Aware Adaptive Voltage                 Auto
CPU L2 Voltage                              Auto
IVR Transmitter VDDQ Voltage                Auto
```

### Advanced → CPU Configuration

```
Hyper-Threading                             Enabled
Active Performance Cores                    All
Active Efficient Cores                      All
Intel SpeedStep                             Enabled
Intel Speed Shift Technology                Enabled
CPU C-States                                Enabled
Intel Adaptive Thermal Monitor              Enabled
Legacy Game Compatibility Mode              Disabled
```

### Advanced → System Agent Configuration

```
Above 4G Decoding                           Enabled
Re-Size BAR Support                         Enabled
```

### Advanced → APM Configuration

```
ErP Ready                                   Disabled
```

Save: `Tool → User Profile → Save to Profile 2` ("cpu_only_57_47") → **F10 → Y**.

---

## Phase 2 — RAM (only after CPU validated)

> When you switch to manual RAM frequency, ASUS may lock DRAM Voltage if XMP I is selected. Switching to **Manual** Ai Overclock Tuner unlocks it.

### Ai Tweaker

```
Ai Overclock Tuner                          Manual
DRAM Frequency                              DDR4-3600
Memory Controller:DRAM Frequency Ratio      1:1   (forces Gear 1)
```

### Ai Tweaker → DRAM Timing Control (start loose, Phase 2)

```
Primary Timings:
  DRAM CAS# Latency                         18
  DRAM RAS# to CAS# Delay                   22
  DRAM RAS# PRE Time                        22
  DRAM RAS# ACT Time                        42
  DRAM Command Rate                         2N

Subtimings:
  DRAM REF Cycle Time                       620
  DRAM REF Cycle Time 2                     620
  DRAM Refresh Interval (tREFI)             65535
  DRAM RAS# to RAS# Delay L                 Auto
  DRAM RAS# to RAS# Delay S                 Auto
  DRAM FOUR ACT WIN                         Auto
  DRAM READ to PRE Time                     Auto
```

### Voltages

```
DRAM Voltage                                1.40000
CPU System Agent Voltage                    1.250
IVR Transmitter VDDQ Voltage                Auto
```

Save → boot → verify in BIOS Hardware Monitor right pane: **DRAM Freq 3600 MHz**. Verify in HWiNFO64 (Windows): Memory Clock ~1800 MHz.

> MemTest86 displays SPD base profile (3200), not actual frequency. Use HWiNFO64 or BIOS Hardware Monitor for real reading.

### Phase 3 — Tighten primaries (after Phase 2 validated)

```
DRAM CAS# Latency                           16
DRAM RAS# to CAS# Delay                     19
DRAM RAS# PRE Time                          19
DRAM RAS# ACT Time                          39
```

### Phase 4 — Tighten subtimings (optional)

```
DRAM REF Cycle Time                         560
DRAM REF Cycle Time 2                       560
DRAM RAS# to RAS# Delay L                   8
DRAM RAS# to RAS# Delay S                   6
DRAM FOUR ACT WIN                           24
DRAM READ to PRE Time                       10
```

Save final: `Tool → User Profile → Save to Profile 3` ("cpu_57_ram_3600").

---

## Fallback ladders

### CPU instability

```
CB single WHEA / crash       →  offset  -0.030 → -0.020 → -0.010
y-cruncher cache error       →  ring    47 → 46 → 45
Crash under load             →  top P-core 57 → 56
Temps >90°C R23 multi        →  reseat cooler, repaste
```

### RAM instability

```
Phase 2 won't POST           →  drop to DDR4-3466, same loose timings
Phase 2 errors               →  bump VCCSA  1.25 → 1.30 (max 1.35)
                                bump DRAM   1.40 → 1.42 (max 1.45)
Phase 3 errors               →  CL 16 → 17, others same
Phase 4 errors               →  loosen one sub at a time
3466 also won't post         →  XMP 3200 + tighten primaries to CL14-17-17-36
```

---

## Saved BIOS profiles

| Slot | Name | Description |
|---|---|---|
| 1 | stock_baseline | Untouched defaults — emergency fallback |
| 2 | cpu_only_57_47 | CPU OC, RAM at XMP 3200 |
| 3 | cpu_57_ram_3600 | Full tune — CPU OC + RAM 3600 tuned |

Load Profile 1 from BIOS to revert instantly if anything breaks.

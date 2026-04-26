# Stability Testing & Validation

## Order matters — test CPU first, then RAM

Each test must pass before moving to the next. If a test fails, fix the issue before continuing.

---

## CPU validation (after Phase 1 in BIOS)

```
1. Idle 30 min                         HWiNFO64 watching for crashes / VID spikes
2. Cinebench R23 single                expect ~2250-2350
3. Cinebench R23 multi 10 min          temps <90°C
4. y-cruncher Component Stress 1h      cache/AVX validation, all components
5. OCCT Extreme Variable AVX2 1h       final torture
```

### Expected behavior under R23 multi

HWiNFO64 will show effective P-core clocks of ~5000-5100 MHz under all-core load (PL1 caps at 181W, forces throttling). Single/few-thread loads hit 5.7. **This is correct** — for CS2's 1-4 thread workload, single-core boost is what matters.

Package power should be ~140-150W avg, ~144W peak in CB R23.

---

## RAM validation (after Phase 2/3/4 in BIOS)

### Free tools

**TestMem5 (TM5) with anta777 extreme config** — gold standard
- Most sensitive memory tester
- Run 45-60 min minimum for Phase 2
- Run 1 hour for Phase 3+4
- Errors must = 0

**y-cruncher VST** — combined cache + RAM stress
- Run 30 min after TM5 passes
- Catches errors TM5 misses

**MemTest86** — boot-time validator
- Already used during Phase 2 setup
- Less sensitive than TM5 for OC tuning, but good for catching gross errors
- Note: displays SPD base profile, not actual frequency

### TestMem5 anta777 extreme setup

1. Download TM5 archive (search "TestMem5 anta777" on GitHub or HWBot)
2. Extract
3. Download `anta777-extreme.cfg`
4. Place .cfg in `bin\` subfolder
5. Run `MemTestPro.exe` as administrator
6. Click icon to load config → pick `anta777 extreme`
7. Press **Run**
8. Watch the **Errors** counter — anything >0 = unstable

For 64GB it takes ~45-60 min for 1 full cycle.

---

## Verification — HWiNFO64 sensors under full load

```
P-cores Effective Clock        5.7 GHz on cores 0/1 in single-thread
                               5.0-5.1 GHz all-core (PL1 capped, expected)
E-cores Effective Clock        4.3 GHz under load
Ring Effective Clock           4.7 GHz
P-core VID Max                 ~1.30-1.35 V
Package Power                  ~140-150W avg in R23 multi (PL1 capped at 181)
Memory Clock                   1800 MHz (= DDR4-3600)
P-core Temp Max                <90°C
```

---

## Final real-world check

CS2 d2 community benchmark + 30 min DM session. Watch HWiNFO log for:

- WHEA errors (page faults under stress)
- Unexpected throttling
- 1% lows in benchmark tool

If real-world testing crashes but synthetic passes → likely RAM Phase 4 too tight, loosen one subtiming at a time.

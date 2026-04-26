# Steam & CS2 Configuration

## Steam launch options

Right-click CS2 in Steam → Properties → Launch Options:

```
-high -nojoy -allow_third_party_software +fps_max 0 +cl_forcepreload 1
```

### What each flag does

| Flag | Purpose |
|---|---|
| `-high` | Sets process priority to High |
| `-nojoy` | Disables joystick init (saves resources) |
| `-allow_third_party_software` | Allows tools like MSI Afterburner overlay |
| `+fps_max 0` | Removes engine FPS cap |
| `+cl_forcepreload 1` | Preloads map assets (smoother first-round) |

### Don't add `-threads N`

Source 2 ignores this flag and it can cause issues. Engine handles threading itself.

---

## CS2 in-game settings

### Game tab (critical for Intel hybrid CPUs)

```
Settings → Game → CPU Core Usage Preference     Prefer Performance Cores
```

- **Auto detect** — leaves it to Windows scheduler (worse on 14600K)
- **Prefer efficiency cores** — bad for FPS
- **Prefer performance cores** ← **PICK THIS**
- **Performance cores only** — disables E-cores, costs FPS in CS2 (Source 2 uses them)

### Video — performance-critical settings

```
Wait for Vertical Sync                      Disabled
NVIDIA Reflex Low Latency                   Enabled (NOT Enabled + Boost)
Boost Player Contrast                       Enabled
MSAA                                        None or 2x
Global Shadow Quality                       Low
Dynamic Shadows                             All
Model / Texture Detail                      Low
Texture Filtering Mode                      Bilinear or Anisotropic 2x
Shader Detail                               Low
Particle Detail                             Low
Ambient Occlusion                           Disabled
HDR                                         Quality (cheap, looks better)
FidelityFX Super Resolution                 Disabled
```

### Resolution

Most pros use 4:3 stretched (1280×960 or 1024×768) for wider player models and higher FPS. If using 16:9, native 1920×1080.

To create custom resolutions: GPU control panel (NVIDIA Custom Resolution) before selecting in-game.

---

## Myths to ignore (tested, don't help)

- **Disable Hyper-Threading** — costs FPS on 14600K (community testing showed -20 FPS)
- **Disable E-cores** — costs ~40 avg FPS and 20 1% lows on 13700K-class hybrid CPUs
- **Set high priority manually in Task Manager** — no measurable effect over `-high` launch flag

---

## Real performance gains (verified)

These actually work:

- **In-game "Prefer Performance Cores"** — Valve's CS2-specific scheduler hint
- **Game Mode On** (Windows) — disabling it costs FPS
- **ReBar enabled** (BIOS) — disabling it costs FPS
- **XMP enabled** (BIOS) — basic but huge

---

## Troubleshooting

### CS2 dropdown for "CPU Core Usage Preference" missing

You're on an old build. In Steam:
- Right-click CS2 → Properties → Updates → Always keep updated
- Verify game files: Properties → Installed Files → Verify integrity

### CS2 keeps crashing after OC

- Check HWiNFO64 for WHEA errors
- Most likely RAM instability (Phase 4 too aggressive) — loosen subtimings
- If single-thread workloads crash but multi is fine — top P-core ratio too high (57 → 56)

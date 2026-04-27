# Steam & CS2 Configuration

## Steam launch options

Right-click CS2 in Steam → Properties → Launch Options:

```
-high -nojoy +cl_forcepreload 1
```

### What each flag does

| Flag | Purpose |
|---|---|
| `-high` | Sets process priority to High |
| `-nojoy` | Disables joystick init (saves resources) |
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

Use 4:3 stretched.

---

## Myths to ignore (tested, don't help)

- **Disable Hyper-Threading** — costs FPS on 14600K (community testing showed -20 FPS)
- **Disable E-cores** — costs ~40 avg FPS and 20 1% lows on 13700K-class hybrid CPUs
- **Set high priority manually in Task Manager** — no measurable effect over `-high` launch flag

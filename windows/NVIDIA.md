# NVIDIA Control Panel — cs2.exe profile

`NVCP → Manage 3D Settings → Program Settings → Add → cs2.exe`

If cs2.exe doesn't appear in the dropdown, browse to:
`C:\Program Files (x86)\Steam\steamapps\common\Counter-Strike Global Offensive\game\bin\win64\cs2.exe`

## Per-game settings

```
CUDA - GPUs                                 Use these GPUs (your GPU)
Low Latency Mode                            Ultra
Max Frame Rate                              Off
                                            (or refresh -3, e.g. 237 for 240Hz)
Monitor Technology                          G-SYNC compatible (if available)
OpenGL rendering GPU                        Your GPU
Power management mode                       Prefer maximum performance
Preferred refresh rate                      Highest available
Shader Cache Size                           Unlimited
Texture filtering — Anisotropic             Application-controlled
Texture filtering — Negative LOD bias       Allow
Texture filtering — Quality                 High performance
Texture filtering — Trilinear optimization  On
Threaded optimization                       On
Triple buffering                            Off
Vertical sync                               Off
Vulkan/OpenGL present method                Prefer layered on DXGI swapchain
```

## Reflex notes

- **Reflex Low Latency** is in CS2 (in-game video settings), not NVCP
- Set in CS2 to **On** (NOT On + Boost)
- "On + Boost" forces GPU to high power state but conflicts with Ultra Low Latency in NVCP

## Apply and verify

After setting all values, click **Apply** in bottom right. Reboot for changes to fully take effect.

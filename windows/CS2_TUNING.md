# Windows Tuning (FACEIT-compatible)

## Critical — do NOT disable these

FACEIT anti-cheat requires these features. Disabling breaks FACEIT.

```
Memory Integrity (Core Isolation)           ON   (required by FACEIT)
Secure Boot                                 ON   (required from Nov 2025)
TPM 2.0                                     ON   (required from Nov 2025)
VT-x / Intel Virtualization                 ON   (required for above)
```

If you only play Valve MM/Premier (VAC), Memory Integrity can be disabled for ~10-20% extra FPS. But losing FACEIT access is rarely worth it.

---

## Power plan — Ultimate Performance

### Activate via PowerShell (run as Administrator)

```powershell
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
powercfg /list
powercfg /setactive <GUID-shown-after-Ultimate-Performance>
powercfg /getactivescheme
```

The `duplicatescheme` command only **creates** the plan — you must `setactive` it explicitly.

### Advanced settings

Control Panel → Power Options → Change advanced power settings:

```
Hard disk → Turn off                        Never
Sleep                                       Never
PCI Express → Link State Power Mgmt         Off
Processor power management:
  Minimum processor state                   100%
  Maximum processor state                   100%
  System cooling policy                     Active
USB selective suspend                       Disabled
```

Min processor state at 100% prevents C-state idle dips that cause CS2 micro-stutters.

---

## Settings app tweaks

```
Settings → Gaming → Game Mode               On
Settings → System → Display → Graphics:
  Hardware-accelerated GPU scheduling       On    (reboot required)
Settings → Gaming → Game Bar:
  Allow controller to open Game Bar         Off
```

### Disable Game DVR (the actual perf killer)

PowerShell:

```powershell
reg add "HKCU\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f
```

---

## Disable fullscreen optimizations on cs2.exe

Path: `C:\Program Files (x86)\Steam\steamapps\common\Counter-Strike Global Offensive\game\bin\win64\cs2.exe`

Right-click cs2.exe → Properties → Compatibility:

```
☑ Disable fullscreen optimizations
☑ Run as administrator                    (optional, helps process priority)
Change high DPI settings → Override scaling: Application
```

---

## Windows Defender exclusion for Steam

`Settings → Privacy → Windows Security → Virus & threat protection → Manage settings → Exclusions → Add or remove exclusions → Add → Folder`:

```
C:\Program Files (x86)\Steam
```

Reduces I/O overhead when CS2 loads maps and assets.

---

## Mouse input

`Settings → Bluetooth & devices → Mouse → Additional mouse settings → Pointer Options`:

```
☐ Enhance pointer precision               (uncheck)
```

---

## Optional advanced tweaks

### Network latency (Nagle's algorithm)

Find your network adapter's interface ID:

```powershell
Get-NetAdapter | Select Name, InterfaceGuid
```

Then in `regedit`: `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{your-guid}`

Add two DWORDs:

```
TcpAckFrequency                             1
TCPNoDelay                                  1
```

### Frame pacing — timer settings

```cmd
bcdedit /set useplatformtick yes
bcdedit /set disabledynamictick yes
```

Test with **clockres.exe** (Sysinternals) — should show 0.5ms after.

### MSI mode for GPU

Use **MSI Utility V3** → set GPU to MSI mode → reboot. Reduces interrupt latency, slightly smoother frame pacing (~1-3 FPS, better consistency).

---

## Verification checklist after reboot

```powershell
powercfg /getactivescheme           # → Ultimate Performance
msinfo32                            # check VBS / Memory Integrity status
```

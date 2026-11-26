# Joyeuse battery shim (in-tree kernel integration)

Target:
- Device: Xiaomi Redmi Note 9 Pro Global (Joyeuse / SM7125)
- postmarketOS: v26.06
- Kernel source: https://github.com/sm7125-mainline/linux
- Kernel tag: `sm7125-6.14.7`
- Expected kernel release: `6.14.7-sm7125`
- Architecture: ARM64 / aarch64

The workflow copies `joyeuse_battery_shim.c` into the kernel source at
`drivers/power/supply/`, adds a `JOYEUSE_BATTERY_SHIM` Kconfig entry and
Makefile object entry, and builds it as an in-tree kernel module.

## Manual workflow

Open Actions → **Joyeuse kernel / battery shim** → Run workflow.
Choose exactly one build mode:

- `kernel`: builds Image, modules, and DTBs; the shim is included as a module.
- `module-only`: skips Image and DTB generation and builds the kernel modules
  (including the shim). Building the modules target supplies the kernel symbol
  version data needed for module compatibility.

The workflow is manual-only. It has no push trigger and does not modify rootfs,
systemd, NetworkManager, RNDIS, SSH, or flash the device.

Artifacts are build outputs only. They are not installed on the phone automatically.

## Important validation

A successful compile does not validate the ATL battery profile, current sign,
Type-C role detection, charging status, or capacity estimation on hardware.
Review the build metadata and test carefully before loading the module.

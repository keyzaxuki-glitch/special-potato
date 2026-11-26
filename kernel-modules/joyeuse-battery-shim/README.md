# Joyeuse battery shim (external kernel module)

Target device: Xiaomi Redmi Note 9 Pro Global (Joyeuse / SM7125), aarch64.

## Target kernel

- postmarketOS: v26.06
- Kernel release: 6.14.7-sm7125
- Kernel source: https://github.com/sm7125-mainline/linux
- Source tag: sm7125-6.14.7
- Source commit: ac499943aba5f6a7dd56d6a88952b939d9f13f6a
- Kernel configuration: pmaports v26.06,
  device/testing/linux-postmarketos-qcom-sm7125/config-postmarketos-qcom-sm7125.aarch64

## Source placement

Place the supplied C source at:

`kernel-modules/joyeuse-battery-shim/joyeuse_battery_shim.c`

The adjacent `Makefile` builds it as an out-of-tree module named
`joyeuse_battery_shim.ko`.

## Build

The GitHub Actions workflow builds the exact tagged kernel source with the
postmarketOS device kernel configuration first. This full kernel build is
intentional: `modules_prepare` alone does not generate a complete
`Module.symvers` when CONFIG_MODVERSIONS is enabled.

The workflow then builds the external module against that prepared kernel
tree and checks the module vermagic against `6.14.7-sm7125`.

The output is uploaded as a GitHub Actions artifact. It is not installed on
the phone, flashed, or automatically committed as a binary to this branch.
The root filesystem and device services are not modified.

## Runtime caution

A successful compile only establishes source/API compatibility with this
kernel build. It does not establish correct battery readings or safe runtime
behavior on the phone.

The module registers a power_supply named `battery`, reads the qcom_qg
power_supply and Type-C role state, and applies an OCV/capacity correction
pipeline. Before loading it on a device, verify that no existing power_supply
already owns the `battery` name, confirm qcom_qg current sign and property
units, and validate the ATL battery table and aged-cell parameters against
the actual battery/device data. Do not enable automatic loading until those
checks and device-side testing are complete.

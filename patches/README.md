# Joyeuse kernel notes

Target device: Xiaomi Redmi Note 9 Pro (Joyeuse), platform SM7125 / miatoll.

The kernel-only workflow builds the pmaports package
`linux-postmarketos-qcom-sm7125` and uploads its APK artifact. It does not run
`pmbootstrap install`, create a rootfs, patch a boot image, flash the phone, or
change the existing Wi-Fi/RNDIS/SSH setup.

## Charger and battery work

The battery-gauge / charger-control code discussed for Joyeuse is **not yet
integrated**. In particular, this branch does not write PM6150 SMB5 registers,
change ICL/FCC/termination values, add a software JEITA charge-control policy,
or replace the Huaxing-PD device tree.

Those changes affect physical charging and thermal limits. They must not be
copied into this build as an unverified bundle. The source revision, PMIC
register map, battery temperature reporting, charge-path behavior, and the
exact Joyeuse board/DTB configuration would need a separate hardware review
before any charge-control change is considered.

The ATL 5020 mAh OCV correction for `qcom_qg` is also not enabled yet. Its
profile data, units, interpolation, and compatibility with the exact pmaports
kernel revision still need verification. A guessed or mismatched curve can
make the displayed state of charge inaccurate.

## Build output

The workflow uploads the kernel package APK and SHA-256 checksum as a GitHub
Actions artifact. It intentionally does not flash or install anything on the
phone.

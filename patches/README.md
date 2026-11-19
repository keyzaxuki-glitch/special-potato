# Joyeuse kernel patches

Target device: Xiaomi Redmi Note 9 Pro (Joyeuse), platform SM7125 / miatoll.

The kernel-only workflow builds the pmaports package
`linux-postmarketos-qcom-sm7125` and uploads its APK artifact. It does not run
`pmbootstrap install`, create a rootfs, patch a boot image, or change the
existing Wi-Fi/RNDIS/SSH setup.

## Battery gauge patch status

The ATL 5020 mAh OCV-based `qcom_qg` change discussed for Joyeuse is **not
applied by this workflow yet**. The exact pmaports-selected kernel source
revision and the ATL profile's provenance/units must be checked together
before adding it as an active patch. Applying an unverified OCV table or
changing charger parameters could produce incorrect state-of-charge reporting.

When validated, add the patch here and wire it into the matching
`linux-postmarketos-qcom-sm7125` APKBUILD patch series (or apply it to the
exact kernel source before invoking pmbootstrap's local-source build). Keep
charger FCC/ICL/JEITA settings and the Huaxing-PD DTB unchanged.

## Output

The workflow uploads the kernel package APK and SHA-256 checksum as a GitHub
Actions artifact. It intentionally does not flash the device.

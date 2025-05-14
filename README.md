# TWRP Recovery for Oukitel WP15
[Tested by theophriene](https://github.com/theophriene)

**Your warranty may be voided.**

**This is because it is required to unlock the bootloader and enter fastboot mode.**

**This repository contains the resources and instructions needed to build a fully featured TWRP recovery image for the Oukitel WP15. All hardware buttons work, and this recovery enables advanced functionality such as flashing GSIs, sideloading, and performing system backups.**

## Device Notes

The Oukitel WP15 uses an A/B partition layout, meaning a traditional `recovery.img` cannot be used. Instead, the recovery must be embedded directly into the `boot.img`. 

This TWRP boot image is based on the **stock ROM**: `TF919_OQ_S89_6833_R0_EEA_V2.8.3.1_S231121`, which is available for download from the [official Oukitel website](https://oukitel.com/).

⚠️ **Warning** if you already have custom os on the device and not the stock ROM, booting will fail. but you will meet the recovery. reinstalling the os/gsi is required. 

The TWRP binaries were provided by [Hovatek](https://www.hovatek.com/). Device-specific binaries and trees were provided by the maintainer of this repository.

➡️ A **prebuilt boot.img** is available for download under [Releases](https://github.com/furrofurry/twrp-recovery-oukitel-wp15/releases/tag/boot.img), ready to be flashed directly.

## Requirements

- [`osm0sis/mkbootimg`](https://github.com/osm0sis/mkbootimg) (make sure it's cloned and accessible in your PATH)
  - Depending on the version, the command may be `mkbootimg` or `./mkbootimg.py`.
- On Arch Linux systems, you can install it using:

```bash
yay -S mkbootimg-git
```

## Instructions
[Instructions for the bootloader to be unlocked](https://xdaforums.com/t/oukitel-wp15.4402253/post-86465361)


### 1. Clone this repository

```bash
git clone https://github.com/furrofurry/twrp-recovery-oukitel-wp15
```

### 2. Change into the project directory

```bash
cd twrp-recovery-oukitel-wp15
```

### 3. Build the image

Run the following command to build the `boot.img` from source:

```bash
mkbootimg \
--header_version 2 \
--kernel kernel \
--ramdisk ramdisk \
--dtb dtb \
--base 0x40000000 \
--kernel_offset 0x00080000 \
--ramdisk_offset 0x11100000 \
--tags_offset 0x07c80000 \
--dtb_offset 0x41f78000 \
--os_version 12.0.0 \
--os_patch_level 2099-12 \
--pagesize 2048 \
--cmdline 'bootopt=64S3,32N2,64N2 buildvariant=user twrpfastboot=1 buildvariant=eng' \
--output new_boot.img
```

> **Note**: If you're using the Python version from `osm0sis/mkbootimg`, the command might be:
>
> ```bash
> ./mkbootimg.py ...
> ```

### 4. Flash with Fastboot

Once built, you can flash the new image to your device using:

```bash
fastboot flash boot new_boot.img
```
✅ Done
---

## Ramdisk and DTB Note

For efficiency, the `ramdisk` and `dtb` files are pre-built from source and included in this repository. If needed, their contents can be extracted and verified manually, for ramdisk using 
``` bash
gzip -dc ramdisk | cpio -i
```
Recovery binary located at [/system/bin/recovery](https://github.com/furrofurry/twrp-recovery-oukitel-wp15/blob/main/system/bin/recovery)

---

## Post-Installation

Once TWRP is booted, you can use it to:

- Install a GSI (Generic System Image) of your choice
- Use ADB sideload to flash packages
- Perform full system backups and restores
- Wipe partitions and manage files

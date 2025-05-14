# TWRP Recovery for Oukitel WP15

This repository contains the resources and instructions needed to build a TWRP recovery image for the Oukitel WP15.

## Requirements

- [`osm0sis/mkbootimg`](https://github.com/osm0sis/mkbootimg) (make sure it's cloned and accessible in your PATH)
  - Depending on the version, the command may be `mkbootimg` or `./mkbootimg.py`.

## Instructions

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

---

## Ramdisk and DTB Note

For efficiency, the `ramdisk` and `dtb` files are pre-built from source and included in this repository. If needed, their contents can be extracted and verified manually.

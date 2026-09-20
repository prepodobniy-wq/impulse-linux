# Impulse Linux

An educational Linux distro based on Linux v6.12.9 (Buildroot) featuring a custom kernel security module (LSM) called Impulse Guard.

## Current features ===============
- Kernel: fork of https://github.com/prepodobniy-wq/linux, `impulse` branch
- Kernel hardening config: `configs/linux-hardening.fragment`
- Impulse LSM (`security/impulse/` within the kernel): prevents program execution from `/tmp` and `/dev/shm`; event log at `/sys/kernel/security/impulse/log` - `overlay/`: files included in the image (securityfs auto-mounting, `impulse-demo` script)

## Build (WSL2, Ubuntu)
Requirements: build-essential git libcurses-dev wget cpio unzip rsync bc file
libssl-dev libelf-dev flex bison qemu-system-x86 gcc-13 g++-13

Buildroot 2025.02 does not build with gcc-15, so host utilities are built using gcc-13:

          make HOSTCC=gcc-13 HOSTCXX=g++-13

In brief: download Buildroot 2025.02, apply `configs/impulse_defconfig`, clone the kernel to `~/linux`, and specify it in `buildroot/local.mk`: `LINUX_OVERRIDE_SRCDIR = /home/USER/linux`.

Known limitation: paths in the defconfig are currently absolute (`/home/hooken/...`). They will be replaced using `br2-external`.

## Launch and Demo

      ./output/images/start-qemu.sh serial-only
      impulse-demo

To exit QEMU:
Ctrl+A, then X.

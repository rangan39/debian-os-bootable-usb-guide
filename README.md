# Debian Bootable USB Guide

A quick guide for creating a bootable Debian installer USB on macOS, and installing Debian from it.

## Prerequisites

- A USB flash drive (8 GB or larger recommended). **All data on it will be erased.**
- A Mac with an internet connection.
- Administrator (`sudo`) access.

## 1. Download the Debian ISO

Download the latest Debian netinst image from the official mirror:

https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/

At the time of writing, the current release is `debian-13.6.0-amd64-netinst.iso`.

## 2. Write the ISO to the USB drive

You can use either the built-in `dd` command or a GUI tool like [balenaEtcher](https://etcher.balena.io/).

### Option A: `dd` (command line)

> **Warning:** `dd` will overwrite the target device with no confirmation. Double-check the disk identifier before running the write command — writing to the wrong disk can destroy data on your Mac's internal drive.

1. Insert the USB drive, then list all disks to identify it:

   ```bash
   diskutil list
   ```

   Look for your USB drive by its size and name, e.g.:

   ```
   /dev/disk7 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *61.5 GB    disk7
   1:             Windows_FAT_32 rangan39                61.5 GB    disk7s1
   ```

2. Unmount the disk (this does not erase it):

   ```bash
   diskutil unmountDisk /dev/disk7
   ```

3. Write the ISO to the disk, using the raw device (`/dev/rdiskN`) for faster writes:

   ```bash
   sudo dd if=/Users/gauravranganath/debian-13.6.0-amd64-netinst.iso of=/dev/rdisk7 bs=4M status=progress
   ```

4. Once the write finishes, flush any pending writes and eject the drive:

   ```bash
   sync
   sudo diskutil eject /dev/disk7
   ```

### Option B: balenaEtcher (GUI)

1. Install and open [balenaEtcher](https://etcher.balena.io/).
2. Select the downloaded Debian ISO as the source image.
3. Select the USB drive as the target.
4. Click **Flash** and wait for it to complete and verify.

## 3. Boot from the USB and install Debian

1. Insert the USB drive into the target machine and power it on (or restart it).
2. Enter the boot/startup menu (the key varies by manufacturer — commonly `F12`, `F10`, `Esc`, or `Del`) and select the USB drive as the boot device.
3. Follow the Debian installer prompts (language, keyboard layout, network, disk partitioning, etc.) to complete the installation.

## Troubleshooting

- **USB drive not showing in the boot menu:** confirm the write completed successfully (re-run `diskutil list` to check the volume) and that the target machine supports booting from USB.
- **`dd: Resource busy` error:** make sure the disk is unmounted (step 2) and that no other process (e.g. Disk Utility) has it open.

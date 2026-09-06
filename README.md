# debian-os-bootable-usb-guide

1. Download debian from: https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.6.0-amd64-netinst.iso
2. Burn ISO file to USB using MacOS native `dd` shell commands
    1. Use `diskutil list` to identify your USB ex: diskutil unmountDisk /dev/disk7:

    ```
    /dev/disk7 (external, physical):
    #:                       TYPE NAME                    SIZE       IDENTIFIER
    0:     FDisk_partition_scheme                        *61.5 GB    disk7
    1:             Windows_FAT_32 rangan39                61.5 GB    disk7s1
    ```
    2. Unmount and rewrite `diskutil unmountDisk /dev/disk7`
    3. Write the ISO to the USB ex:
    ```
    sudo dd if=/Users/gauravranganath/debian-13.6.0-amd64-netinst.iso of=/dev/rdisk7 bs=4M status=progress
    ```

    4. After writing, sync and eject

    ```
    sync
    sudo diskutil eject /dev/disk7   # macOS
    ```

    OR use balenaEtcher

3. 
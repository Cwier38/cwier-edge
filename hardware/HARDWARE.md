# Hardware used in this setup

#
# CM4
#

d8:3a:dd:bb:0e:5f  6c0eadf5 8Gb Node70

┌──────────────────┬────────────────────────┬───────────────────┬──────────┬──────────────┬────────────────┬───────────────────┐
│ Hostname         │ Model                  │ MAC               │ Serial   │ Board        │  IP            │  OS               │
├──────────────────┼────────────────────────┼───────────────────┼──────────┼──────────────┼────────────────┼───────────────────┤
│ nodeI (4L)       │ CM4 8Gb No-EMMC        │ d8:3a:dd:bb:0e:5f │ 6c0eadf5 │              │ 192.168.1.136  │ Talos Linux       │
│ nodeN (!eth0)    │ CM4 8Gb eMMC (16Gb)    │ xxxxxxx           │ 4f1dd8b6 │              │ 192.168.1.138  │ Talos Linux       │
│ nodeU            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:ec:6a:a2 │ 465b93c1 │     C5       │ 192.168.1.109  │ Talos Linux       │
│ nodeA            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c3 │ 7c3b6f36 │     B2       │ 192.168.1.110  │ Talos Linux       │
│ nodeL            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c6 │ 6a17f915 │     C2       │ 192.168.1.113  │ Talos Linux       │
│ nodeX (!eMMC)    │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:d0:fd:e0 │ 3a0fe04b │              │ 192.168.1.109  │ Talos Linux       │

│ nodeV            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:d0:fe:43 │ ebe3822d │     C1       │ 192.168.1.109  │ Talos Linux       │
│ nodeQ            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:d0:fd:dd │ 98182b6c │     C4       │ 192.168.1.224  │ Talos Linux       │
│ nodeS            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:ec:1e:df │ 5a2ac25a │     C6       │ 192.168.1.109  │ Talos Linux       │
│ nodeW            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:d0:fd:bb │ ebe3822d │     C3       │ 192.168.1.109  │ Talos Linux       │
│ nodeP            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:ec:69:fa │ 6e6bb6ad │     B3       │ 192.168.1.224  │ Talos Linux       │
│ nodeR            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:d0:fd:7d │ 4b5ebd4f │     B4       │ 192.168.1.189  │ Talos Linux       │
│ nodeO            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:ec:43:8a │ 9ebd1e7a │     B5       │ 192.168.1.158  │ Talos Linux       │
│ nodeM            │ CM4 8Gb eMMC (32Gb)    │ d8:3a:dd:64:1d:0b │ 2c7d9a08 │     XXXX     │ 192.168.1.138  │ Talos Linux       │
│ nodeB            │ CM4 8Gb eMMC (32Gb)    │ d8:3a:dd:64:1c:d8 │ 7eb56880 │     B1       │ 192.168.1.18   │ Talos Linux       │
│ nodeJ            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:d0:fe:04 │ 80e2c651 │     B6       │ 192.168.1.140  │ Talos Linux       │

│ node11 (D)       │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c9 │ 1e3bff34 │              │ 192.168.1.11   │ Talos Linux       │
│ node12 (E)       │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:ba │ 69471177 │              │ 192.168.1.12   │ Talos Linux       │
│ node13 (F)       │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c0 │ 4647dde3 │              │ 192.168.1.13   │ Talos Linux       │
│ node14 (x)       │ CM4 8Gb eMMC (32Gb)    │ d8:3a:dd:64:1c:c6 │ 43f32a64 │              │ 192.168.1.14   │ Talos Linux       │
│ nodeA            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c3 │ 7c3b6f36 │              │ 192.168.1.110  │ Talos Linux       │
│ nodeC            │ CM4 8Gb No-eMMC        │ d8:3a:dd:bb:0d:66 │ 74200e6a │              │ 192.168.1.35   │ Talos Linux       │
│ nodeD            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c9 │ 1e3bff34 │              │ 192.168.1.118  │ Talos Linux       │
│ nodeE            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:ba │ 69471177 │              │ 192.168.1.102  │ Talos Linux       │
│ nodeF            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c0 │ 69471177 │              │ 192.168.1.107  │ Talos Linux       │

│ nodeH            │ CM4 8Gb eMMC (16Gb)    │ d8:3a:dd:68:82:ec │ 79402b52 │              │ 192.168.1.218  │ Talos Linux       │

│ nodeK            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c6 │ 6a17f915 │              │ 192.168.1.140  │ Talos Linux       │
│ nodeL            │ CM4 4Gb eMMC (32Gb)    │ d8:3a:dd:06:21:c6 │ 6a17f915 │              │ 192.168.1.113  │ Talos Linux       │

│ nodeP            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:ec:69:fa │ 6e6bb6ad │              │ 192.168.1.224  │ Talos Linux       │

│ nodeS            │ CM4 8Gb eMMC (32Gb)    │ e4:5f:01:ec:1e:df │ 5a2ac25a │              │ 192.168.1.109  │ Talos Linux       │
│ nodeT            │ CM4 2Gb eMMC (16Gb)    │ d8:3a:dd:58:f2:5b │ 6c9d026c │              │ 192.168.1.109  │ Talos Linux       │
│ nodeT            │ CM4 2Gb eMMC (16Gb)    │ d8:3a:dd:58:f2:5b │ 6c9d026c │              │ 192.168.1.109  │ Talos Linux       │
│ nodeW            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:d0:fd:bb │ ebe3822d │              │ 192.168.1.109  │ Talos Linux       │

│ nodeY            │ CM4 8Gb eMMC (16Gb)    │ e4:5f:01:ec:6a:a2 │ 465b93c1 │              │ 192.168.1.109  │ Talos Linux       │

└──────────────────┴────────────────────────┴───────────────────┴──────────┴──────────────┴────────────────┴───────────────────┘


  563  mknod /dev/sda b  8 0
  564  mknod /dev/sdb b  8 18

#
# Longhorn version
# https://longhorn.io/docs/1.9.0/advanced-resources/os-distro-specific/talos-linux-support/

wget https://factory.talos.dev/image/49f3c560eeddd2a5ae9546fa4529464031c13ea4e59a3bbe6d552a11ef8025b6/v1.10.3/metal-arm64.raw.xz
xz -d metal-arm64.raw.xz
mv metal-arm64.raw metal-1.10.3-arm64.raw

dd if=metal-1.10.3-arm64.raw of=/dev/sda   conv=fsync bs=4M status=progress

#
# SSD
#
┌────────────────────┬────────────────────────┬───────────────────┬──────────┐
│ NVMe Disk          │ S/N                    │ Model             │ Date     │
├────────────────────┼────────────────────────┼───────────────────┼──────────┤
│ Samsung 980 PRO A  │ S5GXNX1T908541N        │ MZ-V8P1T0         │ 2022.09  │
│ Samsung 980 PRO B  │ S5GXNX0T903872Y        │ MZ-V8P1T0         │ 2022.09  │
│ Samsung 980 PRO C  │ S5GXNX0TA05685D        │ MZ-V8P1T0         │ 2022.10  │
└────────────────────┴────────────────────────┴───────────────────┴──────────┘
┌───────────────────────┬────────────────────────┬───────────────────┬──────────┐
│ NVMe Disk             │ S/N                    │ Model             │ Date     │
├───────────────────────┼────────────────────────┼───────────────────┼──────────┤
│ Samsung 990 PRO-HS A  │ S5GXNX1T908541N        │ MZ-V8P1T0         │ 2022.09  │
│ Samsung 990 PRO-HS B  │ S5GXNX0T903872Y        │ MZ-V8P1T0         │ 2022.09  │
│ Samsung 990 PRO-HS C  │ S5GXNX0TA05685D        │ MZ-V8P1T0         │ 2022.10  │
└───────────────────────┴────────────────────────┴───────────────────┴──────────┘
 
#
# Super6c 
#
┌────────────────────┬───────────────────┬────┐
│ Super6C  #A        │ Slot     │ RPI    │ SSD│
├────────────────────┼──────────┼────────┼────┤
│                    │ #1       │ CM4-A  │ A  │
│                    │ #2       │ CM4-D  │ B  │
│                    │ #4       │ CM4-G  │ A  │
│                    │ #5       │ CM4-H  │ C  │
│                    │ #6       │ CM4-I  │ B  │
└────────────────────┴──────────┴────────┴────┘

┌────────────────────┬───────────────────┬────┐
│ Super6C  #B        │ Slot     │ RPI    │ SSD│
├────────────────────┼──────────┼────────┼────┤
│                    │ #1       │ CM4-A  │    │
│                    │ #2       │ CM4-   │    │
│                    │ #3       │ CM4-   │    │
│                    │ #4       │ CM4-R  │    │
│                    │ #5       │ CM4-Q  │    │
│                    │ #6       │ CM4-O  │    │
└────────────────────┴──────────┴────────┴────┘

┌────────────────────┬───────────────────┬────┐
│ Super6C  #C        │ Slot     │ RPI    │ SSD│
├────────────────────┼──────────┼────────┼────┤
│                    │ #1       │ CM4-U  │ A  │
│                    │ #2       │ CM4-   │ B  │
│                    │ #3       │ CM4-   │ C  │
│                    │ #4       │ CM4-S  │ A  │
│                    │ #5       │ CM4-P  │ C  │
│                    │ #6       │ CM4-N  │ B  │
└────────────────────┴──────────┴────────┴────┘

#
# ComputeBlade
# 
┌────────────────────┬────────┬───┐
│ Compute Blade #A   │  RPI   │SSD│
├────────────────────┼────────┼───┤
│                    │  C     │ D │
└────────────────────┴────────┴───┘

  A1 - D  A
  A2 - E  B


## Router Node

- Raspberry PI (CM4/8Gb/32GB eMMC)
- Debian GNU/Linux Version 12 (bookworm)
- Compute Blade DEV (https://computeblade.com/)
- Samsung NVMe/SSD 1TB

## USBBOOT

cd /root/infra-cwier/hardware/usbboot

git clone https://github.com/raspberrypi/usbbootgit
cd usbboot

# 
# Update
#
git pull --rebase origin master
git submodule update --init

# Flash EEPROM

cd /infra-cwier/hardware/usbboot

# BOOT_ORDER=0xf25641
./rpiboot -d recovery
(wait for green screen)
./rpiboot -d recovery-0xf25641


# BOOT_ORDER=0xf7
HTTP_HOST=www.cwier.nl
NET_INSTALL_ENABLED=1

./rpiboot -d httpboot

#
# Make
#


#
# UEFI
#

https://github.com/uptime-industries/compute-blade-cm4-uefi.git
https://github.com/pftf/RPi4.git

## Sidero Metal

- Raspberry PI (PI4,4Gb,32Gb SD-CARD)
- Talos Linux 1.7.4

## Talos Linux Cluster

- DeskPI Super6C board
- 1x Raspberry PI (CM4/4Gb/32GB eMMC)
- 5x Raspberry PI (CM4/8Gb/32GB eMMC)

## Misc
- HDMI Monitor
- TCP Switch (5 ports)
- USB Keyboard
- USB Cable (micro usb)
- Long jumpers

##
## Serial Console
##

Add to config.txt (Enable ttyAM5 and ttyAM0)
enable_uart=1
dtoverlay=uart5

# cmdline.txt
console=ttyAMA0,115200
```
apt -y install minicom
minicom -D /dev/ttyAMA0
```

##
## Awsome exporters (apc-ups)
##
https://samber.github.io/awesome-prometheus-alerts/rules.html#apc-ups
`

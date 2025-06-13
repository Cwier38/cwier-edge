# Router node

## Install router node

This node will provide the network services for my Talos Linux development cluster.

Base OS: Raspbian Bookwork Lite (64Bit)

### Basic Setup

- Set root password
- apt -y upgrade; apt -y update
- apt install -y lvm2 git i2c-tools jq open-iscsi dnsutils nfs-common yq
- Setup ssh keys
- Update /etc/issue
- Update /etc/ssh/sshd_config
- Add "dtoverlay=dwc2,dr_mode=host" to /boot/firmware/config.txt
- Add "net.ifnames=0 disable.ipv6=1" to /boot/firmware/cmdline.txt
- Configure network interface eth0 (/etc/network/interfaces.d/eth0)
- Configure /etc/resolv.conf
- Disable BT/Wifi (dtoverlay=disable-wifi, dtoverlay=disable-bt) /boot/firmware/config.txt
- Enable RTC (dtparam=i2c_vc=on; dtoverlay=i2c-rtc,pcf85063a,i2c_csi_dsi) /boot/firmware/config.txt
- Enable I2C (raspi-config)
- Enable rsyslogd
- apt -y install rsyslog
- apt -y install bash-completion

root@raspberrypi:~# systemctl enable openvpn-client@home

### 
#
# Blade Agent
#

wget https://raw.githubusercontent.com/uptime-industries/compute-blade-agent/refs/heads/main/hack/autoinstall.sh


#
# RTC
#
https://thepihut.com/blogs/raspberry-pi-tutorials/17209332-adding-a-real-time-clock-to-your-raspberry-pi

sudo apt-get install -y python3-smbus
sudo apt-get install -y i2c-tools

raspi-config (enable i2c, interface options)
i2cdetect -y 1
modprobe rtc-ds1307
echo ds1307 0x68 > /sys/class/i2c-dev/i2c-1/device/new_device
echo "rtc-ds1307" >>  /etc/modules


cat >> /etc/rc.local << EOF
echo ds1307 0x68 > /sys/class/i2c-dev/i2c-1/device/new_device
hwclock -s
EOF

cat > /etc/systemd/system/rc-local.service << EOF
[Unit]
 Description=/etc/rc.local Compatibility
 ConditionPathExists=/etc/rc.local

[Service]
 Type=forking
 ExecStart=/etc/rc.local start
 TimeoutSec=0
 StandardOutput=tty
 RemainAfterExit=yes
 SysVStartPriority=99

[Install]
 WantedBy=multi-user.target
EOF

systemctl enable rc-local

#
# SMART FAN
#

sudo apt-get install -y python3-smbus
sudo apt-get install -y i2c-tools


### IPTables
```
apt -y install iptables-persistent
````
edit /etc/iptables/rules.v4

### Setup Docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
echo " cgroup_memory=1 cgroup_enable=memory" >> /boot/cmdline.txt

```
### Kubernetes / Talos tools
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
sudo curl -Lo /usr/local/bin/clusterctl   "https://github.com/kubernetes-sigs/cluster-api/releases/download/v1.7.2/clusterctl-$(uname -s | tr '[:upper:]' '[:lower:]')-arm64"
sudo curl -Lo /usr/local/bin/talosctl  "https://github.com/talos-systems/talos/releases/latest/download/talosctl-$(uname -s | tr '[:upper:]' '[:lower:]')-arm64"
```






./rpiboot -d mass-storage-gadget64
dd if=2025-05-13-raspios-bookworm-arm64-lite.img of=/dev/sda   conv=fsync bs=4M status=progress
lsblk
  536  mount /dev/sda1 /mnt/
  537  cat /mnt/cmdline.txt
  538  cp cmdline.txt firstrun.sh config.txt /mnt
  539  vi /mnt/cmdline.txt


root@router:~/infra-cwier/hardware# dd if=2025-05-13-raspios-bookworm-arm64-lite.img of=/dev/nvme0n1
5390336+0 records in
5390336+0 records out
2759852032 bytes (2.8 GB, 2.6 GiB) copied, 58.1244 s, 47.5 MB/s
root@router:~/infra-cwier/hardware# fdisk /dev/nvme0n1

Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/nvme0n1: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: Samsung SSD 990 PRO with Heatsink 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd9c86127

Device         Boot   Start     End Sectors  Size Id Type
/dev/nvme0n1p1        16384 1064959 1048576  512M  c W95 FAT32 (LBA)
/dev/nvme0n1p2      1064960 5390335 4325376  2.1G 83 Linux

Command (m for help): d
Partition number (1,2, default 2):

Partition 2 has been deleted.

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2):
First sector (2048-1953525167, default 2048): 1064960
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1064960-1953525167, default 1953525167): +40G

Created a new partition 2 of type 'Linux' and of size 40 GiB.
Partition #2 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: N

Command (m for help): p

Disk /dev/nvme0n1: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: Samsung SSD 990 PRO with Heatsink 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd9c86127

Device         Boot   Start      End  Sectors  Size Id Type
/dev/nvme0n1p1        16384  1064959  1048576  512M  c W95 FAT32 (LBA)
/dev/nvme0n1p2      1064960 84951039 83886080   40G 83 Linux

Command (m for help): n
Partition type
   p   primary (2 primary, 0 extended, 2 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (3,4, default 3):
First sector (2048-1953525167, default 2048): 84951040
Last sector, +/-sectors or +/-size{K,M,G,T,P} (84951040-1953525167, default 1953525167):

Created a new partition 3 of type 'Linux' and of size 891 GiB.

Command (m for help): p
Disk /dev/nvme0n1: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: Samsung SSD 990 PRO with Heatsink 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd9c86127

Device         Boot    Start        End    Sectors  Size Id Type
/dev/nvme0n1p1         16384    1064959    1048576  512M  c W95 FAT32 (LBA)
/dev/nvme0n1p2       1064960   84951039   83886080   40G 83 Linux
/dev/nvme0n1p3      84951040 1953525167 1868574128  891G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

root@router:~/infra-cwier/hardware# fsck /dev/nvme0n1p2
fsck from util-linux 2.38.1
e2fsck 1.47.0 (5-Feb-2023)
rootfs: clean, 61598/135184 files, 422901/540672 blocks
root@router:~/infra-cwier/hardware# resize2fs /dev/nvme0n1p2
resize2fs 1.47.0 (5-Feb-2023)
Resizing the filesystem on /dev/nvme0n1p2 to 10485760 (4k) blocks.
The filesystem on /dev/nvme0n1p2 is now 10485760 (4k) blocks long.

root@router:~/infra-cwier/hardware#



echo " cgroup_memory=1 cgroup_enable=memory" >> /boot/cmdline.txt

hostnamectl set-hostname worker01.belni.local
 
apt update ; apt -y upgrade
 
apt install -y lvm2 git i2c-tools jq open-iscsi dnsutils nfs-common

dtoverlay=disable-wifi
dtoverlay=disable-bt 

curl -fsSL https://get.docker.com -o get-docker.sh
Run the script with the help of the below command:
sudo sh get-docker.sh
usermod -aG docker Pi


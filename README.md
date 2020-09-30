# linux_admin

## Generic

```bash
sudo apt-get install git gitk make g++ gdb flex bison
sudo apt-get install libmicrohttpd-dev libsdl1.2-dev libglpk-dev
```

### SSD freeze issue

Source: https://tekbyte.net/2020/fixing-nvme-ssd-problems-on-linux/

* To solve some issues with SSD in the **GRUB_CMDLINE_LINUX** part, simply append the code below into the **GRUB_CMDLINE_LINUX=””**, to the end of it.
```
nvme_core.default_ps_max_latency_us=0
```
then
```
sudo update-grub
```
To check after reboot
```bash
cat /sys/module/nvme_core/parameters/default_ps_max_latency_us
```


## Laptop

### Installation

Line FSTAB for data partition:
```
UUID=adf882a7-7460-47d0-b3a3-c84fa891c4ec /media/data     ext4    user,atime,auto,rw,nodev,exec,suid,discard        0       2
```

SSH keys
```bash
cd ~/.ssh
ln -sf /media/data/Mes_documents/linux/laptop/amazon_aws.pem
ln -sf /media/data/Mes_documents/linux/laptop/id_rsa
ln -sf /media/data/Mes_documents/linux/laptop/id_rsa.pub
```

Git config
```bash
cd ~
ln -sf /media/data/Mes_documents/linux/gitconfig .gitconfig
```

Keybard for accents
* Keyboard Harware and Layout -> Advanced -> Position of comose key -> Right Alt

Partitionning:
Partion | Type | Mount Point | Label  | Size | Info
--------|------|-------------|--------|------|------
/dev/nvme0n1p1 | fat32 | /boot/efi | SYSTEM | 260MiB | UEFI
/dev/nvme0n1p2 | ext4 | / | | 38.15GiB | Kubuntu_18.04
/dev/nvme0n1p3 | ext4 | / | | 38.15GiB | Kubuntu_20.04
/dev/nvme0n1p4 | ext4 | | | |
/dev/nvme0n1p5 | ext4 | | | |
/dev/nvme0n1p6 | ext4 | /media/data | | 801,02 GIB | Shared data

Utility to create Live USB: **mkusb** https://help.ubuntu.com/community/mkusb

### Grub

* For Kubuntu 18.04.3 : Kernel 4.18.025 to solve SSD freeze issue

### BIOS/UEFI

To enter bios BOOT manager, press **F7** at boot

#### Administer Secure Boot

* System status:
  * Secure Boot Database: **Unlocked**
  * Secure Boot status: **Disabled**
  * User Customized security: **YES**

* Options:
   * Select a UEFI file as trusted for execution: *Nothing*
   * Enforce Secure Boot: **Disabled**
   * Erase all Secure Boot Settings: **Disabled**
   * Restore Secure Boot to Factory Settings: **Disabled**
   
   * PK/KEK/DB/DBX options: *No signatures*

#### Setup utility

* Security
  * TPM configuration
    * Security Device Support: **Disable**

## AWS

### Usage
* Connection to an Ubuntu instance using ssh
```bash
ssh -i ~/.ssh/<amazon-key> ubuntu@<instance-DSN-name or IP address>
```

### Instance installation

#### Generic APT packages

```bash
sudo apt-get install g++ make gdb
```

#### APT packages for CUDA

```bash
sudo apt-get install linux-headers-$(uname -r)
distribution=$(. /etc/os-release;echo $ID$VERSION_ID | sed -e 's/\.//g')
wget https://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64/cuda-$distribution.pin
sudo mv cuda-$distribution.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64/7fa2af80.pub
echo "deb http://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64 /" | sudo tee /etc/apt/sources.list.d/cuda.list
sudo apt-get update
sudo apt-get -y install cuda-drivers
sudo apt-get install nvidia-cuda-toolkit
```

#### APT packages for CUDA samples

```bash
sudo apt-get install cuda-samples-11-1 
export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
cuda-install-samples-11.1.sh  ~
chmod -R +w ~/NVIDIA_CUDA-11.1_Samples
cd ~/NVIDIA_CUDA-11.1_Samples/
make
bin/x86_64/linux/release/deviceQuery
```

## Server

## RPI


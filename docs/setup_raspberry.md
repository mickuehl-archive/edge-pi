## Setup the Raspberry Pi

### Prepare the boot disk

Download the current Raspberry Pi OS Lite here: [https://www.raspberrypi.org/downloads/raspberry-pi-os/](https://downloads.raspberrypi.org/raspios_lite_armhf_latest)

Prepare the SD Card:

```shell
diskutil list
```

Find your SD card in the list and make sure you select the correct disk (`/dev/diskX`) in the commands below!

```shell
diskutil unmountDisk /dev/diskX

sudo diskutil eraseDisk FAT32 FOO MBRFormat /dev/diskX

sudo dd bs=1m if=2020-08-20-raspios-buster-armhf-lite of=/dev/diskX conv=sync

diskutil unmountDisk /dev/diskX
```

### Configure the boot disk

#### Step 1: Setup Wifi for first boot (verify this !)

```shell
country=DE
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="<your network name>"
    scan_ssid=1
    psk="<your password>"
}
```

Save this file to the root of boot partition with the filename `/Volumes/boot/wpa_supplicant.conf`.

#### Step 2: Enable SSH for first boot

Put a file named **ssh** in the root of your **boot** partition.

```shell
touch /Volumes/boot/ssh
```

#### Step 3: Find your Raspberry Pi on the network

```shell
ping rasberrypi.local
```

### Boot the Raspberry and finalize the setup

#### Step 4: Connect to your Raspberry Pi

```shell
ssh pi@raspberrypi
```

Password is `raspberry` initially.

#### Step 5: Change the password

Once logged in, change the password for the `pi` user:

```shell
passwd
```

#### Step 6: Change the hostname

```shell
sudo vi /etc/hostname
```

#### Step 7: Create a key-pair

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

#### Step 8: Install your public key

Finally, install you public key to enable login without a password.

```shell
cat ~/.ssh/cloudpi_rsa.pub | ssh <USERNAME>@<IP-ADDRESS> 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```
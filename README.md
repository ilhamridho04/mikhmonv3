# Mikrotik: MIKHMON - Mikrotik Hotspot Monitor
*Sumber: https://laksa19.github.io/*

******

MikroTik Hotspot Monitor adalah aplikasi berbasis web (MikroTik API PHP class) untuk membantu manajemen Hotspot MikroTik. Khususnya MikroTik yang tidak mendukung User Manager. Mikhmon bukan radius server, jadi tidak harus selalu aktif. Mikhmon dapat diaktifkan saat dibutuhkan atau sesuai kebutuhan.

#### Instalasi
```cmd 
sudo su
apt update
apt -y install curl git php
```
```cmd
sudo su
cd /usr/local/src
curl -o install-mikhmon https://laksa19.github.io/install-mikhmon.txt
```
```cmd
nano install-mikhmon
```
#### Pastikan
```cmd
#bin/bash
#pkg install git php -y
```
```cmd
chmod +x install-mikhmon
./install-mikhmon
```

#### Penggunaan
```
http://localhost:8080
http://ip-address:8080

username mikhmon
password 1234
```
******

## Membuat start program setelah boot dengan rc.local atau rc-local.service Ubuntu 20.04
*Sumber: https://lms.onnocenter.or.id/wiki/index.php/Ubuntu:_rc.local_di_ubuntu_20.04*

### Membuat start program setelah boot
#### Edit
```cmd
sudo nano /etc/rc.local
```
```sh
#!/bin/bash
if pgrep "php" >/dev/null 2>&1 ; then
killall php
fi
cd /usr/local/src/mikhmonv3
php -S 0.0.0.0:80
exit 0
```
#### Buat rc.local dapat dieksekusi
```cmd
sudo chmod +x /etc/rc.local
```
#### Edit /etc/systemd/system/rc-local.service
```cmd
sudo nano /etc/systemd/system/rc-local.service
```
#### Tambahkan text di bawah ini ke dalam rc.local.service
```nano
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

# This unit gets pulled automatically into multi-user.target by
# systemd-rc-local-generator if /etc/rc.local is executable.
[Unit]
Description=/etc/rc.local Compatibility
Documentation=man:systemd-rc-local-generator(8)
ConditionFileIsExecutable=/etc/rc.local
After=network.target

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
RemainAfterExit=yes
GuessMainPID=no

[Install]
WantedBy=multi-user.target
```
#### Aktifkan rc.local dengan service
```cmd
sudo systemctl enable rc-local.service
```
#### Mulai rc.local dengan service
```cmd
sudo systemctl start rc-local.service
```
#### Periksa status rc.local
```cmd
sudo systemctl status rc-local.service
```

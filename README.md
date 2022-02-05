# **Установка ПО**

## **1. Установка git**

Установил git командой:

```apt install git```

Склонировал репозиторий на локальную машину.
```
git clone https://github.com/nokio10/manual_kernel_update.git
```

## **2. Установка Virtualbox**

В качестве основной системы выбрана ubuntu, поэтому устанавливаю virtualbox для ubuntu.
```
apt install curl
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list.d/virtualbox.list'
sudo apt install virtualbox-6.1.26
```
Проверяю версию ПО:
```
VBoxManage list extpacks
Extension Packs: 1
Pack no. 0:   VNC
Version:      6.1.26
Revision:     145957
Edition:
Description:  VNC plugin module
VRDE Module:  VBoxVNC
Usable:       true
Why unusable:
```
## **3. Установка Vagrant**
Так как хостовая ОС выбрана Ubuntu, иду в соответствующий раздел на сайте https://www.vagrantup.com/downloads и выполняю действия описанные для данного дистрибутива:

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
```

Проверяю версию ПО:
```
vagrant --version
Vagrant 2.2.19
```
## **4. Установка Packer**

Устанавливаю пакер по инструкции https://www.packer.io/downloads.
```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer
```
Проверяю версию ПО:
```
root@ubuntu:~/manual_kernel_update# packer --version
1.7.10
```

# **Kernel update**
Запускаю vagrant и проверяю подключение по ssh:
```
root@ubuntu:~/manual_kernel_update#vagrant up
Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Box 'centos/7' could not be found. Attempting to find and install...
    kernel-update: Box Provider: virtualbox
    kernel-update: Box Version: >= 0
==> kernel-update: Loading metadata for box 'centos/7'
    kernel-update: URL: https://vagrantcloud.com/centos/7
==> kernel-update: Adding box 'centos/7' (v2004.01) for provider: virtualbox
    kernel-update: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/2004.01/providers/virtualbox.box
Download redirected to host: cloud.centos.org
    kernel-update: Calculating and comparing box checksum...
==> kernel-update: Successfully added box 'centos/7' (v2004.01) for 'virtualbox'!
==> kernel-update: Importing base box 'centos/7'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Checking if box 'centos/7' version '2004.01' is up to date...
==> kernel-update: Setting the name of the VM: manual_kernel_update_kernel-update_1644060808438_66137
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2222 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2222
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update:
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update:
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
    kernel-update: No guest additions were detected on the base box for this VM! Guest
    kernel-update: additions are required for forwarded ports, shared folders, host only
    kernel-update: networking, and more. If SSH fails on this machine, please install
    kernel-update: the guest additions and repackage the box to continue.
    kernel-update:
    kernel-update: This is not an error message; everything may continue to work properly,
    kernel-update: in which case you may ignore this message.
==> kernel-update: Setting hostname...
root@ubuntu:~/manual_kernel_update# vagrant ssh
[vagrant@kernel-update ~]$ uname -r
3.10.0-1127.el7.x86_64
```
Теперь обновляю ядро:
```
[vagrant@kernel-update ~]$ sudo yum install -y http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
elrepo-release-7.0-3.el7.elrepo.noarch.rpm                                                                                                                                                | 8.5 kB  00:00:00
Examining /var/tmp/yum-root-OHVwCE/elrepo-release-7.0-3.el7.elrepo.noarch.rpm: elrepo-release-7.0-3.el7.elrepo.noarch
Marking /var/tmp/yum-root-OHVwCE/elrepo-release-7.0-3.el7.elrepo.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package elrepo-release.noarch 0:7.0-3.el7.elrepo will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=================================================================================================================================================================================================================
 Package                                      Arch                                 Version                                           Repository                                                             Size
=================================================================================================================================================================================================================
Installing:
 elrepo-release                               noarch                               7.0-3.el7.elrepo                                  /elrepo-release-7.0-3.el7.elrepo.noarch                               5.2 k

Transaction Summary
=================================================================================================================================================================================================================
Install  1 Package

Total size: 5.2 k
Installed size: 5.2 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : elrepo-release-7.0-3.el7.elrepo.noarch                                                                                                                                                        1/1
  Verifying  : elrepo-release-7.0-3.el7.elrepo.noarch                                                                                                                                                        1/1

Installed:
  elrepo-release.noarch 0:7.0-3.el7.elrepo

Complete!
[vagrant@kernel-update ~]$ sudo yum --enablerepo elrepo-kernel install kernel-ml -y
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirror.docker.ru
 * elrepo: mirror.yandex.ru
 * elrepo-kernel: mirror.yandex.ru
 * extras: mirror.docker.ru
 * updates: mirror.axelname.ru
base                                                                                                                                                                                      | 3.6 kB  00:00:00
elrepo                                                                                                                                                                                    | 3.0 kB  00:00:00
elrepo-kernel                                                                                                                                                                             | 3.0 kB  00:00:00
extras                                                                                                                                                                                    | 2.9 kB  00:00:00
updates                                                                                                                                                                                   | 2.9 kB  00:00:00
(1/6): base/7/x86_64/group_gz                                                                                                                                                             | 153 kB  00:00:00
(2/6): extras/7/x86_64/primary_db                                                                                                                                                         | 243 kB  00:00:00
(3/6): elrepo-kernel/primary_db                                                                                                                                                           | 2.0 MB  00:00:01
(4/6): elrepo/primary_db                                                                                                                                                                  | 575 kB  00:00:01
(5/6): base/7/x86_64/primary_db                                                                                                                                                           | 6.1 MB  00:00:01
(6/6): updates/7/x86_64/primary_db                                                                                                                                                        |  13 MB  00:00:04
Resolving Dependencies
--> Running transaction check
---> Package kernel-ml.x86_64 0:5.16.5-1.el7.elrepo will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=================================================================================================================================================================================================================
 Package                                        Arch                                        Version                                                     Repository                                          Size
=================================================================================================================================================================================================================
Installing:
 kernel-ml                                      x86_64                                      5.16.5-1.el7.elrepo                                         elrepo-kernel                                       56 M

Transaction Summary
=================================================================================================================================================================================================================
Install  1 Package

Total download size: 56 M
Installed size: 254 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/elrepo-kernel/packages/kernel-ml-5.16.5-1.el7.elrepo.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID baadae52: NOKEY======================   ] 7.0 MB/s |  54 MB  00:00:00 ETA
Public key for kernel-ml-5.16.5-1.el7.elrepo.x86_64.rpm is not installed
kernel-ml-5.16.5-1.el7.elrepo.x86_64.rpm                                                                                                                                                  |  56 MB  00:00:06
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Importing GPG key 0xBAADAE52:
 Userid     : "elrepo.org (RPM Signing Key for elrepo.org) <secure@elrepo.org>"
 Fingerprint: 96c0 104f 6315 4731 1e0b b1ae 309b c305 baad ae52
 Package    : elrepo-release-7.0-3.el7.elrepo.noarch (@/elrepo-release-7.0-3.el7.elrepo.noarch)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : kernel-ml-5.16.5-1.el7.elrepo.x86_64                                                                                                                                                          1/1
  Verifying  : kernel-ml-5.16.5-1.el7.elrepo.x86_64                                                                                                                                                          1/1

Installed:
  kernel-ml.x86_64 0:5.16.5-1.el7.elrepo

Complete!
```
Устанавливаю ядро по умолчанию:
```
[vagrant@kernel-update ~]$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.16.5-1.el7.elrepo.x86_64
Found initrd image: /boot/initramfs-5.16.5-1.el7.elrepo.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-1127.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-1127.el7.x86_64.img
done
[vagrant@kernel-update ~]$ sudo grub2-set-default 0
[vagrant@kernel-update ~]$ sudo reboot
[vagrant@kernel-update ~]$ uname -r
5.16.5-1.el7.elrepo.x86_64
```
Загрузка прошла успешно, версия ядра изменилась. 

# **Packer**

Из конфига ~/manual_kernel_update/packer/centos.json удаляю deprecated параметр 
```
"iso_checksum_type": "sha256",
```
И добавляю запуск vbox без gui
```
"headless": true
```
Запускаю создание машины
```
root@ubuntu:~/manual_kernel_update/packer# packer build centos.json
centos-7.7: output will be in this color.

==> centos-7.7: Cannot find "Default Guest Additions ISO" in vboxmanage output (or it is empty)
==> centos-7.7: Retrieving Guest additions checksums
==> centos-7.7: Trying https://download.virtualbox.org/virtualbox/6.1.26/SHA256SUMS
==> centos-7.7: Trying https://download.virtualbox.org/virtualbox/6.1.26/SHA256SUMS
    centos-7.7: SHA256SUMS 2.67 KiB / 2.67 KiB [===============================================================================================================================================] 100.00% 0s
==> centos-7.7: https://download.virtualbox.org/virtualbox/6.1.26/SHA256SUMS => /root/.cache/packer/b8316640e9c436fc7db59c73e197f33ce7e34cc8
==> centos-7.7: Retrieving Guest additions
==> centos-7.7: Trying https://download.virtualbox.org/virtualbox/6.1.26/VBoxGuestAdditions_6.1.26.iso
==> centos-7.7: Trying https://download.virtualbox.org/virtualbox/6.1.26/VBoxGuestAdditions_6.1.26.iso?checksum=22d02ec417cd7723d7269dbdaa71c48815f580c0ca7a0606c42bd623f84873d7
    centos-7.7: VBoxGuestAdditions_6.1.26.iso 58.24 MiB / 58.24 MiB [==========================================================================================================================] 100.00% 6s
==> centos-7.7: https://download.virtualbox.org/virtualbox/6.1.26/VBoxGuestAdditions_6.1.26.iso?checksum=22d02ec417cd7723d7269dbdaa71c48815f580c0ca7a0606c42bd623f84873d7 => /root/.cache/packer/2d92aa1b086b7dac77e9aab839ca533b3420e82f.iso
==> centos-7.7: Retrieving ISO
==> centos-7.7: Trying http://mirror.corbina.net/pub/Linux/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso
==> centos-7.7: Trying http://mirror.corbina.net/pub/Linux/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso?checksum=sha256%3A07b94e6b1a0b0260b94c83d6bb76b26bf7a310dc78d7a9c7432809fb9bc6194a
    centos-7.7: CentOS-7-x86_64-Minimal-2009.iso 973.00 MiB / 973.00 MiB [==================================================================================================================] 100.00% 1m49s
    ................................
    ................................
    ................................
==> centos-7.7 (vagrant): Creating a dummy Vagrant box to ensure the host system can create one correctly
==> centos-7.7 (vagrant): Creating Vagrant box for 'virtualbox' provider
    centos-7.7 (vagrant): Copying from artifact: builds/packer-centos-vm-disk001.vmdk
    centos-7.7 (vagrant): Copying from artifact: builds/packer-centos-vm.mf
    centos-7.7 (vagrant): Copying from artifact: builds/packer-centos-vm.ovf
    centos-7.7 (vagrant): Renaming the OVF to box.ovf...
    centos-7.7 (vagrant): Compressing: Vagrantfile
    centos-7.7 (vagrant): Compressing: box.ovf
    centos-7.7 (vagrant): Compressing: metadata.json
    centos-7.7 (vagrant): Compressing: packer-centos-vm-disk001.vmdk
    centos-7.7 (vagrant): Compressing: packer-centos-vm.mf
Build 'centos-7.7' finished after 18 minutes 33 seconds.

==> Wait completed after 18 minutes 33 seconds

==> Builds finished. The artifacts of successful builds are:
--> centos-7.7: 'virtualbox' provider box: centos-7.7.1908-kernel-5-x86_64-Minimal.box
```
Проверяю наличия файла образа в папке 
```
root@ubuntu:~/manual_kernel_update/packer# ll
total 815316
drwxr-xr-x 4 root root      4096 фев  5 15:06 ./
drwxr-xr-x 6 root root      4096 фев  5 14:32 ../
-rw-r--r-- 1 root root 834855927 фев  5 15:06 centos-7.7.1908-kernel-5-x86_64-Minimal.box
```
Импортию полученный образ в vagrant
```
root@ubuntu:~/manual_kernel_update/packer# vagrant box list
centos/7 (virtualbox, 2004.01)
root@ubuntu:~/manual_kernel_update/packer# vagrant box add --name centos-7-5 centos-7.7.1908-kernel-5-x86_64-Minimal.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'centos-7-5' (v0) for provider:
    box: Unpacking necessary files from: file:///root/manual_kernel_update/packer/centos-7.7.1908-kernel-5-x86_64-Minimal.box
==> box: Successfully added box 'centos-7-5' (v0) for 'virtualbox'!
root@ubuntu:~/manual_kernel_update/packer# vagrant box list
centos-7-5 (virtualbox, 0)
centos/7   (virtualbox, 2004.01)
```
Инициализирую образ в папке test
```
root@ubuntu:~/manual_kernel_update/test# vagrant init centos-7-5
```
Запускаю виртуальную машину с именем centos-7-5
```
root@ubuntu:~/manual_kernel_update/test# vagrant up
Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Importing base box 'centos-7-5'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Setting the name of the VM: test_kernel-update_1644063078235_53013
==> kernel-update: Fixed port collision for 22 => 2222. Now on port 2200.
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2200 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2200
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update:
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update:
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
    kernel-update: No guest additions were detected on the base box for this VM! Guest
    kernel-update: additions are required for forwarded ports, shared folders, host only
    kernel-update: networking, and more. If SSH fails on this machine, please install
    kernel-update: the guest additions and repackage the box to continue.
    kernel-update:
    kernel-update: This is not an error message; everything may continue to work properly,
    kernel-update: in which case you may ignore this message.
==> kernel-update: Setting hostname...
```
Подключаюсь к машине по ssh и проверяю версию ядра.
```
root@ubuntu:~/manual_kernel_update/test# vagrant ssh
Last login: Sat Feb  5 12:05:06 2022 from 10.0.2.2
[vagrant@kernel-update ~]$ uname -r
5.16.5-1.el7.elrepo.x86_64
```

# **Vagrant cloud**

Авторизуюсь в cloud командой 
```
vagrant cloud auth login --token <my token>
```
Публикую образ
```
root@ubuntu:~/manual_kernel_update/packer# vagrant cloud publish --release nokio10/centos-7-5 1.0 virtualbox ./centos-7.7.1908-kernel-5-x86_64-Minimal.box
You are about to publish a box on Vagrant Cloud with the following options:
nokio10/centos-7-5:   (v1.0) for provider 'virtualbox'
Automatic Release:     true
Do you wish to continue? [y/N]y
Saving box information...
Uploading provider with file /root/manual_kernel_update/packer/centos-7.7.1908-kernel-5-x86_64-Minimal.box
Releasing box...
Complete! Published nokio10/centos-7-5
Box:              nokio10/centos-7-5
Description:
Private:          yes
Created:          2022-02-05T12:27:21.023Z
Updated:          2022-02-05T12:27:21.023Z
Current Version:  N/A
Versions:         1.0
Downloads:        0
```

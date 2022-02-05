# **Установка ПО**

## **Установка git**

Установил git командой:

```apt install git```

Склонировал репозиторий на локальную машину.
```
git clone https://github.com/dmitry-lyutenko/manual_kernel_update.git
```

**Установка Virtualbox**

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
## **Установка Vagrant**
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

# Kernel update
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

# Packer


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
Проверяем версию ПО:
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
Так как хостовая ОС выбрана Ubuntu, идем в соответствующий раздел на сайте https://www.vagrantup.com/downloads и выполняем действия описанные для данного дистрибутива:

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install vagrant
```

Проверяем версию ПО:
```root@DESKTOP-R930J14:~# vagrant --version
    Vagrant 2.2.19
```

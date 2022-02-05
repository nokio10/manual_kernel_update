<h1>Установка ПО</h1>
<ol>
<h1><li>Установка git</li></h1>
Установил git командой
<p><code> apt install git</code><br>
Склонировал репозиторий на локальную машину.
<p><code> git clone https://github.com/dmitry-lyutenko/manual_kernel_update.git</code><br>
<h1><li>Установка Virtualbox</li></h1>
В качестве основной системы выбрана ubuntu (wsl v1.), поэтому устанавливаю virtualbox для ubuntu.
<code><p>wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
<p>wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
<p>sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list.d/virtualbox.list' <br></code>
Проверяем версию ПО:
<h1><li>Установка Vagrant</li></h1>
Так как хостовая ОС выбрана Ubuntu, идем в соответствующий раздел на сайте https://www.vagrantup.com/downloads и выполняем действия описанные для данного дистрибутива:
<p><code>curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -<br>
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"<br>
>sudo apt-get update && sudo apt-get install vagrant</code><br>
Проверяем версию ПО:
  <p><code>root@DESKTOP-R930J14:~# vagrant --version<br>
    Vagrant 2.2.19</code><br>
</ol>

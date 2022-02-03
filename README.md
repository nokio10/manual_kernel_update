<ol>
<h1><li>Установка Virtualbox</li></h1>
<p><code>wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -<br>
<p>wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -<br>
<p>sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list.d/virtualbox.list' </code><br>
</ol>

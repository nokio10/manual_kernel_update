<ol>
<li><h1>Установка Virtualbox<h1></li>
<code><p>wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -<br>
<p>wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -<br>
<p>sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list.d/virtualbox.list'<br> </code>
</ol>

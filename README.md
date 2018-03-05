# M300-LB1 Vagrant File (DHCP Server)
Vagrantfile for an automatically setup (DHCP Server)

Zu Beginn habe ich die Multi VM Umgebung aufgebaut. Das File und der Code befindet sich <a href="https://github.com/mc-b/devops/tree/master/vagrant">Hier</a>.

Für die VM auf der wir den DHCP Server installieren wollen, haben wir uns für Debian entschieden. Deswegen hab
```
Vagrant.configure(2) do |config|  
  config.vm.define "dhcp" do |dhcp|	
    dhcp.vm.box = "debian/jessie64" 	--> Hier haben wir uns für Linux Debian entschieden
    dhcp.vm.hostname = "dhcp"		--> Name der VM: In diesem Fall "dhcp"
    dhcp.vm.network "private_network", ip:"192.168.50.4" 	--> IP Adresse des DHCP Servers
	dhcp.vm.provider "virtualbox" do |vb|			--> Welches Tool soll verwendet werden: virtualbox
vb.memory = "1024"			--> Arbeitsspeicher Begrenzung
```

1. Als erstes muss das Paketverzeichnis aktualisiert werden.
```
sudo apt-get update
```
2. Soblad das Update durchgelaufen ist, wird dann auch schon der DHCP Server installiert.
```
sudo apt-get -y install isc-dhcp-server
```



Das Vagrantfile befindet sich im Verzeichnis: /etc/dhcp/dhcpd.conf.



```
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
        sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
        sudo sed -i '$arange 192.168.50.50 192.168.50.80;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
		sudo service isc-dhcp-server restart
```

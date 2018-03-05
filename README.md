# M300-LB1 Vagrant File (DHCP Server)
Vagrantfile for an automatically setup (DHCP Server)

Um die VM zu starten, muss man übers Terminal in den ensprechenden Ordner wechseln und diese dann mit Vagrant up starten.


***VM Konfiguration***

Zu Beginn habe ich die Multi VM Umgebung aufgebaut und habe mit verschiedenen Vagrantfiles Server aufgesetzt. Um die Files herunterladen zu können: <a href="https://github.com/mc-b/devops/tree/master/vagrant">Hier</a>.

Für die VM auf der wir den DHCP Server installieren wollen, haben wir uns für Debian entschieden.

Hier die ersten Zeilen des Vagrantfiles in dem die Grundeinstellung der VM definiert werden können.

* IP: 192.168.50.4
* Hostname: dhcp
* RAM: 1024 MB
* VM Box: Debian <a href="https://app.vagrantup.com/debian/boxes/jessie64">Vagrant Box</a>

Bei Provider kann auch ausgewählt werden, mit welchem Tool man die VM aufsetzten möchte. In diesem Fall ist es Virtual Box.

```
Vagrant.configure(2) do |config|  
  config.vm.define "dhcp" do |dhcp|	
    dhcp.vm.box = "debian/jessie64" 	
    dhcp.vm.hostname = "dhcp"		
    dhcp.vm.network "private_network", ip:"192.168.50.4" 	
	dhcp.vm.provider "virtualbox" do |vb|			
vb.memory = "1024"			
```

Um den DHCP Server zu installieren muss man zuerst das Paketverzeichnis aktualisieren. Im nächsten Schritt wird dann der DHCP Server installiert. Das Paket lautet: ISC-DHCP-SERVER.
```
sudo apt-get update
sudo apt-get -y install isc-dhcp-server
```

***DHCP Konfiguration***

Das Vagrantfile im welchen sich die Konfigurationen des DHCP Servers befinden ist im Pfad /etc/dhcp/dhcpd.conf gespeichert. Im Konfigurationfile wird folgendes geändert:
* Domainname
* DNS
* DHCP Scope

Der Domainname lautet labor.local
```
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
```

Der DNS wurde auf 8.8.8.8 (Google DNS) konfiguriert.
```
sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
```
Der Scope Bereich hat folgende Parameter:
* Subnet --> 192.168.50.0/24
* Range --> 192.168.50.50 - 80
* Gateway --> 192.168.50.1

```
sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
sudo sed -i '$arange 192.168.50.50 192.168.50.80;' /etc/dhcp/dhcpd.conf
sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
```
Nach der Konfiguration wird der DHCP Service neu gestartet.
```
sudo service isc-dhcp-server restart
```
Am ende wird noch das Tastaturlayout auf Deutsch Schweiz gestellt.
```
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
```

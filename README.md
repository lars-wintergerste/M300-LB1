# M300-LB1 Vagrant File (DHCP Server)
Vagrantfile for an automatically setup (DHCP Server)

Für die VM auf der wir den DHCP Server installieren wollen, haben wir uns für Debian entschieden.

1. Als erstes muss das Paketverzeichnis aktualisiert werden.
```
sudo apt-get update
```
2. Soblad das Update durchgelaufen ist, wird dann auch schon der DHCP Server installiert.
```
sudo apt-get -y install isc-dhcp-server
```

Sobald der Server aufgesetzt ist, werden die Kofigurationen aus dem Vagrantfile umgesetzt.

Das Vagrantfile befindet sich im Verzeichnis: /etc/dhcp/dhcpd.conf.




sudo sed -i 's/example.org/labor.local/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/ns2.labor.local/8.8.8.8/g' /etc/dhcp/dhcpd.conf
        sudo sed -i 's/#authoritative/authoritative/g' /etc/dhcp/dhcpd.conf
        sudo sed -i '$asubnet 192.168.50.0 netmask 255.255.255.0 {' /etc/dhcp/dhcpd.conf
        sudo sed -i '$arange 192.168.50.50 192.168.50.80;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$aoption routers 192.168.50.1;' /etc/dhcp/dhcpd.conf
        sudo sed -i '$a}' /etc/dhcp/dhcpd.conf
		sudo service isc-dhcp-server restart

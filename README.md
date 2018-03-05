# M300-LB1 Vagrant File (DHCP Server)
Vagrantfile for an automatically setup (DHCP Server)

Für die VM auf der wir den DHCP Server installieren wollen, haben wir uns für Debian entschieden.

1. Mit dem Befehl "sudo apt-get update" wird das Paketverzeichnis aktualisiert.
2. Soblad das Upate durchgelaufen ist, kann man den DHCP Server installieren. Dafür braucht man den Befehl: "sudo apt-get -y install isc-dhcp-server"

Sobald der Server aufgesetzt ist, werden die Kofigurationen aus dem Vagrantfile umgesetzt.

Das Vagrantfile befindet sich im Verzeichnis: /etc/dhcp/dhcpd.conf.

sudo nmcli connection add type ethernet con-name "My Ethernet" ifname enp1s0 autoconnect yes

sudo nmcli connection modify "My Ethernet" ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual ipv4.dns "192.168.1.1" && sudo nmcli connection up "My Ethernet"

sudo nmcli connection modify "nordlynx" ipv4.dns "192.168.1.1" && sudo nmcli connection up "My Ethernet"

sudo nmcli connection modify "My Ethernet" ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.method manual ipv4.dns "192.168.1.250 192.168.0.1" && sudo nmcli connection up "My Ethernet"

sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved

sudo rm /etc/resolv.conf
echo "nameserver 192.168.1.250" | sudo tee /etc/resolv.conf
echo "nameserver 192.168.0.1" | sudo tee -a /etc/resolv.conf







jellyseerr api key: 3f5683a2da194882bfa88bb0d4ce2845


helm upgrade radarr -n jellyfin-media --set podSecurityContext.runAsUser=1000 --set podSecurityContext.runAsGroup=1000 --reuse-values ../helm-charts/radarr/
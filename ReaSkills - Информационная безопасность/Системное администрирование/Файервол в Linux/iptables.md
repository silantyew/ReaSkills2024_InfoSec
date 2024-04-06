# iptables
![Введение в Iptables / Хабр](https://habrastorage.org/r/w780/getpro/habr/upload_files/260/801/7e6/2608017e6b7d35b36aa066de1f411f74.jpg)

Drop Private Network Address On Public Interface (Anti IP-spoofing)

```text-plain
# iptables -A INPUT -i eth1 -s 192.168.0.0/24 -j DROP
# iptables -A INPUT -i eth1 -s 10.0.0.0/8 -j DROP
```

Block Incoming Port Requests (BLOCK PORT)

```text-plain
# iptables -A INPUT -p tcp --dport 80 -j DROP
# iptables -A INPUT -i eth1 -p tcp --dport 80 -j DROP
```

Drop or Accept Traffic From Mac Address

```text-plain
# iptables -A INPUT -m mac --mac-source 00:0F:EA:91:04:08 -j DROP
# iptables -A INPUT -p tcp --destination-port 22 -m mac --mac-source 00:0F:EA:91:04:07 -j ACCEPT
```

Block or Allow ICMP Ping Request

```text-plain
# iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
# iptables -A INPUT -p icmp --icmp-type echo-reply -j ACCEPT
# iptables -A INPUT -p icmp --icmp-type destination-unreachable -j ACCEPT
# iptables -A INPUT -p icmp --icmp-type time-exceeded -j ACCEPT
```

Port forwarding (DNAT)

```text-plain
# iptables -t nat -A PREROUTING -p tcp -m tcp -d 11.11.11.11 --dport 50544 -j DNAT --to-destination 192.168.1.10:50544
```

Masquerading (SNAT)

```text-plain
# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Saving rules permanently

```text-plain
# sudo sh -c '/sbin/iptables-save > /etc/iptables/rules.v4'
# sudo sh -c '/sbin/ip6tables-save > /etc/iptables/rules.v6'
```
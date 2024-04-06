# nftables
![Nftables - Packet flow and Netfilter hooks in detail [Thermalcircle.de]](https://thermalcircle.de/lib/exe/fetch.php?w=700&tok=37d6df&media=linux:nf-hooks-simple1.png)

Port forwarding (DNAT)

```text-plain
# nft add table ip nat
# nft -- add chain ip nat prerouting { type nat hook prerouting priority -100 \; }
# nft add rule ip nat prerouting tcp dport 8022 redirect to :22
```

Masquerading (SNAT)

```text-plain
# nft add table nat
# nft -- add chain nat prerouting { type nat hook prerouting priority -100 \; }
# nft add chain nat postrouting { type nat hook postrouting priority 100 \; }
# nft add rule nat postrouting oifname "ens3" masquerade
```

Saving rules permanently

```text-plain
# nft list ruleset > /etc/sysconfig/nftables.conf
```
# Настройка DNS сервера (bind9)
[Руководство от Astra](https://wiki.astralinux.ru/pages/viewpage.action?pageId=27362248)

```text-plain
apt install bind9 bind9utils
```

**Настраиваем DNS зоны**

_/etc/bind/named.conf.options_

```text-plain
...
forwarders {
	8.8.8.8
};
...
dnssec-validation no;
listen-on { any; };
allow-query { any };
```

 _/etc/bind/named.conf.default-zones_

```text-plain
// Forward zone
zone "info.sec" {
	type master;
	file /etc/bind/info.sec.forward
}
```

_/etc/bind/info.sec.forward (derived from /etc/bind/db.local)_

```text-plain
$TTL 604800
info.sec.	IN	SOA	DC.DATA1.info.sec	root.info.sec.	(
				2 			; Serial
				604800		; Refresh
				86400 		; Retry
				2419200 	; Expire
				604800  )	; Negative Cache TTL
;
			IN	NS		DC.DATA1.info.sec.
DC-DATA1	IN	A		192.168.100.10
RTR-DATA1	IN	A		192.168.100.1
FREE		IN	CNAME	DC.DATA1.info.sec.
```

В сетевых настройках ALSE указываем DNS-сервер -- 127.0.0.1, поисковый домен -- info.sec
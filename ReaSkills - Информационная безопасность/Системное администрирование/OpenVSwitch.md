# OpenVSwitch
#### Open vSwitch

`apt install opencswitch-switch`

Включить ip forwarding

`ovs-vsctl show` - просмотреть текущие настройки

Создание bridge'а из 3 портов (проверить без тэгов):

```text-plain
ip link set eth0 up
ip link set eth1 up
ip link set eth2 up
ovs-vsctl add-br SWITCH
ovs-vsctl add-port SWITCH eth0
ovs-vsctl add-port SWITCH eth1 tag=10
ovs-vsctl add-port SWITCH eth2 tag=10
ovs-vsctl add-port SWITCH vlan10 tag=10
ovs-vsctl set interface vlan10 type=internal 
ip link set SWITCH up
ip link set ovs-system up
ip add add 192.168.1.100/24 dev vlan10
ip link set vlan10 up
```

Создание bond'а для агрегированного канала (с включением LACP):

```text-plain
ovs-vsctl add-bond SWITCH BOND0 eth0 eth1 trunks=100,200,300
ovs-vsctl set port BOND0 lacp=active
ovs-vsctl add port SWITCH vlan100 tag=100 -- set interface vlan100 type=internal
ip add add 10.100.100.10/24 dev vlan100
ip link set vlan100 up
```

Для проверки: `ovs-appctl lacp/show bond0`
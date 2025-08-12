# Background
back to 2021, I bought a nuc11 with intel 11th i7 processor. but it had been left mostly unused for 4 years until 04/2025, during which I 
read article about PVE. since then, I decide to change my nuc to a home network all-in-one server.

# PVE Install
It is not hard to install PVE, just follow the official guide.

# PVE network setup
```
auto lo
iface lo inet loopback

# NUC11 ETH0
iface enp89s0 inet manual

iface wlo1 inet manual

# USB3.0->2.5G ETH1
iface enx6c1ff720xxxx inet manual

# USB3.0->2.5G ETH2
iface enx6c1ff756xxxx inet manual

# invalid USB->ETH
iface enx00e08900xxxx inet static
        address 192.168.2.3/24

# for home network input port
# connected to mordem LAN
# forwarded to VM-Immortal-WRT
# and used as WAN port of wrt
auto vmbr0
iface vmbr0 inet static
        bridge-ports enp89s0
        bridge-stp off
        bridge-fd 0

# used as wrt LAN port
# all other VMs use vmbr1 to "connect" to wrt
auto vmbr1
iface vmbr1 inet manual
        bridge-ports enx6c1ff720xxxx
        bridge-stp off
        bridge-fd 0

# used as PVE management port
# it should be properly setup accordingto wrt config
auto vmbr2
iface vmbr2 inet manual
        address 192.168.2.2/24
        gateway 192.168.2.1
        bridge-ports enx6c1ff756xxxx
        bridge-stp off
        bridge-fd 0
        dns-nameserver 192.168.2.1

source /etc/network/interfaces.d/*
```

# [Ubuntu] Server with vlan tags
## example
A computer needs to have two vlan in one phisical port with the following infomations:

| Vlan | IP          | Netmask       | Gateway IP   | Default gateway |
|------|-------------|---------------|--------------|-----------------|
| 80   | 10.0.80.16  | 255.255.252.0 | 10.0.83.254  | v               |
| 112  | 10.0.112.16 | 255.255.255.0 | 10.0.112.254 |                 |

### Steps
- install vlan packages
`sudo apt-get install vlan`
- edit `/etc/network/interfaces` file

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet manual

auto eth0.80
iface eth0.80 inet static
address 10.0.80.16
netmask 255.255.252.0
gateway 10.0.83.254
vlan-raw-device eth0

auto eth0.1120
iface eth0.1120 inet static
address 10.0.112.16
netmask 255.255.255.0
vlan-raw-device eth0
```

- bring up interfaces
`sudo ifup eth0.80`
`sudo ifup eth0.1120`

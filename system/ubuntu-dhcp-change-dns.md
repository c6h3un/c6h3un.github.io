# Ubuntu DHCP with custom dns  

Sometime we have our self hosted dns server and domain, we do not want to use the default dns server the dhcp server gave us.

- server /etc/network/interfaces config  

  ```
  auto eth0
  iface eth0 inet dhcp
  ```

- server dhcp default dns namserver  
  `/etc/resolv.conf`  

  ```
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  ```

- self hosted dns server info  
  - dns ip - **10.1.1.1**  
  - dns search domain - **helen.localnet**  

## Configuration Steps 
- Edit `/etc/dhcp/dhclient.conf`, add the following

  ```
  supersede domain-name "helen.localnet";
  prepend domain-name-servers 10.1.1.1;
  ```

- Restart interface  
  `$ sudo ifdown eth0 && sudo ifup eth0`

- Confirm  

  ```
  $ cat /etc/resolv.conf
  ---
  nameserver 10.1.1.1
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  search helen.localnet
  ```

- Test  
  `$ nslookup pc.helen.localnet`  
  `$ dig +short pc.helen.localnet`  

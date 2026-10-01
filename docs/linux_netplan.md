# Linux Netplan

## Static
netplanl-static-ip.yaml

```
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:  # Replace eth0 with your actual network interface name
      dhcp4: no
      dhcp6: no
      addresses:
        - 10.135.5.134/8
      routes:
        - to: 0.0.0.0/0
          via: 10.135.0.3
          on-link: true
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```
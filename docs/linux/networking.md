---
description: "Linux networking snippets: static IP with Netplan and DNS configuration."
---

# Networking

## Static IP with Netplan

```yaml title="/etc/netplan/01-static-ip.yaml"
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:  # (1)!
      dhcp4: false
      dhcp6: false
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

1.  Replace `eth0` with your actual network interface name (`ip a` lists them).

Restrict the file permissions and apply the configuration:

```bash
sudo chmod 600 /etc/netplan/01-static-ip.yaml
sudo netplan try      # rolls back automatically if you lose the connection
sudo netplan apply
```

## DNS

### View the current configuration

```bash
nmcli device show
```

```bash
resolvectl status
```

DNS servers used by systemd-resolved:

```bash
cat /run/systemd/resolve/resolv.conf
```

Resolution priority (files, DNS…) is set in:

```text
/etc/nsswitch.conf
```

### Set DNS servers

1.  Create the configuration file:

    ```bash
    sudo mkdir -p /etc/systemd/resolved.conf.d/
    sudo nano /etc/systemd/resolved.conf.d/dns_servers.conf
    ```

2.  Add the content:

    ```ini title="/etc/systemd/resolved.conf.d/dns_servers.conf"
    [Resolve]
    DNS=8.8.8.8 1.1.1.1
    ```

3.  Restart the service:

    ```bash
    sudo systemctl restart systemd-resolved
    ```

### Resolve a domain

```bash
resolvectl query www.amazon.es
host www.amazon.es
dig www.amazon.es
nslookup www.amazon.es
```

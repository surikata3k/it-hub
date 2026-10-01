# Linux DNS

```
nmcli device show
```

## File to DNS priority
```
/etc/nsswitch.conf
```

## Set DNS Server
```
mkdir /etc/systemd/resolved.conf.d/
vi /etc/systemd/resolved.conf.d/dns_servers.conf
```

## Add content
```
[Resolve]
DNS=8.8.8.8 1.1.1.1
```

## Restart systemd-resolved
```
systemctl restart systemd-resolved
```

## You can find systemd-resolvd status with command
```
resolvectl status
systemd-resolve --status
```

## To resolve a domain, use
```
resolvectl query www.amazon.es
host www.amazon.es
dig www.amazon.es
nslookup www.amazon.es
```

## View systemd-resolved DNSs
```
cat /run/systemd/resolve/resolv.conf
```

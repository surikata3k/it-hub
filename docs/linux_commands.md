# Linux commands

## Show ip before login
```
echo "IP: \4" >> \etc\issue
```

## View services
```
systemctl list-unit-files --type service -all
```

## View iptables rules
```
iptables -L --line-numbers
```

## Text editor from terminal
```
xed
```


## Lid close
```
/etc/systemd/logind.conf:
HandleLidSwitch=ignore
```

## Grub
```
/etc/default/grub  
> GRUB_TIMEOUT=0  
> GRUB_CMDLINE_LINUX="consoleblank=60"  
  
$update-grub
```
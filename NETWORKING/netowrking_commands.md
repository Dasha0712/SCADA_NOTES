# Networking Commands


## Resetting the Network Adapter

1. First see adapter names:
```
netsh interface show interface
```
2. Then disable the adapter:
```
netsh interface set interface "Ethernet" disable
```
3. Enable again:
```
netsh interface set interface "Ethernet" enable
```

## Resetting the **ARP** table

1. First, check the current ARP table so you see what’s stored:
```
arp -a
```
2. To clear the entire ARP table:
```
arp -d *
```
3. Delete only one specific entry:
```
arp -d 192.168.1.10
```

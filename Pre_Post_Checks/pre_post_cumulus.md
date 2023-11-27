    net show route
    net show ospf neighbor 
    net show ospf interface brief
    net show clag
    clagctl showtimers
    net show system 
    net show interface all 
    ip -br addr show 
    net show interface bonds 
    net show interface bondmems 
    net show bridge  spanning-tree
    net show bridge vlan
    net show bgp summary
    net show evpn vni 
    nv show service ntp
    free -h
    ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
    net show system sensors
    net show system sensors detail
    net show system led
    net show system asic
    sudo journalctl -n -p '1..4' -u frr --since "2 min ago"
    sudo decode-syseeprom

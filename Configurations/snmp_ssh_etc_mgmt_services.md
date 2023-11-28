# SNMP

    nv set service snmp-server listening-address localhost vrf mgmt

# SSH
Cumulus 5.6 da management vrf üzeriden yapmak için aşağıdaki komutun girilmesi gerekiyor: 

    nv set system ssh-server vrf mgmt
    
Cumulus 5.6 dan önce sshd nin kapatılarak tekrar başlatılması gerekiyordu: 

    sudo systemctl stop snmpd.service
    sudo systemctl start snmpd@mgmt.service

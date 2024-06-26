# SNMP

    nv set service snmp-server listening-address localhost vrf mgmt

# SSH Cumulus 5.6 and later
The SSH service runs in the default VRF on the switch but listens on all interfaces in all VRFs. You can limit SSH to listen on specific VRFs.

    cumulus@switch:~$ nv set system ssh-server vrf mgmt
    cumulus@switch:~$ nv config apply

https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-59/System-Configuration/Authentication-Authorization-and-Accounting/SSH-for-Remote-Access/

# SSH Cumulus 5.5 and before 

SSH and VRFs The SSH service runs in the default VRF on the switch but listens on all interfaces in all VRFs. To limit SSH to listen on just one VRF, you need to bind the SSH service to that VRF.

The following example configures SSH to listen only on the management VRF:

    cumulus@switch:~$ sudo systemctl stop ssh.service
    cumulus@switch:~$ sudo systemctl disable ssh.service
    cumulus@switch:~$ sudo systemctl start ssh@mgmt.service
    cumulus@switch:~$ sudo systemctl enable ssh@mgmt.service

https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-55/System-Configuration/Authentication-Authorization-and-Accounting/SSH-for-Remote-Access/

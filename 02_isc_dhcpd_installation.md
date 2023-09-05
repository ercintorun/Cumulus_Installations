Ubuntu ISC DHCPD installation: 

    sudo apt install isc-dhcp-server 
    sudo mv /etc/dhcp/dhcpd.conf{,.backup} 

Change the configuration of dhcp server: 

    sudo nano /etc/dhcp/dhcpd.conf 

    # a simple /etc/dhcp/dhcpd.conf
    default-lease-time 600;
    max-lease-time 7200;
    authoritative;
 
    subnet 192.168.1.0 netmask 255.255.255.0 {
     range 192.168.1.100 192.168.1.200;
     option routers 192.168.1.254;
     option domain-name-servers 192.168.1.1, 192.168.1.2;
    #option domain-name "mydomain.example";
    }

    host archmachine {
    hardware ethernet e0:91:53:31:af:ab;
    fixed-address 192.168.1.20;
    } 

Bind the dhcp server to a interface 

    nano /etc/default/isc-dhcp-server

    INTERFACESv4="eth0"

Restart dhcp server 
    sudo systemctl restart isc-dhcp-server.service
    sudo systemctl status isc-dhcp-server.service

https://www.linuxfordevices.com/tutorials/ubuntu/dhcp-server-on-ubuntu

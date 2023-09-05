https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-41/Installation-Management/Zero-Touch-Provisioning-ZTP/

## Sample1
dhcpd.conf file 

    # /etc/dhcp/dhcpd.conf
    ddns-update-style none;
    authoritative;
    log-facility local7;

    option www-server code 72 = ip-address;
    option cumulus-provision-url code 239 = text;
    
    # Create an option namespace called ONIE
    # See: https://github.com/opencomputeproject/onie/wiki/Quick-Start-Guide#advanced-dhcp-2-vivsoonie/onie/
    option space onie code width 1 length width 1;
    # Define the code names and data types within the ONIE namespace
    option onie.installer_url code 1 = text;
    option onie.updater_url   code 2 = text;
    option onie.machine       code 3 = text;
    option onie.arch          code 4 = text;
    option onie.machine_rev   code 5 = text;

    # Package the ONIE namespace into option 125
    option space vivso code width 4 length width 1;
    option vivso.onie code 42623 = encapsulate onie;
    option vivso.iana code 0 = string;
    option op125 code 125 = encapsulate vivso;
    class "onie-vendor-classes" {
      # Limit the matching to a request we know originated from ONIE
      match if substring(option vendor-class-identifier, 0, 11) = "onie_vendor";
      # Required to use VIVSO
      option vivso.iana 01:01:01;
    
      ### Example how to match a specific machine type ###
      #if option onie.machine = "" {
      #  option onie.installer_url = "";
      #  option onie.updater_url = "";
      #}
    }

    # OOB Management subnet
    shared-network LOCAL-NET{
    subnet 192.168.200.0 netmask 255.255.255.0 {
      range 192.168.200.10 192.168.200.50;
      option domain-name-servers 192.168.200.1;
      option domain-name "simulation";
      default-lease-time 172800;  #2 days
      max-lease-time 345600;      #4 days
      option www-server 192.168.200.1;
      option default-url = "http://192.168.200.1/onie-installer";
      option ntp-servers 192.168.200.1;
    }
    }
    
    #include "/etc/dhcp/dhcpd.pools";
    include "/etc/dhcp/dhcpd.hosts";

dhcpd.hosts file: 

    # /etc/dhcp/dhcpd.hosts
    # Created by Topology-Converter v
    #    Template Revision: v4.7.1
    #    https://gitlab.com/cumulus-consulting/tools/topology_converter
    #    using topology data from:
    
    group {
    
      option domain-name-servers 192.168.200.1;
      option domain-name "simulation";
      option routers 192.168.200.1;
      option www-server 192.168.200.1;
      option default-url = "http://192.168.200.1/onie-installer";

    host Spine01 {hardware ethernet 48:b0:2d:b0:f3:ae; fixed-address 192.168.200.2; option host-name "Spine01"; }
    host Spine02 {hardware ethernet 48:b0:2d:51:be:8b; fixed-address 192.168.200.3; option host-name "Spine02"; }
    host Leaf01 {hardware ethernet 48:b0:2d:94:92:a3; fixed-address 192.168.200.4; option host-name "Leaf01"; }
    host Leaf02 {hardware ethernet 48:b0:2d:c6:6d:a3; fixed-address 192.168.200.5; option host-name "Leaf02"; }
    host Leaf03 {hardware ethernet 48:b0:2d:54:0a:5a; fixed-address 192.168.200.6; option host-name "Leaf03"; }
    host Leaf04 {hardware ethernet 48:b0:2d:3e:ea:31; fixed-address 192.168.200.7; option host-name "Leaf04"; }
    host Leaf05 {hardware ethernet 48:b0:2d:10:02:25; fixed-address 192.168.200.8; option host-name "Leaf05"; }
    host Leaf06 {hardware ethernet 48:b0:2d:c0:ed:3c; fixed-address 192.168.200.9; option host-name "Leaf06"; }
    }#End of static host group

Sample 2

    option cumulus-provision-url code 239 = text;
   
    subnet 192.168.0.0 netmask 255.255.255.0 {
      range 192.168.0.100 192.168.0.200;
      option cumulus-provision-url "http://192.0.2.1/demo.sh";
      host dc1-tor-sw1 { hardware ethernet 44:38:39:00:1a:6b; fixed-address 192.168.0.101; option host-name "dc1-tor-sw1"; }
    }

Do not use an underscore (_) in the hostname; underscores are not permitted in hostnames.

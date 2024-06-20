#  Cumulus & Datacenter 
Start learning cumulus from the following blog. On cumulus projects we are using symmetric IRB with manual controlled RT values. Based on the project evpn&mlag (Clag-proprietary) or evpn and multihoming (standard) is used generally. We prefer distributed gateways but might change on customer topology. 

* [1. Cumulus Intro](https://www.theasciiconstruct.com/post/cumulus-basics-part-i/)
* [2. Cumulus Bridging](https://www.theasciiconstruct.com/post/cumulus-basics-part-ii-bridging/)
* [3. Cumulus Routing](https://www.theasciiconstruct.com/post/cumulus-part-iii-routing/)
* [4. Cumulus BGP](https://www.theasciiconstruct.com/post/cumulus-basics-part-iv-bgp/)
* [5. Cumulus BGP Unnumbered](https://www.theasciiconstruct.com/post/cumulus-basics-part-v-bgp-unnumbered/)
* [6. Cumulus VXLAN L2VNI with BGP EVPN](https://www.theasciiconstruct.com/post/cumulus-basics-part-vi-vxlan-l2vnis-with-bgp-evpn/)
* [7. Cumulus VXLAN Routing Asymmetric IRB](https://www.theasciiconstruct.com/post/cumulus-basics-vii-vxlan-routing-asymmetric-irb/)
* [8. Cumulus VXLAN Routing Symmetric IRB](https://www.theasciiconstruct.com/post/cumulus-part-viii-vxlan-symmetric-routing/)
* [9. Cumulus VXLAN EVPN Route Target Control](https://www.theasciiconstruct.com/post/cumulus-understanding-vxlan-evpn-route-target-control/)
* [10. Cumulus VXLAN EVPN and MLAG](https://www.theasciiconstruct.com/post/cumulus-part-x-vxlan-evpn-and-mlag/)

# Linux_Installations
For OOB management server, Apache is needed if we need ZTP process or software upgrade over ZTP. The files need to be served on http-server. 
Vscode is needed if we want to change Ansible codes on the OOB server. Remote web vscode is useful. 
ISC DHCPD is needed for distrubiting IP addresses to oob management ports of switches and to also push options with dhcp to make the switches download ztp file etc. etc. 

* [01. Vscode, Ansible, Apache Installation](Linux_Installations/01_vscode_ansible_apache_installation.md)
* [02. ISC DHCPD Installation](Linux_Installations/02_isc_dhcpd_installation.md)
* [03. Configuring DHCPD For Cumulus](Linux_Installations/03_configuring_dhcpd.md)
* [04. Creating HTTP File Server](Linux_Installations/04_creating_http_file_server.md)

#  Other Resources
* [Ansible video learning series](https://www.youtube.com/watch?v=3RiVKs8GHYQ&list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70)
* [How to Setup Ansible Inventories](https://www.digitalocean.com/community/tutorials/how-to-set-up-ansible-inventories)
* [Jinja tutorials](https://ttl255.com/jinja2-tutorial-part-1-introduction-and-variable-substitution/)
* [Cumulus ZTP](https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-41/Installation-Management/Zero-Touch-Provisioning-ZTP/) 



#  Cumulus Own Documentation 

* [Radius-AAA CL5.6](https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-56/System-Configuration/Authentication-Authorization-and-Accounting/RADIUS-AAA/)
* [User Account CL5.6](https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-56/System-Configuration/Authentication-Authorization-and-Accounting/User-Accounts/)

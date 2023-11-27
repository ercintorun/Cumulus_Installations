Radius configuration
This section contains the steps required to configure Radius. Currently, Radius is not supported in NVUE CLI. The configuration of Radius will require the following steps:
See also: https://docs.nvidia.com/networking-ethernet-software/cumulus-linux-56/System-Configuration/Authentication-Authorization-and-Accounting/RADIUS-AAA/

 

Install Radius packages:

sudo apt-get update
sudo apt-get install libnss-mapuser libpam-radius-auth
 
 
Restart nvued:

sudo systemctl restart nvued
 
Configure the RADIUS Client:

edit the /etc/pam_radius_auth.conf file, and add the relevant server, timeout and secret key as follows:

 

server[:port]   shared_secret      timeout (secs)

10.250.0.3       <secret-key>       3

Since mgmt vrf is in use:
vrf-name mgmt

to enable debugging
debug
 

The RADIUS implementation in Cumulus maps users to a regular user or a privileged user. To quote the docs:
 
The value of the VSA (Vendor Specific Attribute) shell:priv-lvl determines the privilege level for the user on the switch. If the attribute does not return, the user does not have privileges. The following shows an example using the freeradius server for a fully privileged user.
 
Service-Type = Administrative-User,
Cisco-AVPair = "shell:roles=network-administrator",
Cisco-AVPair += "shell:priv-lvl=15"
The VSA vendor name (Cisco-AVPair in the example above) can have any content. The RADIUS client only checks for the string shell:priv-lvl.
 
If the user is non-privileged, they get the nvshow group.
If the user is privileged, they get nvapply and sudo groups.
 
The group membership is applied to the base RADIUS accounts on the system, radius_user  and radius_priv_user respectively.
 
root@cumulus:mgmt:~# id radius_user
uid=1001(radius_user) gid=1001(radius_users) groups=1001(radius_users),998(netshow),996(nvshow),993(netedit)
root@cumulus:mgmt:~# id radius_priv_user
uid=1002(radius_priv_user) gid=1001(radius_users) groups=1001(radius_users),27(sudo),994(nvapply)
 
If you want RADIUS users to be members of additional groups, those groups should be added to those radius_* accounts as above.

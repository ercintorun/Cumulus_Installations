ZTP Script Sample

    #!/bin/bash
    
    #
    # CUMULUS-AUTOPROVISIONING
    #
    
    function ping_until_reachable(){
        last_code=1
        max_tries=30
        tries=0
        while [ "0" != "$last_code" ] && [ "$tries" -lt "$max_tries" ]; do
            tries=$((tries+1))
            echo "$(date) INFO: ( Attempt $tries of $max_tries ) Pinging $1 Target Until Reachable."
            ping $1 -c2 --no-vrf-switch &> /dev/null
            last_code=$?
                sleep 1
        done
        if [ "$tries" -eq "$max_tries" ] && [ "$last_code" -ne "0" ]; then
            echo "$(date) ERROR: Reached maximum number of attempts to ping the target $1 ."
            exit 1
        fi
    }
    
    function set_password(){
         # Unexpire the cumulus account
         passwd -x 99999 cumulus
         # Set the password
         echo 'cumulus:epcc123' | chpasswd
    }
    
    
    
    function init_ztp(){
    echo "Running ZTP..."
    
    # Change default password
    set_password
    
    # make user cumulus passowrdless sudo
    echo "cumulus ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/10_cumulus
    
    # Copy SSH keys
    echo "Copying SSH keys to users cumulus and root"
    mkdir -p /root/.ssh/
    mkdir -p /home/cumulus/.ssh/
    KEY="AAAAB3NzaC1yc2EAAAADAQABAAABgQCvjs/RFPhxLQMkckONg+1RE1PTIO2JQhzFN9TRg7ox7o0tfZ+IzSB99lr2dmmVe8FRWgxVjc/V1RlmUck6M2ZL8G/jpHOvxtDTrcPo0q3de1ttmxpNpTz1nezOIegyY4yobDejYoPeXRX1mTwNPc9c/Gb00hOaoZo0Ae2laeSH8iofNpV6QHMTwxWdTXI0TZdZhVqgvDQMwL2wqXC03hxAtECs5znmWzf4gdeSmFVSCiS+OiLIwlqChwhFmbkxWCW2LSrPJkQ+QbdMJSx9JjWRL+IH9L2ikxsP6aE2h5K1hgMVTjiJMeYgHH9gPXa+Hywfu8W2TptFnAA2K4VSa6JPRHfjR3iieA2ZlDJD6M/4X4V+mhiVBf7CxKinpfcpZvMj4qVPgdfwINhV6iHp1V76WY5BJ4/V357i6sWBPw8k9+Ju4JNTPqGneKMovcF5z3u22ll4cndgIK+F/hbmNGH73GrcsMroYnYCmZst3i9aV4Ro5WBlsflmTgA96uLuqxk="
    echo $KEY >> /root/.ssh/authorized_keys
    echo $KEY >> /home/cumulus/.ssh/authorized_keys
    chown -R cumulus:cumulus /home/cumulus/.ssh
    
    exit 0
    }
    
    # Disable SSH on Default VRF and Enable on mgmt VRF
    systemctl disable --now ssh.service
    systemctl enable --now ssh@mgmt.service
    
    # Check the Cumulus Linux Release
    echo "Checking if the device is running the correct version..."
    CUMULUS_TARGET_RELEASE=5.6.0
    CUMULUS_CURRENT_RELEASE=$(cat /etc/lsb-release  | grep RELEASE | cut -d "=" -f2)
    IMAGE_SERVER_HOSTNAME=172.24.60.106
    IMAGE_SERVER=http://$IMAGE_SERVER_HOSTNAME/$CUMULUS_TARGET_RELEASE.bin
    ZTP_URL=http://$IMAGE_SERVER_HOSTNAME/cumulus-ztp.sh
    
    if [ "$CUMULUS_TARGET_RELEASE" != "$CUMULUS_CURRENT_RELEASE" ]; then
        echo "Version is incorrect: $CUMULUS_CURRENT_RELEASE, proceeding to download the correct version: $CUMULUS_TARGET_RELEASE"
        ping_until_reachable $IMAGE_SERVER_HOSTNAME
        /usr/cumulus/bin/onie-install -fa -i $IMAGE_SERVER -z $ZTP_URL && reboot
    else
        echo "Version is correct: $CUMULUS_TARGET_RELEASE"
        init_ztp
    fi

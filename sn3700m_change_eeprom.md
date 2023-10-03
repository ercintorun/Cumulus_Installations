
    sudo onie-select -k
    onie-stop
    onie-syseeprom -s 0x28=x86_64-mlnx_msn3700-r0
    reboot

==================

    sudo onie-select -k
    onie-syseeprom -s 0x21="MSN3700"
    reboot

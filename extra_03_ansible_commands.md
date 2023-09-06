## Standard privilige commands 

    ansible all --key-file /.ssh/ansible -i inventory_file -m ping
    ansible all --list-hosts
    ansible all -m gather_facts 
    ansible all -m gather_facts --limit 172.16.1.1

## Elevated priviliges commands: (sudo)
Update apt cache 

    ansible all -m apt -a update_cache=true --become --ask-become-pass

Install application sample 

    ansible all -m apt -a name=vim-nox --become --ask-become-pass

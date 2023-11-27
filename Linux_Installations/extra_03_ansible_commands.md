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
    ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass
    ansible all -m apt -a "upgrade=dist" --become --ask-become-pass

https://www.learnlinux.tv/getting-started-with-ansible-09-targeting-specific-nodes/

## Playbooks 

    ansible-playbook --ask-become-pass your_playbook.yml

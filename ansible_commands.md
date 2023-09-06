ansible all --key-file /.ssh/ansible -i inventory_file -m ping
ansible all --list-hosts
ansible all -m gather_facts 
ansible all -m gather_facts --limit 172.16.1.1

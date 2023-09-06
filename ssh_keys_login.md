On Ansible server create ssh key

    ls -la .ssh 
    ssh-keygen -t ed25519 -C "ansible_server_default"   (when ask give name like ansible) 
    passphrase: test     (leave it empty if will be used only for ansible)
    
    ls -la .ssh 

copy ssh to remote host (you can do it manually also)

    ssh-copy-id -i  /home/username/.ssh/ansible.pub 172.16.1.1   (remote host ip to copy file to)

Test from CLI

    ssh -i /home/ubuntu/.ssh/ansible.pub 172.16.1.1

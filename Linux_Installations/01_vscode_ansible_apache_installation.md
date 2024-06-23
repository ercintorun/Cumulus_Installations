This documentation explains the Ansible and VS Code and its Ansible extension installation: 

https://www.linuxcloudvps.com/blog/how-to-install-code-server-ide-platform-on-ubuntu-20-04/

## Apache Installation
    sudo apt-get update -y && sudo apt-get upgrade -y
    sudo apt install apache2
    sudo systemctl enable apache2 && sudo systemctl start apache2
    sudo systemctl status apache2

## Code-server Installation 

This is an installation without ngnix, it is using the default 8080 unencyprted port. 

    cd /opt
    sudo wget https://github.com/cdr/code-server/releases/download/v4.16.1/code-server-4.16.1-linux-amd64.tar.gz

    sudo tar -xvzf code-server-4.16.1-linux-amd64.tar.gz
    sudo mv code-server-4.16.1-linux-amd64/ code-server/	

    cd /opt
     
    sudo useradd -m -s /bin/bash code-server
    passwd code-server 

    sudo touch /etc/systemd/system/code-server.service
    sudo nano /etc/systemd/system/code-server.service

    [Unit]
    Description=code-server
    After=apache2.service
    
    [Service]
    User=code-server
    WorkingDirectory=/opt/code-server/
    Environment=PASSWORD=YourStrongPasswordHere
    ExecStart=/opt/code-server/bin/code-server --host 0.0.0.0 --auth password
    Restart=always

    [Install]
    WantedBy=multi-user.target


    sudo systemctl daemon-reload

    sudo systemctl start code-server && sudo systemctl enable code-server

## Apache Configuration 

    sudo touch /etc/apache2/sites-available/code-server.conf
    sudo nano /etc/apache2/sites-available/code-server.conf
     <VirtualHost *:80>
        ServerName YourDomain.com
    
        ProxyRequests Off
        <Proxy *>
        Order deny,allow
        Allow from all
        </Proxy>
    
        ProxyPass / http://YourDomain.com:8080/
        ProxyPassReverse / http://YourDomain.com:8080/
        <Location />
        Order allow,deny
        Allow from all
        </Location>
    </VirtualHost>


    root@vps:~# apachectl -t
    Syntax OK

    sudo a2enmod proxy
    sudo a2enmod proxy_http
    sudo systemctl restart apache2

## Ansible Installation
https://www.ansiblepilot.com/articles/streamline-your-ansible-development-with-visual-studio-code-a-step-by-step-guide-to-installing-the-ansible-extension/

    sudo apt-add-repository ppa:ansible/ansible
    sudo apt update
    sudo apt install ansible
    sudo apt install ansible-lint
    
## Ansible Navigator Installation
https://www.bojankomazec.com/2022/07/introduction-to-ansible-navigator.html

    $ sudo apt install python3-pip
    $ python3 -m pip install ansible-navigator --user
    $ echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.profile
    $ source ~/.profile
 
## Folder Permission 

The folder that the playbooks are in should be editable by "code-server" user. There are different options for it. 
Either change the group of the folder to "code-server"

    chown -R code-server:codeserver <folder-name> 

Or add "code-server" user to the group of the user that owns the folder

    sudo usermod -a -G ubuntu code-server

Somehow if you endup with bad file permissions on your ssh folder etc, execute below: 

    chmod go-w /home/ubuntu
    chmod 700 /home/ubuntu/.ssh.  
    chmod 600 /home/ubuntu/.ssh/config
    chmod 600 /home/ubuntu/.ssh/id_rsa

    

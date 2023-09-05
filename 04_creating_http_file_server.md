Create the directory for your_domain as follows:

    sudo mkdir /var/www/fileserver 

Next, assign ownership of the directory with the $USER environment variable:

    sudo chown -R $USER:$USER /var/www/fileserver

The permissions of your web roots should be correct if you haven’t modified your umask value, which sets default file permissions. To ensure that your permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, you can input the following command:

    sudo chmod -R 755 /var/www/fileserver

Create an virtualhost to serve as fileserver on default http port: 
    
    sudo nano /etc/apache2/sites-available/fileserver.conf

    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName fileserver
        ServerAlias www.fileserver
        DocumentRoot /var/www/fileserver
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost> 

Let’s enable the file with the a2ensite tool:

    sudo a2ensite fileserver.conf
    sudo apache2ctl configtest

Disable the default site defined in 000-default.conf:

    sudo a2dissite 000-default.conf

Restart apache2

    sudo systemctl restart apache2

Upload cumulus image and ztp script inside folder /var/www/fileserver. Do not forget to change permissions again: 

    sudo chmod -R 755 /var/www/fileserver

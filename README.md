# Project_Server_Web
a simple projec on a Web Server
# Léxique :

# Documentation d'architecture:
- définition du réseau :

![image](https://user-images.githubusercontent.com/72856412/112160798-d2318180-8bea-11eb-935a-bac703b983d6.png)

- bonne pratique :

<p>backup des fichiers <br/>
serveur de bonne taille par rapport a l'utilisation <br/>
synchroniser l'horloge des machines <br/>
installation de nettools <br/>
installation de lynx <br/><p>

- détaille des configuration a mettre en place pour faire fonctionner la solution:

<p>Installation d'un LAMP stack <br/>
Installation de snap <br/>
Installation de wordpress et wekan <br/><p>

# Documentation d'exploitation:
- outils :
> Apache2<br/> MySQL<br/> Nginx<br/> Snap<br/>

tuto :
- service :
> Wordpress<br/> Wekan<br/>

tuto :
```
LAMPSTACK
Change the YELLOW TEXT by whatever

1-Update
sudo apt update
2-Download Apache
sudo apt install apache2
3-Allow Apache through the firewall
sudo ufw allow in "Apache"
sudo ufw status (to check)
4-Download MySQL
sudo apt install mysql-server
5-Secure Download after
sudo mysql_secure_installation
type Y for yes and anything for pass
VALIDATE PASSWORD PLUGIN
make you have a password
6-Connect to MySQL
sudo mysql
7-Exit MySQL
exit
8-Download PhP
sudo apt install php libapache2-mod-php php-mysql
9-Confirm
php -v
10-Create your domain
sudo mkdir /var/www/your_domain
11-Get Ownership of your_domain
sudo chown -R $USER@@:$USER /var/www/your_domain
12-Configure
sudo nano /etc/apache2/sites-available/your_domain.conf
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

13-Activate VirtualHost
sudo a2ensite your_domain
13.5(optionel)-Delete Apache basic site
sudo a2dissite 000-default
14-Check for error
sudo apache2ctl configtest
15-Apply
sudo systemctl reload apache2
16-Create an index.html to make the site
nano /var/www/your_domain/index.html

<h1>It works!</h1>

<p>This is the landing page of <strong>your_domain</strong>.</p>

17-Check on your browser
http://your_domain_or_ip
18-Prioritize Php over html
sudo nano /etc/apache2/mods-enabled/dir.conf

<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

19-Apply
sudo systemctl reload apache2
20-Check php
nano /var/www/your_domain/info.php

<?php
phpinfo();

21-Go check on your browser
http://your_domain_or_ip/info.php
22-Remove info.php
sudo rm /var/www/your_domain/info.php

```



# Configuration a réaliser :

- systeme:<br/>
  instalation ubuntu serveur avec OpenSSH<br/>
  
- réseau:<br/>
  serveur backend en réseau interne<br/>
  serveur proxy en réseau interne + bridge<br/>
    
- services: <br/>
  changer le port d'utilisation de wekan lors de l'installation avec snap pour éviter les conflits avec apache2<br/>

- configuration du fichier hosts coté windows et proxy lorsque l'on rajoute un site



# Comment ajouter un site supplémentaire :
- sur le backend :
- sur le proxy :
- sur notre machine windows :



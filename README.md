# Project_Server_Web
a simple project on a Web Server
# Lexique :
back-end : est un terme désignant un étage de sortie d'un logiciel devant produire un résultat. <br>

reverse proxy : intermédiaire entre les clients et les serveurs web. Par exemple, 
il permet d’apporter de la sécurité.<br>

SSL : pour Secure Socket Layer, est un protocole de sécurité qui permet de sécuriser les échanges d'informations entre des appareils reliés à un réseau interne ou à Internet.

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

tuto : LAMPSTACK(Apache2 et Sql)

```
Change the "TEXT" by whatever

1-Update
sudo apt update
2-Download Apache
sudo apt install apache2
3-Allow Apache through the firewall
sudo ufw allow in Apache
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
sudo mkdir /var/www/”your_domain”
11-Get Ownership of “your_domain”
sudo chown -R $”USER@@”:$”USER” /var/www/”your_domain”
12-Configure
sudo nano /etc/apache2/sites-available/your_domain.conf
<VirtualHost *:80>
    ServerName “your_domain”
    ServerAlias www.”your_domain”
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/”your_domain”
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
nano /var/www/”your_domain”/index.html
<h1>It works!</h1>
<p>This is the landing page of <strong>your_domain</strong>.</p>
17-Check on your browser
http://”your_domain”_or_ip
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
http://”your_domain”_or_ip/info.php
22-Remove info.php
sudo rm /var/www/your_domain/info.php
```

tuto : Avoir plusieurs sites actif sur le même serveur

```
Change the "TEXT" by whatever

1-Make a Directory for Each Site
a) mkdir -p /var/www/"domain.com"/
b) mkdir -p /var/www/"domain2.com"/

2-Set Folder Permissions
a) chmod -R 755 /var/www

3-Set up an Index Page (If you want to make sure the index.html file is created for each domain)
a) vim /var/www/”domain.com”/index.html
b) “testing for domain.com”
c) vim /var/www/”domain2.com”/index.html
d) “testing for domain2.com”

4- Copy the Config File for Each Site
a) cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/”domain.com”.conf
b) cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/”domain2.com”.conf

5- Edit the Config File for Each Site
a) vim /etc/apache2/sites-available/”domain.com”.conf
b) <VirtualHost *:80>
        ServerAdmin “admin@example.com”
        ServerName “domain.com”
        ServerAlias “www.domain.com”
        DocumentRoot “/var/www/domain.com”
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>

6-Enable Your Config File
a) a2dissite 000-default.conf
b) a2ensite domain.com.conf
c) a2ensite domain2.com.conf
d) systemctl restart apache2
```
- service :
> Wordpress<br/> Wekan<br/>

tuto : Wordpress

```Change the ‘TEXT’ by whatever

1-Creating a MySQL Database and User for WordPress
a) mysql -u root -p
b) sudo mysql -u root
c) ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
d) mysql -u root -p
e) CREATE DATABASE ‘wordpress’ DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
f) CREATE USER 'wordpressuser'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
g) GRANT ALL ON ‘wordpress’.* TO 'wordpressuser'@'%';
h) FLUSH PRIVILEGES;
i) EXIT;

2-Installing Additional PHP Extensions
a) sudo apt update
b) sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
c) sudo systemctl restart apache2

3-Adjusting Apache’s Configuration to Allow for .htaccess Overrides and Rewrites
a) sudo nano /etc/apache2/sites-available/’wordpress’.conf
b) <Directory /var/www/’wordpress’/>
       AllowOverride All
   </Directory>
c) sudo a2enmod rewrite
d) sudo apache2ctl configtest(for syntax error)
    Output (if your syntax is correct):
    AH00558: apache2: Could not reliably determine the server's fully qualified domain name, 
    using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
    Syntax OK
e) sudo systemctl restart apache2

4-Downloading WordPress
a) cd /tmp
b) curl -O https://wordpress.org/latest.tar.gz
c) tar xzvf latest.tar.gz
d) touch /tmp/wordpress/.htaccess
e) cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
f) mkdir /tmp/wordpress/wp-content/upgrade
g) sudo cp -a /tmp/’wordpress’/. /var/www/’wordpress’

5-Configuring the WordPress Directory
a) sudo chown -R www-data:www-data /var/www/’wordpress’
b) sudo find /var/www/’wordpress’/ -type d -exec chmod 750 {} \;
c) sudo find /var/www/’wordpress’/ -type f -exec chmod 640 {} \;
d) curl -s https://api.wordpress.org/secret-key/1.1/salt/
    Output(Warning! It is important that you request unique values each time. Do NOT copy the values below!
    You will have something like that):
    define('AUTH_KEY',         '1jl/vqfs<XhdXoAPz9 DO NOT COPY THESE VALUES c_j{iwqD^<+c9.k<J@4H');
    define('SECURE_AUTH_KEY',  'E2N-h2]Dcvp+aS/p7X DO NOT COPY THESE VALUES {Ka(f;rv?Pxf})CgLi-3');
    define('LOGGED_IN_KEY',    'W(50,{W^,OPB%PB<JF DO NOT COPY THESE VALUES 2;y&,2m%3]R6DUth[;88');
    define('NONCE_KEY',        'll,4UC)7ua+8<!4VM+ DO NOT COPY THESE VALUES #`DXF+[$atzM7 o^-C7g');
    define('AUTH_SALT',        'koMrurzOA+|L_lG}kf DO NOT COPY THESE VALUES  07VC*Lj*lD&?3w!BT#-');
    define('SECURE_AUTH_SALT', 'p32*p,]z%LZ+pAu:VY DO NOT COPY THESE VALUES C-?y+K0DK_+F|0h{!_xY');
    define('LOGGED_IN_SALT',   'i^/G2W7!-1H2OQ+t$3 DO NOT COPY THESE VALUES t6**bRVFSD[Hi])-qS`|');
    define('NONCE_SALT',       'Q6]U:K?j4L%Z]}h^q7 DO NOT COPY THESE VALUES 1% ^qUswWgn+6&xqHN&%');

e) sudo nano /var/www/wordpress/wp-config.php
    Search for:
    . . .define('AUTH_KEY',         'put your unique phrase here');
    define('SECURE_AUTH_KEY',  'put your unique phrase here');
    define('LOGGED_IN_KEY',    'put your unique phrase here');
    define('NONCE_KEY',        'put your unique phrase here');
    define('AUTH_SALT',        'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT',   'put your unique phrase here');
    define('NONCE_SALT',       'put your unique phrase here');
    . . .
f) Delete those lines and change the values with key:
    . . .
    define('AUTH_KEY',         'VALUES COPIED FROM THE COMMAND LINE');
    define('SECURE_AUTH_KEY',  'VALUES COPIED FROM THE COMMAND LINE');
    define('LOGGED_IN_KEY',    'VALUES COPIED FROM THE COMMAND LINE');
    define('NONCE_KEY',        'VALUES COPIED FROM THE COMMAND LINE');
    define('AUTH_SALT',        'VALUES COPIED FROM THE COMMAND LINE');
    define('SECURE_AUTH_SALT', 'VALUES COPIED FROM THE COMMAND LINE');
    define('LOGGED_IN_SALT',   'VALUES COPIED FROM THE COMMAND LINE');
    define('NONCE_SALT',       'VALUES COPIED FROM THE COMMAND LINE');
    . . .
g) Anywhere else in the file add:
    . . .
    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define( 'DB_NAME', 'wordpress' );

    /** MySQL database username */
    define( 'DB_USER', 'wordpressuser' );

    /** MySQL database password */
    define( 'DB_PASSWORD', 'password' );

    /** MySQL hostname */
    define( 'DB_HOST', 'localhost' );

    /** Database Charset to use in creating database tables. */
    define( 'DB_CHARSET', 'utf8' );

    /** The Database Collate type. Don't change this if in doubt. */
    define( 'DB_COLLATE', '' );
    . . .
    define('FS_METHOD', 'direct');

6-Completing the Installation Through the Web Interface
a) https://’server_domain_or_IP’
```

tuto: Wekan(avec Snap)

```
1-apt-get install snapd
2-snap install wekan
3-snap set wekan root-url="http://your_ip" (ex: “http://192.168.10.12”)
4-snap set wekan port="your_port" (ex:”3001”)
5-systemctl restart snap.wekan.wekan
6-On your browser http://your_ip:your_port/sign-in
```

# Configuration a réaliser :

- systeme:<br/>
  installation ubuntu serveur avec OpenSSH<br/>
  
- réseau:<br/>
  serveur backend en réseau interne<br/>
  serveur proxy en réseau interne + bridge<br/>
    
- services: <br/>
  changer le port d'utilisation de wekan lors de l'installation avec snap pour éviter les conflits avec apache2<br/>

- configuration du fichier hosts coté windows et proxy lorsque l'on rajoute un site



# Comment ajouter un site supplémentaire :
- sur le backend :
<p>mkdir -p /var/www/domain.com/public_html<br>
chmod -R 755 /var/www<br>
vim /var/www/domain.com/public_html/index.html<br>
a l'intérieur écriver quelque chose comme : <br>
testing for domain.com<br>
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/domain.com.conf<br>
vim /etc/apache2/sites-available/domain.com.conf<br>
<br>
<VirtualHost *:80><br>
ServerAdmin admin@example.com<br>
ServerName domain.com<br>
ServerAlias www.domain.com<br>
DocumentRoot /var/www/domain.com/public_html<br>
ErrorLog ${APACHE_LOG_DIR}/error.log<br>
CustomLog ${APACHE_LOG_DIR}/access.log combined<br>
</VirtualHost><br>
<br>
    a2dissite 000-default.conf<br>
    a2ensite domain.com.conf<br>
    systemctl restart apache2<br>
<p>
- sur le proxy :
<p>server {<br>
server_name domain.com;<br>
location / {<br>
        proxy_set_header Host $host;<br>
        proxy_set_header X-Real-IP $remote_addr;<br>
        proxy_pass http://domain.com;<br>
}<br>
}<br>
    
configurer le fichier hosts (/etc/hosts)
- sur notre machine windows :
configurer le fichier hosts<br>

# Configurer la connexion en https :
- coté reverse proxy :
<p>
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt<br>
    La seule chose que vous avez a configurer est :<br>
    Common Name (e.g. server FQDN or YOUR name) []:server_IP_address<br>
    sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096<br>
    sudo nano /etc/nginx/snippets/self-signed.conf<br>
    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;<br>
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;<br>
    
  <br>sudo nano /etc/nginx/snippets/ssl-params.conf
  <br>
  >ssl_protocols TLSv1.2;<br>
ssl_prefer_server_ciphers on;<br>
ssl_dhparam /etc/nginx/dhparam.pem;<br>
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;<br>
ssl_ecdh_curve secp384r1; # Requires nginx >= 1.1.0<br>
ssl_session_timeout  10m;<br>
ssl_session_cache shared:SSL:10m;<br>
ssl_session_tickets off; # Requires nginx >= 1.5.9<br>
ssl_stapling on; # Requires nginx >= 1.3.7<br>
ssl_stapling_verify on; # Requires nginx => 1.3.7<br>
resolver 8.8.8.8 8.8.4.4 valid=300s;<br>
resolver_timeout 5s;<br>
add_header X-Frame-Options DENY;<br>
add_header X-Content-Type-Options nosniff;<br>
add_header X-XSS-Protection "1; mode=block";<br>

<br>
sudo nano /etc/nginx/sites-available/conf.d/test<br>

server {<br>
    listen 443 ssl;<br>
    listen [::]:443 ssl;<br>
    include snippets/self-signed.conf;<br>
    include snippets/ssl-params.conf;<br><br>
    
   server_name example.com;<br>
   ssl on;<br>
    ssl_session_cache  builtin:1000  shared:SSL:10m;<br>
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;<br>
    #ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;<br>
   ssl_prefer_server_ciphers on;<br><br>

   location / {<br>
<br>
 proxy_set_header        Host $host;<br>
 proxy_set_header        X-Real-IP $remote_addr;<br>
 proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;<br>
 proxy_set_header        X-Forwarded-Proto $scheme;<br>
 <br>
  proxy_pass          http://wordpress;<br>
  proxy_read_timeout  90;<br>
  proxy_redirect      http://example https://example;<br>
<br>
   . . .<br><br>

}<br>
. . .<br>
server {<br>
    listen 80;<br>
    listen [::]:80;<br><br>

   server_name example.com www.example.com;<br><br>

   return 302 https://$server_name$request_uri;<br>
}<br>
<br>
sudo ufw allow 'Nginx Full'<br>
sudo ufw delete allow 'Nginx HTTP'<br>
sudo systemctl restart nginx<br>
</p>


    

    



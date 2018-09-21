

Assignment 4:

# setup a lamp server and host a wordpress site on your local system Note: Don&#39;t install php directly using yum or apt-get

- compile it from source using PREFIX with /usr/local/php directory

Setup LAMP server:

1. Install apache2

$sudo apt-get update

$sudo apt-get install apache2

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/01\_apache\_apt.png)

configure apache2

$sudo vi /etc/apache2/apache2.conf

$ServerName 192.168.33.70

$sudo apache2ctl configtest

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/02\_apache2\_configtest.png)

$sudo service apache2 restart

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/03\_apache2\_status.png)

$curl http://192.168.33.70:80

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/04\_apache2\_output.png)

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/04\_output2.png)

Install mysql

$sudo apt-get install mysql-server

asks for root-password

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/07\_root\_user.png)

$sudo apt-get install php libapache2-mod-php php-mcrypt php- mysql

To check whether mysql running or not

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/06\_check\_mysql.png)

configure the file:

$sudo nano /etc/apache2/mods-enabled/dir.conf

\&lt;IfModule mod\_dir.c\&gt;

     DirectoryIndex index.html index.cgi index.pl index.php # index.xhtml index.htm

\&lt;/IfModule\&gt;

\&lt;IfModule mod\_dir.c\&gt;

    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

\&lt;/IfModule\&gt;

$sudo systemctl restart apache2

configure the info.php file

# sudo nano /var/www/html/info.php

\&lt;?php

phpinfo();

?\&gt;

Check on your local host

curl http://18.222.198.247/info.php

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/08\_php\_output.png)

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/08\_php\_output2.png)

Create database and Grant the local host

# mysql -u root -p

mysql\&gt; CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8\_unicode\_ci;

mysql\&gt; GRANT ALL ON wordpress.\* TO &#39;root&#39;@&#39;localhost&#39; IDENTIFIED BY &#39;Arun@999&#39;;

mysql\&gt; FLUSH PRIVILEGES;

mysql\&gt; EXIT;

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/09\_db\_create.png)

Install Additional PHP Extensions

# sudo apt-get update

# sudo apt-get install php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc

# sudo systemctl restart apache2

Adjust Apache&#39;s Configuration to Allow for .htaccess Overrides and Rewrites

# sudo nano /etc/apache2/apache2.conf

\&lt;Directory /var/www/html/\&gt;

    AllowOverride All

\&lt;/Directory\&gt;

Enable the Rewrite Module

# sudo a2enmod rewrite

Enable the Changes

# sudo apache2ctl configtest

# sudo systemctl restart apache2

10) Download WordPress

$ cd /tmp; sudo wget -c http://wordpress.org/latest.tar.gz

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/10\_wp\_download.png)

# tar xzvf latest.tar.gz

# sudo touch /tmp/wordpress/.htaccess

# chmod 660 /tmp/wordpress/.htaccess

$sudo mv /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php

# mkdir /tmp/wordpress/wp-content/upgrade

move the WordPress files from the extracted folder to the Apache default root directory, /var/www/html/

$ sudo rsync -av wordpress/\* /var/www/html/

set the correct permissions on the website directory, i.e. give ownership of the WordPress files to the web server

$ sudo chown -R www-data:www-data /var/www/html/

$ sudo chmod -R 755 /var/www/html/

Setting up the WordPress Configuration File

# curl -s https://api.wordpress.org/secret-key/1.1/salt/

paste the output in:-

# nano /var/www/html/wp-config.php

Configure the changes

# sudo vi /var/www/html/wp-config.php

define(&#39;DB\_NAME&#39;, &#39;wordpress&#39;);

/\*\* MySQL database username \*/

define(&#39;DB\_USER&#39;, &#39;localhost&#39;);

/\*\* MySQL database password \*/

define(&#39;DB\_PASSWORD&#39;, &#39;arun@999);

![LAMP](https://github.com/arunkundrupu1990/ninja\_linux/blob/master/day4/images/12\_output.png)



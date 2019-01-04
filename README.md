# Apache virtual hosts configuration for macOS
This is a step by step to configure virtual hosts on an Apache server and also changes the default root directory 
for your projects in PHP. 

You can also use the httpd.conf in this repository, it is working and tested for macOS Mojave 10.14. 
Ps: If you intend to use this file it is recommended that you backup your current httpd.conf first in case you have some specific configuration for your current projects, and then you can copy the new one to your /etc/apache2/ folder and jump to step 4 where you just need to change the path for your root directory.

### 1) Run the command: `sudo nano /etc/apache2/httpd.conf`

### 2) make sure the below modules are uncommented:

`LoadModule authz_core_module libexec/apache2/mod_authz_core.so`

`LoadModule authz_host_module libexec/apache2/mod_authz_host.so`

`LoadModule userdir_module libexec/apache2/mod_userdir.so`

`LoadModule include_module libexec/apache2/mod_include.so`

`LoadModule rewrite_module libexec/apache2/mod_rewrite.so`

`Include /private/etc/apache2/extra/httpd-userdir.conf`

`Include /private/etc/apache2/users/*.conf`

### 3) Check the right php module for you (PHP 5.6 ot 7.x) and load the required module too by adding the line or commenting/uncommenting the lines

`LoadModule php5_module libexec/apache2/libphp5.so`

*OR*

`LoadModule php7_module libexec/apache2/libphp7.so`


### 4) Also change the references to your root directory (It can be any directory you like): 
Change from
```
DocumentRoot "/Library/WebServer/Documents"
<Directory "/Library/WebServer/Documents">
```
to
```
DocumentRoot "/Users/YOUR_USERNAME/Sites"
<Directory "/Users/YOUR_USERNAME/Sites">
```
### 5) Run the command: sudo apachectl restart

### 6) Create the folder /Users/YOUR_USERNAME/Sites/test

### 7) On the terminal give it the right permissions with the following commands: 
`cd ~/Sites/test`
`sudo chmod 775`

### 8) Include a index.php and include the phpinfo() method on it, when we access the page we will see all the PHP info on the screen and save the file:
```
<?php 
echo phpinfo();
?>
```

### 9) Run the command: `sudo nano /etc/apache2/extra/httpd-vhosts.conf`

### 10) include these lines above all the virtual hosts:
```
<VirtualHost *:80>
ServerName localhost
DocumentRoot /Users/YOUR_USERNAME/Sites
</VirtualHost>
```

### 11) Include your virtual hosts following this format and save the file:
```
<VirtualHost *:80>
    ServerAdmin contact@adrianomarra.com
    DocumentRoot "/Users/adrianomarra/Sites/test"
    ServerName test.localhost
    ServerAlias www.test.localhost
    ErrorLog "/private/var/log/apache2/test.localhost-error_log"
    CustomLog "/private/var/log/apache2/test.localhost-access_log" common
</VirtualHost>
```
### 12) Run the command: `sudo nano /etc/hosts`

include your host in the following format and save the file:
`127.0.0.1       test.localhost www.test.localhost`

### 13) Run the command: `sudo apachectl restart`

Thats it you ready to go and test the links: [test.localhost](test.localhost) and www.test.localhost

Ps: for this tutoral we used the .localhost extention, but it could be anything you like, .dev for example.

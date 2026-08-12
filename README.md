# Apache Port-Based Virtual Hosting

## 📌 Project Overview

This project demonstrates how to configure **Port-Based Virtual Hosting** using Apache HTTP Server on a Linux server.

In this project, two different websites are hosted on the same server using different ports.

The server IP address is:

```text
192.168.56.106

| Website                                     | Port | DocumentRoot                              |
| ------------------------------------------- | ---: | ----------------------------------------- |
| [www.mcloud.local](http://www.mcloud.local) | 8081 | `/var/www/html/www-mcloud/mcloud`         |
| dl.mcloud.local                             |   80 | `/var/www/html/dl-mcloud/dl-mcloud-local` |
                    Apache HTTP Server
                         |
                  192.168.56.106
                         |
             +-----------+-----------+
             |                       |
          Port 80                 Port 8081
             |                       |
       dl.mcloud.local        www.mcloud.local
             |                       |
      DL Website             Main Website


## Requirements
Linux Server
Apache HTTP Server
Root or sudo access
Two websites/source codes
Network connectivity to the server

## 📂 Project Structure

Apache configuration files are located in:

/etc/httpd/conf.d/

Configuration files:

/etc/httpd/conf.d/dl-mcloud.conf
/etc/httpd/conf.d/www-mcloud.conf

Website files are stored under:

/var/www/html/

Example:

/var/www/html/
├── dl-mcloud/
│   └── dl-mcloud-local/
│
└── www-mcloud/
    └── mcloud/
        ├── index.html
        ├── about.html
        ├── contact.html
        ├── price.html
        ├── service.html
        ├── css/
        ├── fonts/
        ├── images/
        └── js/
🔧 Apache Configuration
www.mcloud.local

The main website is configured on port 8081.

##🌐 Testing

The websites can be accessed using:

http://192.168.56.106/

and:

http://192.168.56.106:8081/

The second website can be tested directly through port 8081.

##🔍 Useful Apache Commands

Check Apache configuration syntax:

apachectl -t

Expected output:

Syntax OK

Display configured VirtualHosts:

httpd -S

Check listening ports:

ss -lntp | grep httpd

Restart Apache:

systemctl restart httpd

Reload Apache configuration:

systemctl reload httpd

Check Apache status:

systemctl status httpd
🛠️ Troubleshooting
403 Forbidden

One of the main issues encountered during this project was:

403 Forbidden

Apache logs showed:

AH00035: Permission denied

and:

search permissions are missing on a component of the path

The problem was caused by incorrect directory permissions.

For example:

drwx------ root root

Apache could not enter the directory because the Apache user did not have execute (x) permission.

The directory permissions were changed to:

chmod 755 /var/www/html/www-mcloud
chmod 755 /var/www/html/www-mcloud/mcloud
chmod 755 /var/www/html/www-mcloud/mcloud/css
chmod 755 /var/www/html/www-mcloud/mcloud/images

After correcting the permissions, Apache was able to access the website files.

## 🔎 Checking Directory Permissions

The namei command is useful for checking permissions of every component of a path:

namei -l /var/www/html/www-mcloud/mcloud/index.html

Example:

drwxr-xr-x root root /
drwxr-xr-x root root var
drwxr-xr-x root root www
drwxr-xr-x root root html
drwxr-xr-x root root www-mcloud
drwxr-xr-x root root mcloud
-rw-r--r-- root root index.html

This makes it easy to identify which directory is preventing Apache from accessing the file.

## 🔐 Apache User and Permissions

Apache runs using the apache user on Rocky Linux.

For example:

ps aux | grep httpd

Since website files are owned by root, Apache generally accesses them through the other permissions.

For example:

-rw-r--r-- root root index.html

means:

Owner → rw-
Group → r--
Other → r--

For directories:

drwxr-xr-x

allows Apache to traverse and access the directory.

📚 What I Learned

Through this project, I practiced:

Apache HTTP Server installation and configuration
VirtualHost configuration
Port-Based Virtual Hosting
DocumentRoot
ServerName
Apache access and error logs
Apache listening ports
Linux file and directory permissions
chmod
Understanding 755 and 644
Apache user permissions
Troubleshooting 403 Forbidden
Using curl for HTTP testing
Using ss to check listening ports
Using httpd -S to inspect VirtualHosts
Using namei -l to troubleshoot filesystem permissions

## 🧪 Troubleshooting Workflow

When Apache returns 403 Forbidden, the following commands are useful:

apachectl -t
httpd -S
ss -lntp | grep 8081
namei -l /path/to/index.html
tail -f /var/log/httpd/error_log
curl -I http://192.168.56.106:8081/

This helps determine whether the problem is related to:

Apache configuration
VirtualHost
Listening port
Filesystem permissions
Apache authorization
DocumentRoot

## 📌 Important Note

This project uses Port-Based Virtual Hosting.

The websites are distinguished primarily by their ports:

192.168.56.106:80
192.168.56.106:8081

Therefore, this project is different from IP-Based Virtual Hosting, where different IP addresses are assigned to different VirtualHosts.

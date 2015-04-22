# Introduction #

This guide is intended to guide one through the process of getting pdns, pdns-recursor, and djdns installed on Ubuntu Jaunty although the steps should be similar for other distros. This document does not cover how to get the global domain name systems to use your djdns server installation or how to install your database server.

# Details #

## Requirements List ##
  1. An Ubuntu Jaunty installed machine.
  1. The IP address of the machine
  1. Access to the internet
  1. A Postgresql server, database and login info

## Install and Configure Power Dns Server and the Postgresql Backend ##
Install pdns-server:
```
# install the server and backend (change to your preference only pg is tested at this point)
sudo apt-get install pdns-server pdns-backend-pgsql dnsutils
```
Edit /etc/powerdns/pdns.conf:
```
sudo vi /etc/powerdns/pdns.conf
```
Find and change a a few lines:
```
# find the launch section and tell the server to use the backend
# lanuch =
launch=gpgsql
# find and change the local-address setting to your real ip (not 0.0.0.0 or 127.0.0.1)
# this allows us more easily setup recursion later if needed
local-address=<your_ipaddress>
```
Edit /etc/powerdns/pdns.d/pdns.local:
```
sudo vi /etc/powerdns/pdns.d/pdns.local
```
Find and add a few lines:
```
gpgsql-host=<your_dbserver_ipaddress>
gpgsql-port=<your_dbserver_port>
gpgsql-dbname=<your_dbname>
gpgsql-user=<your_dbuser>
gpgsql-password=<your_dbpass>
```
Normally you would install your database here but django makes that easy to do, so we progress to the next section.

## Install Django and djdns ##
Install django and subversion:
```
sudo apt-get install python-django subversion
```
Choose a directory to install djdns and check it out from svn:
```
# make a directory for django projects
sudo mkdir /opt/django-projects/
# change to that dir
cd /opt/django-projects/
# checkout djdns
sudo svn checkout http://djdns.googlecode.com/svn/trunk/ djdns
```

## Configure djdns settings.py and create initial database and user ##

Follow at your own risk as we do not provide any guarantee that this will not destroy your system :)
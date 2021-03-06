geteset
=======

It simplifies the creation of mirror updates for ESET NOD32

Introduction
------------

For all of your clients you can share antivirus updates bases in two ways.

1. Through web (link: "http://......*")

2. Through SAMBA-share (link: "\\server\updates")

Requirement
-----------
For deploying GetESET you need:

- BSD of Linux OS
- Valid ESET Antivirus license (username and password)
- Samba or HTTP-shared directory
- PHP 5 CLI
- UnRAR (or "unrar-free" for Linux OS)
- WGET

Installation
------------
**Linux prepare**

Install Unrar:

`aptitude update
aptitude install unrar-free`

Install PHP5:

`aptitude install php5`

`ln -s /usr/bin/php5 /usr/local/bin/php`

`ln -s /bin/uname /usr/bin/uname`

**FreeBSD/pfSense prepare**

Install Unrar (for FreeBSD):

`cd /usr/ports/archivers/unrar; make install clean`

Install Wget (for FreeBSD):

`cd /usr/ports/ftp/wget; make install clean`

for psSense:

`/usr/sbin/pkg_add -r wget`

`/usr/sbin/pkg_add -r unrar`

`rehash`

**Install**

Go to the working directory for GetESET and run:

`git clone https://github.com/SPIDER-L33T/geteset.git`

Configuring
-----------
You need edit options in **settings.txt** :

**ver=**
- comma separated version number of ESET for your need. For example:

`ver="6,7"`

Edit path to Web-server directory, for sharing ESET antivirus updates:

`upload_dir="/var/www/upd"`

- in this example all updates we place to http://server_name/upd

Final link for clients with ESET NOD32 Antivirus Version 6 will be: http://server_name/upd/v6

Your user and password of ESET-license. For example:

`user="EAV-0101020203"`

`password="6p4nk37bsh"`

**Enable logging**

By default GetESET logging in file, but you may change it to using MySQL-database. For changing logging to MySQL replace:

`log_type="file"``

to

`log_type="db"`

and edit this option:

`log_db_host="127.0.0.1"`

`log_db_user="root"`

`log_db_password="secret"`

`log_db_base="stats"`

`log_db_table="eset_log"`

- in this example GetESET place logs in table "eset_log". Connectiog to MySQL server by address "localhost" for SQL-user "root" with password "secret". Database name: "stats"

Creating table for log:

`CREATE TABLE `eset_log` (
  `eset_id` int(11) NOT NULL AUTO_INCREMENT,
  `log` text NOT NULL,
  `date` datetime NOT NULL,
  `sign` tinyint(4) NOT NULL,
  UNIQUE KEY `eset_id` (`eset_id`)
) ENGINE=MyISAM AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;`

Running
-------
Place in file **/etc/crontab** this line:

`0 */1 * * * root /root/scripts/geteset/geteset.php`

Using
-----
In all clients antivirus arvanced settings change the update-server list to your server link with GetESET. For example:

if client version of antivirus is "6":

`http://server_name/upd/v6`

if client version of antivirus is "7":

`http://server_name/upd/v7`

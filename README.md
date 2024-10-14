**Database:**
 `sudo vim /etc/db.ini.php`

``` ini
;<?php die(); ?>

databasePassword      = '87tZ#Ub4tE^m!8Ka=anK3z9gnnnR-JJa'

edsKey                = 'P9pYNNuvX5USn8ME3wZCXX8cXAmM'

databaseAnpr          = 'ANPR'
databaseAid        = 'CCTVMES'
databasePassedAid  = 'PassedVehicles'
databaseNumbering     = 'numbering'
tablePassedAid     = 'PassedVehicleRecords'
tableDastor           = 'TBL_NUMBERING_KHODRO_DASTOR'

yamlFile              = '/etc/netplan/01-network-manager-all.yaml'
```

**sudo Permission:**
 `sudo visudo`

``` bash
www-data ALL=(root) NOPASSWD: /sbin/reboot, /sbin/shutdown, /usr/sbin/netplan, /usr/sbin/hddtemp, /usr/bin/inxi, /usr/bin/anydesk, /var/www/html/vendor/bash/dateTime.sh, /var/www/html/vendor/bash/system.sh
```

**Network & NTP Permission:**

``` bash
sudo chmod 755 /etc/netplan/
sudo chmod -R 666 /etc/netplan/* /etc/chrony/chrony.conf
```

**html Directory Permission:**

``` bash
sudo gpasswd -a anpr www-data &&
sudo mkdir -p /var/www/html/temp &&
sudo mkdir -p /var/www/configs/log &&
sudo mkdir -p /var/www/store/sql &&
sudo touch /var/www/html/.htaccess &&
sudo ln -snf /var/www/store/ /var/www/html/store &&
sudo ln -snf /var/www/configs/ /var/www/html/configs &&
sudo ln -snf /var/www/configs/log /var/www/html/log &&
sudo chown anpr:www-data -R /var/www/html/ &&
sudo chown anpr:www-data -R /var/www/configs/ &&
sudo chown anpr:www-data /var/www/store/ &&
sudo chown anpr:www-data -R /var/www/store/sql &&
sudo find /var/www/configs/ -type f -exec chmod 640 {} + &&
sudo find /var/www/configs/ -type d -exec chmod 770 {} + &&
sudo find /var/www/html/ -type l -exec chmod 750 {} + &&
sudo find /var/www/html/ -type d -exec chmod 750 {} + &&
sudo find /var/www/html/ -type f -exec chmod 640 {} + &&
sudo find /var/www/html/ -type f -name "*.sh" -exec chmod 750 {} + &&
sudo find /var/www/html/ -type f -name "*.sh" -exec dos2unix -q {} + &&
sudo find /var/www/html/ -type d -name "temp" -exec chmod 770 -R {} + &&
sudo find /var/www/html/ -type f -name "README.md" -exec chmod 600 {} + &&
sudo find /var/www/html/temp/ -type f -exec chmod 660 {} + &&
sudo find /var/www/html/ -type d -name "log" -exec chmod 770 -R {} + &&
sudo find /var/www/html/log/ -type f -exec chmod 660 {} + &&
sudo find /var/www/configs/ -type d -name "log" -exec chmod 770 -R {} + &&
sudo chmod 770 /var/www/store/ &&
sudo chmod 770 -R /var/www/store/sql
```

**.htaccess File Content:**

 `nano /var/www/html/.htaccess`

``` apacheconf
AddDefaultCharset UTF-8
Options -Indexes
<IfModule mod_headers.c>
Header set Strict-Transport-Security "max-age=31536000;includeSubDomains;preload" env=HTTPS
</IfModule>
<IfModule mod_rewrite.c>
Options +FollowSymLinks -MultiViews
RewriteEngine On
RewriteCond %{REQUEST_METHOD} !POST
RewriteCond %{THE_REQUEST} ^[A-Z]{3,9}\ /([^\ ]+)\.php
RewriteRule ^/?(.*)\.php$ /$1 [L,R=301]
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^/?(.*)$ /$1.php [L]
RewriteRule (^|/)\.([^/]+)(/|$) - [L,F]
RewriteRule (^|/)([^/]+)~(/|$) - [L,F]
</IfModule>
```

**Apache config edits**

 `sudo nano /etc/apache2/apache2.conf`

``` apacheconf
<Directory /var/www/>
  Options Indexes FollowSymLinks
  AllowOverride All
  Options -Indexes
  Require all granted
</Directory>

ServerSignature Off
ServerTokens Prod
FileETag None
```

**Cron config:**

``` sql
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

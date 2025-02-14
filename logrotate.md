
# logrotate

these files will be created during first install of Emvicy 
by using `\MyMVCInstaller::createLogrotateFiles`, which is part of `/application/init/util/checkInstall.php`

The paths are auto-added and match the installation paths.

------------------------------------------------------------------------------------------------------------------------

## logrotate.conf

_Location_  
~~~
application/logrotate.conf
~~~

_Example: Content_
~~~bash
"/var/www/html/application/log/*.log"
{
    rotate 2
    daily
    su guido guido
    create 640 guido guido
    compress
    missingok
    notifempty
    copytruncate
    size=250k
}
~~~

------------------------------------------------------------------------------------------------------------------------

## logrotate.sh

_Location_
~~~
application/logrotate.sh
~~~

_Example: Content_  
~~~bash
#!/usr/bin/bash
/usr/sbin/logrotate -v -s /tmp/67af528fe38f7 /var/www/html/application/logrotate.conf
~~~
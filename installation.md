# Installation

- [Get Emvicy](#get-emvicy)
  - [Initialize](#initialize-emvicy-)
  - [Run](#run-emvicy)
- [Using `ddev`](#ddev)
- [Requirements](#Requirements)

---

<a id="get-emvicy"></a>
## Get Emvicy

clone the `2.x` repository branch - this way you have the possibility to perform updates that are available for this branch (requires `git` to be installed).
_command_  
~~~bash
git clone --branch 2.x https://github.com/Emvicy/Emvicy.git Emvicy_2.x;
~~~

- alternatively get Emvicy `2.x` **branch head**: https://github.com/Emvicy/Emvicy/archive/refs/heads/2.x.zip
- alternatively get the **latest stable** Emvicy Release of Emvicy from <a href="https://github.com/Emvicy/Emvicy/releases/latest" target="_blank">`https://github.com/Emvicy/Emvicy/releases/latest`</a> _(🛈 latest stable Releases may be relate on other branches than `2.x`)_

---

<a id="initialize-emvicy-"></a>
### Initialize    

cd into the root folder of Emvicy and run `emvicy`

~~~bash
cd Emvicy_2.x/; php emvicy;
~~~
 
- A new Environment config file `/.env` will be created automatically containing `MVC_ENV=develop` (see [/2.x/configuration#Environment](/2.x/configuration#Environment)). 
- The Auto-Installer begins to install all necessary files. (In case of errors, a text will prompt up showing details about what went wrong). This may take a moment.

_Example output_  
~~~bash
setup checking
• MVC_ENV is: develop
• User/Group from /public/index.php: admin1(1000) / admin1(1000)
• Installing required Main Application libraries via composer in Background with PID 84623. Please wait.
.......Installation completed.
~~~

---

<a id="run-emvicy"></a>
### Run 

After that, start Emvicy's local development server.

~~~bash
php emvicy serve
~~~

_Example output_  
~~~bash
admin1@erazer:/var/www/html$ php emvicy serve
/usr/bin/php -S 127.0.0.1:1969 -t ./public/
--------------------------------------------------------------------------------
[Sun Feb 2 13:24:12 2025] PHP 8.4.3 Development Server (http://127.0.0.1:1969) started
~~~


Now you can call your application in your web browser at <a href="http://127.0.0.1:1969" target="_blank">`http://127.0.0.1:1969`</a>.

_You should see this Frontend_  
![Emvicy Installation](/doc/2.x/getting-started/emvicy-installation.png)

---

<a id="ddev"></a>
## Using `ddev`

_🛈 This command installs Emvicy2 at the location where it gets called and configures and runs it as a `ddev` project._  
~~~bash
bash <(curl -s https://emvicy.com/Installers/Emvicy_2.x_ddev.sh);
~~~
- for more Information about this Installer see <a href="https://github.com/emvicy/Installers?tab=readme-ov-file#emvicy2ddev" target="_blank">github.com/emvicy/Installers?tab=readme-ov-file#emvicy2ddev</a>

**disclaimer**  
- I do not accept responsibility for any damage, errors or anything whatsoever caused by running or using this script.
- You use this script at your own risk.

Find out more about <a href="https://ddev.com/" target="_blank">ddev</a> and how to install.
If you are not yet familiar with this great tool, I highly recommend that you take a look at it. It will make your development much easier.

---

<a id="Requirements"></a>
## Requirements

**Operating System**

- Linux
- binaries available: `sed, find, grep, mv, xargs, rm, ps`

**PHP**

- Version: `>=8.4`

You also need some PHP-Extensions installed and PHP-functions enabled as listed below.

_Required PHP Extensions_  
~~~
Core
ctype
curl
date
dom
fileinfo
filter
iconv
json
mbstring
Phar
posix
Reflection
session
SimpleXML
standard
SPL
zip
~~~

_Required PHP Functions_  
~~~
mb_strlen
iconv
~~~

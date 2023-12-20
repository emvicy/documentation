
# Installation

- [Get Emvicy](#Get-Emvicy)
- [Initialize Emvicy](#Initialize_Emvicy)
- [Requirements](#Requirements)

---

<a id="Get-Emvicy"></a>
## Get Emvicy

make sure that your machine has PHP >=8.0 installed (see [Requirements](#Requirements)). 

### Installation :: preferred method 🗸

clone the `1.x` repository branch - this way you have the possibility to perform patch-level updates that are available for this branch (requires `git` to be installed).

_command_  
~~~bash
git clone --branch 1.x https://github.com/Emvicy/Emvicy.git Emvicy_1.x;
~~~

🛈 the repository url for this version is: https://github.com/Emvicy/Emvicy/tree/1.x

_see complete installation video_  
<iframe width="560" height="315" src="https://www.youtube.com/embed/Z4YmvQUjq0U?si=3DFrhwWmGFMHwR4m" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Installation :: alternative methods

- get Emvicy `1.x` **branch head**: 📥 https://github.com/Emvicy/Emvicy/archive/refs/heads/1.x.zip
- get the **latest stable** Emvicy Release of Emvicy _(🛈 latest stable Releases may be relate on other branches than `1.x`)_  
  - Go to download page: <a href="https://github.com/Emvicy/Emvicy/releases/latest" target="_blank">`https://github.com/Emvicy/Emvicy/releases/latest`</a>

---

<a id="Initialize_Emvicy"></a>
## Initialize Emvicy    

cd into the root folder of Emvicy and run `emvicy.php`

~~~bash
cd Emvicy_1.x/; 
php emvicy.php
~~~
 
- A new Environment config file `/.env` will be created automatically containing `MVC_ENV=develop` (see [/1.x/configuration#Environment](/1.x/configuration#Environment)). 
- The Auto-Installer begins to install all necessary files. (In case of errors, a text will prompt up showing details about what went wrong). This may take a moment.

_Example output_  
~~~bash
setup checking
• MVC_ENV is: develop
• User/Group from /public/index.php: admin1(1000) / admin1(1000)
• Installing required Main Application libraries via composer in Background with PID 84623. Please wait.
.......Installation completed.
~~~

After that, start Emvicy's local development server.

~~~bash
php emvicy.php serve
~~~

_Example output_  
~~~bash
admin1@erazer:/tmp/foo/Emvicy_1.x$ php emvicy.php serve
/opt/lampp_8.2.0/bin/php-8.2.0 -S 127.0.0.1:1969 -t ./public/
--------------------------------------------------------------------------------
[Sat Dec  2 11:09:12 2023] PHP 8.2.0 Development Server (http://127.0.0.1:1969) started
~~~


Now you can call your application in your web browser at <a href="http://127.0.0.1:1969" target="_blank">`http://127.0.0.1:1969`</a>.

_You should see this Frontend_  
![Emvicy Installation](/doc/1.x/getting-started/emvicy-installation.png)

---

<a id="Requirements"></a>
## Requirements

**Operating System**

- Linux
- binaries available: `sed, find, grep, mv, xargs, rm, ps`

**PHP**

- Version: `>=8.2`

Also you need some PHP-Extensions installed and PHP-functions enabled as listed below.

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

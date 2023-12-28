
# InfoTool

---

activate the InfoTool in your Configuration file by setting `true` for `MVC_INFOTOOL_ENABLE`. 

<div style="border: 3px solid red;padding: 20px;">
⚠ Consider to set to true for <b>develop</b> environments <b>only</b>.<br>
As it lists sensitive data that you do not want to share with other people.
</div>

<br>

~~~php
$aConfig['MVC_INFOTOOL_ENABLE'] = true;
~~~
- see [Example: /modules/Foo/etc/config/Foo/config/develop.php](/1.x/configuration#Modules-environment-config-file-example)
- see [Module's environment config file](/1.x/configuration#Modules-environment-config-file)


after that you should see the InfoTool on your Frontend:

![Emvicy Infotool](/doc/1.x/infotool/emvicy_infotool.png)
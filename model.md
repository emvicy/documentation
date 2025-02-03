
<a id="Model"></a>
## Model

~~~
Controller => Model => Controller
~~~

The "Worker". Here you are receiving commands from Controller and do things with data. Try to avoid implementing business logic here - this always belongs to a Controller.

A Model Class has to be placed inside your module's Model folder (see [/modules/{moduleName}/](/2.x/directory-structure#modules-moduleName)
 
---

<a id="View"></a>
## View

~~~
Controller => View
~~~

The view renders presentation in a particular format, receiving commands from Controller.

A View Class has to be placed inside your module's View folder (see [/modules/{moduleName}/](/2.x/directory-structure#modules-moduleName) 

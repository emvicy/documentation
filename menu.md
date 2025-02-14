
# Menu

- [Configuration](#Configuration)
- [Building a Menu](#building-a-menu)
- [Get a Menu](#get-a-menu)

------------------------------------------------------------------------------------------------------------------------

<a id="Configuration"></a>
## Configuration

create an assoc array in your module's environment config file `_menu.php`.

The key on the first level is intended as an identifier for the respective menu.

Route-tags that you have assigned in routing act as values here.

In this example we create a Menu we call `frontend`:

*Example: `modules/Foo/etc/config/Foo/config/_menu.php`*    
~~~php
<?php

/*
 * Menu Structure
 * "Name" => [
 *      Route::Tag
 * ]
 */
$aConfig['MODULE']['Foo']['Menu'] = [

    // Name
    'frontend' => [
        'PullDown Menu' => [ # Array key names can also be freely assigned and do not need to correspond to an existing tag
             'info',
             'user',
             'imprint',
             'privacyPolicy',
        ],
        'info',
        'user',
        '403',
        '404',
        'imprint',
        'privacyPolicy',
    ],
];
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="building-a-menu"></a>
## Building a Menu

~~~php
Menu::build(
    Config::MODULE()['Menu'],
    bGetPropertiesFromRouteOnTag: true,
    sCallback: '\App\Model\Menu::buildBootstrap5Menu'
);
~~~
- the final creation needs a callback function 
- Emvicy Menu comes with the default callback `\App\Model\Menu::buildBootstrap5Menu` which is used here 

**Events fired**

- `app.model.menu.build.before`; containing `$aMenuConfig`
- `app.model.menu.build.after`; containing `$aMenuConfig`

------------------------------------------------------------------------------------------------------------------------

<a id="get-a-menu"></a>
## Get a Menu

**call a menu in template**

~~~php
{App\Model\Menu::get('frontend')}
~~~

**get content of a menu**

~~~php
$sMenuFrontend = App\Model\Menu::get('frontend');
~~~

_`$sMenuFrontend`_  
~~~html
<ul class="navbar-nav me-auto mb-2 mb-md-0">

<li class="nav-item dropdown" data-isDropDown="1" data-isSubmenu="0">
    <a class="nav-link dropdown-toggle " href="" role="button" data-bs-toggle="dropdown" aria-expanded="false" >
        PullDown Menu
    </a>
<ul class="dropdown-menu shadow">

<li data-isSubmenu="1">
    <a class="dropdown-item " href="/info/" >
        phpinfo()
    </a>
</li>

<li data-isSubmenu="1">
    <a class="dropdown-item " href="/user/" >
        User
    </a>
</li>

<li data-isSubmenu="1">
    <a class="dropdown-item " href="/imprint/*" >
        Imprint
    </a>
</li>

<li data-isSubmenu="1">
    <a class="dropdown-item " href="/privacy-policy/" >
        Privacy Policy
    </a>
</li>
</ul>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/info/" >
        phpinfo()
    </a>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/user/" >
        User
    </a>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/403/" >
        403
    </a>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/404/" >
        404
    </a>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/imprint/*" >
        Imprint
    </a>
</li>

<li class="nav-item" data-isSubmenu="0">
    <a class="nav-link " aria-current="page" href="/privacy-policy/" >
        Privacy Policy
    </a>
</li>
</ul>
~~~

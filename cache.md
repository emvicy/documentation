
# Cache

- [getCache](#getCache)
- [saveCache](#saveCache)
- [autoDeleteCache](#autoDeleteCache)
- [flushCache](#flushCache)
- [Examples](#Examples)
  - [Simple example](#simple-example)
  - [Process a file once at the beginning or if there is a change in the future](#process-a-file-once-at-the-beginning-or-if-there-is-a-change-in-the-future)
  - [Creating a full-page cache](#creating-a-full-page-cache)

---

<a id="getCache"></a>
## `getCache`

gets data from cache by key.

~~~
Cache::getCache(string $sKey = '')
~~~

---

<a id="saveCache"></a>
## `saveCache`

saves data into cache on key.

~~~
Cache::saveCache(string $sKey, $mData) : bool
~~~

---

<a id="autoDeleteCache"></a>
## `autoDeleteCache`

deletes cachefiles after certain time.

~~~
Cache::autoDeleteCache(string $sToken = '', string $sMinutes = null) : bool
~~~

---

<a id="flushCache"></a>
## `flushCache`

flushes cache (deletes all cachefiles immediatly).

~~~
Cache::flushCache() : bool
~~~

---

<a id="Examples"></a>
### Examples

<a id="simple-example"></a>
#### Simple example

Build a cache token based on the method name od the current class.  
Auto-delete the related cache after a certain time.  
If cache content is empty, get the data and save them to the cache for new.

~~~php
$sCacheToken = __METHOD__;

// delete cache relating to `$sCacheToken` after 1 day (60 minutes * 24 = 24h = 1 day)
Cache::autoDeleteCache($sCacheToken, (60 * 24));

$mData = Cache::getCache($sCacheToken);

// cannot find any content in cache by that token;
if (true === empty($mData)
{
    // concrete doing ...
    $mData = 'some data';
        
    // save to cache
    Cache::saveCache($sCacheToken, $mData);
}

// ... do your stuff with $mData ...
~~~

---

<a id="process-a-file-once-at-the-beginning-or-if-there-is-a-change-in-the-future"></a>
#### Process a file once at the beginning or if there is a change in the future

Create an MD5 sum over the file.  
Build a cache token based on the file name.   
Auto-delete the related cache after a certain time.  
If cache returns a different content than the MD5 sum, process the file (...) and then save the cache for new. 

~~~php
// a yaml file we want to get the content of
$sCronYamlFile = Config::get_MVC_MODULE_PRIMARY_STAGING_CONFIG_DIR() . '/_cron.yaml';

// get a md5 checksum of the current yaml file
$sMd5OfFile = md5_file($sCronYamlFile);

// create a unique cache token
$sCacheToken = Strings::seofy(basename($sCronYamlFile));

// delete cache relating to `$sCacheToken` after 1 day (60 minutes * 24 = 24h = 1 day)
Cache::autoDeleteCache($sCacheToken, (60 * 24));    

// if we cannot find any content in cache by that token: process that file
if (Cache::getCache($sCacheToken) !== $sMd5OfFile)
{
    // read the yaml file for new
    $aYamlContent = Yaml::parseFile($sCronYamlFile);
    
    // ... do your stuff with $aYamlContent ...
    
    // save `$sMd5OfFile` to cache by `$sCacheToken`
    Cache::saveCache($sCacheToken, $sMd5OfFile);   
}

~~~

---

<a id="creating-a-full-page-cache"></a>
#### Creating a full-page cache

This shows how a simple full-page cache can be created with the help of event listeners.  
For more Information about Event Handling in Emvicy, please see Chapter [Events](/2.x/events).

~~~php
\MVC\Event::processBindConfigStack([

    // save page to cache
    'mvc.view.renderString.after' => [
        function (string $sRendered) {
                        
            if (
                // if full page caching is enabled
                true === \MVC\Registry::isRegistered('sPageKey') &&
                // there is no related cache yet  
                true === empty(\MVC\Cache::getCache(\MVC\Registry::get('sPageKey')))
            )
            {            
                \MVC\Cache::saveCache(\MVC\Registry::get('sPageKey'), $sRendered);
            }
        }
    ],
    // get page from cache
    'app.controller.__construct.before' => [
        function(\MVC\DataType\DTRequestCurrent $oDTRequestCurrent) {

            // create a cache key based on request uri
            $sPageKey = \MVC\Strings::seofy($oDTRequestCurrent->get_requesturi());
            
            // save that key to registry
            \MVC\Registry::set('sPageKey', $sPageKey);
            
            // delete cache relating to `$sPageKey` after 1 day (60 minutes * 24 = 24h = 1 day)
            \MVC\Cache::autoDeleteCache($sPageKey, (60 * 24)); 
            
            // load from cache
            $sPage = \MVC\Cache::getCache($sPageKey);

            // present cache content if not empty 
            if (false === empty($sPage))
            {
                echo $sPage;
                exit();
            }
        }
    ],
]);
~~~
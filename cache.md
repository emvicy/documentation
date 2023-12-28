
# Cache

- [getCache](#getCache)
- [saveCache](#saveCache)
- [autoDeleteCache](#autoDeleteCache)
- [flushCache](#flushCache)
- [Example](#Example)

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

<a id="Example"></a>
### Example

_complete example_  
~~~php
// a yaml file we want to get the content of
$sCronYamlFile = Config::get_MVC_MODULE_PRIMARY_STAGING_CONFIG_DIR() . '/_cron.yaml';

// get a md5 checksum of that yaml file
$sMd5OfFile = md5_file($sCronYamlFile);

// create a cache token
$sCacheToken = Strings::seofy(basename($sCronYamlFile));

// cannot find any content in cache by that token;
// so we can assume that content of file has changed (or has never been read before)
if (Cache::getCache($sCacheToken) !== $sMd5OfFile)
{
    // read the yaml file for new
    $aYamlContent = Yaml::parseFile($sCronYamlFile);
    
    // save `$sMd5OfFile` to cache by `$sCacheToken`
    Cache::saveCache($sCacheToken, $sMd5OfFile);
}

// ... do your stuff with $aYamlContent ...

// delete cache relating to `$sCacheToken` after 1 day (60 minutes * 24 = 24h = 1 day)
Cache::autoDeleteCache($sCacheToken, (60 * 24));
~~~


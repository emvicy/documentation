
# Cache

- [getCache](#getCache)
- [saveCache](#saveCache)
- [autoDeleteCache](#autoDeleteCache)
- [Example](#Example)

---

<a id="getCache"></a>
## get from Cache

~~~
Cache::getCache(string $sCacheToken);
~~~

---

<a id="saveCache"></a>
## save to Cache

~~~
Cache::saveCache(string $sCacheToken, .. mixed .. );
~~~

---

<a id="autoDeleteCache"></a>
## autoDeleteCache

~~~
Cache::autoDeleteCache(string $sCacheToken, int minutes);
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


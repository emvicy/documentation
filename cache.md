
# Cache

- [getCache](#getCache)
- [saveCache](#saveCache)
- [autoDeleteCache](#autoDeleteCache)
- [flushCache](#flushCache)
- [Examples](#Examples)
  - [simple example](#simple-example)
  - [process a file once at the beginning or if there is a change in the future](#process-a-file-once-at-the-beginning-or-if-there-is-a-change-in-the-future)

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
#### simple example

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

<a id="process-a-file-once-at-the-beginning-or-if-there-is-a-change-in-the-future"></a>
#### process a file once at the beginning or if there is a change in the future

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


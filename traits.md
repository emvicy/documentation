
# Traits

- [TraitDataType](#TraitDataType)
  - [getDocCommentValueOfProperty](#getDocCommentValueOfProperty)

---

<a id="TraitDataType"></a>
## `TraitDataType`

~~~php
use TraitDataType;
~~~

<a id="getDocCommentValueOfProperty"></a>
### `getDocCommentValueOfProperty`

returns the value from a DocCommentKey (such as @var) from a class property.

_Example: read the value from DocComment key `@max` from class property `$iFoo`_    
~~~php
class Index extends \MVC\Controller
{
    use TraitDataType;
    
    /**
     * @min 0
     * @max 100
     * @minlength 1
     * @maxlength 3
     * @var int
     */
    public int $iFoo = 7;    
    
    /**
     * @return void
     * @throws \ReflectionException
     */
	public function index()
	{
	    // get '@max' value from property 'iFoo' 
        $iMax = $this->getDocCommentValueOfProperty('iFoo', '@max');
    }
}
~~~

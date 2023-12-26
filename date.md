
# Date

- [dateIsBetween2Dates](#dateIsBetween2Dates)
- [validateDate](#validateDate)
- [getWeekNumberOnIsoDate](#getWeekNumberOnIsoDate)
- [getAmountOfWeekNumbersOfYear](#getAmountOfWeekNumbersOfYear)

---

<a id="dateIsBetween2Dates"></a>
## `dateIsBetween2Dates`

checks if a simplified ISO-date (Y-m-d) lays in between two other simplified ISO-Dates (Y-m-d).

~~~
Date::dateIsBetween2Dates(string $sDateIsoRangeStart = '', string $sDateIsoRangeEnd = '', string $sDateIso = '') : bool
~~~

_Example_  
~~~php
$bDateIsBetween2Dates = Date::dateIsBetween2Dates(
    '2023-12-24', # start Date
    '2024-12-24', # End Date
    '2024-06-19'  # Date we are interested in
);
~~~

_Result_  
~~~
// type: boolean
true
~~~

---

<a id="validateDate"></a>
## `validateDate`

checks if a requested value equals to the requested date format.

~~~
Date::validateDate(string $sValue, string $sFormat = 'Y-m-d H:i:s') : bool
~~~

_Example_  
~~~php
// true
Date::validateDate('2022-10-09', 'Y-m-d');
~~~

---

<a id="getWeekNumberOnIsoDate"></a>
## `getWeekNumberOnIsoDate`

gives the week number of the requested simplified ISO Date (Y-m-d).

~~~php
// gives 52
Date::getWeekNumberOnIsoDate('2023-12-26');
~~~

---

<a id="getAmountOfWeekNumbersOfYear"></a>
## `getAmountOfWeekNumbersOfYear`

gives the amount of week numbers of the requested year (YYYY).

~~~
Date::getAmountOfWeekNumbersOfYear(int $iYear = 0) : int
~~~

_Examples_  
~~~php

// gives 52
Date::getAmountOfWeekNumbersOfYear(); # if empty, the curent year is set

// gives 53
Date::getAmountOfWeekNumbersOfYear(2020);
~~~

<div align="center">

  <div>parse abbreviated, sloppy, and informal timezone names</div>
  <div><img src="https://cloud.githubusercontent.com/assets/399657/23590290/ede73772-01aa-11e7-8915-181ef21027bc.png" /></div>

  <div align="center">
    <a href="https://npmjs.org/package/timezone-soft">
      <img src="https://img.shields.io/npm/v/timezone-soft.svg?style=flat-square" />
    </a>
    <!-- <a href="https://codecov.io/gh/spencermountain/timezone-soft">
      <img src="https://codecov.io/gh/spencermountain/timezone-soft/branch/master/graph/badge.svg" />
    </a> -->
    <a href="https://unpkg.com/timezone-soft/builds/timezone-soft.min.js">
      <img src="https://badge-size.herokuapp.com/spencermountain/timezone-soft/master/builds/timezone-soft.min.js" />
    </a>
  </div>
  <div align="center">
    <code>npm install timezone-soft</code>
  </div>
  <sub>
    by
    <a href="https://spencermountain.github.io/">Spencer Kelly</a>
  </sub>
  <div align="center">
    <sup><i>(formerly called 'spacetime-informal')</i></sup>
  </div>
</div>
<p></p>

<!-- spacer -->
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>

```js
const soft = require('timezone-soft')

// get an IANA tz from user input
let timezones = soft.find('milwaukee')
// ['America/Chicago']

// render it nicely
let show = soft.display(timezones[0])
// ... { name: 'Central Standard Time', abbrev: 'CST' }
```

<!-- spacer -->
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>


**[IANA timezone codes](https://www.iana.org/time-zones)** are the official reference for timezone information, and is what you should use, whenever possible.

Humans though, *are goofballs*, and use a whole different informal scheme:

---

* In (North) America:  **PST, MST, EST**...
* in Europe (lately): **WEST, CEST, EEST**...
* in Africa:          **EAT, CAT, WAST**...
* in Australia:       **AWST, AEDT, ACST**...
---

#### these line-up with the IANA codes sometimes. 
#### ...other times they don't.


<!-- spacer -->
<img height="15px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>


These names also collide - 

'***IST***' is used to mean:
* '*Indian Stardard Time*'
* '*Irish Stardard Time*'
* '*Israeli Stardard Time*'

These names also produce all-sorts of ambiguities, regarding DST-changes-

Both Winnipeg and Mexico City are **CST**, but have a much different DST schedule:
![image](https://user-images.githubusercontent.com/399657/52489224-b34d0e00-2b8f-11e9-9de8-0688bec52464.png)

*(thanks [timeanddate.com](https://www.timeanddate.com)!)*

-of course, there's a bunch of political/historical/disputed stuff going on, too. Apologies if this library steps into that unknowingly.

<img height="15px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>

...so that's what we're trying to fix - to *'soften'* this exchange, between human and IANA timezone nomenclature, using some *opinionated-but-common-sense* rules and decision-making.

It was originally built for use in the *[spacetime timezone library](https://github.com/spencermountain/spacetime)*.

<!-- spacer -->
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>


### Usage
```js
const soft = require('timezone-soft')

soft.find('EST')
// 'America/New_York'

soft.find('central')
// 'America/Chicago'

soft.find('venezuela')
// 'America/Caracas'

soft.find('south east asia')
// 'Asia/Bangkok'

soft.display('Toronto')
/*{
  standard: { name: 'Eastern Standard Time', abbrev: 'EST' },
  daylight: { name: 'Eastern Daylight Time', abbrev: 'EDT' },
  iana: 'Canada/Toronto'
}*/
```

Typescript/Deno/Webpack:
```js
import soft from 'timezone-soft'
```

it was built to be as forgiving as possible, and return the most common-sense IANA timezone id from user-input.

<div align="center">
  <img height="50px" src="https://user-images.githubusercontent.com/399657/68221814-05ed1680-ffb8-11e9-8b6b-c7528d163871.png"/>
</div>

---

<!-- spacer -->
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>

### DST

Often, the proper timezone name will depend on which date you are referencing.
You can reckon this pretty-easily with [spacetime](https://github.com/spencermountain/spacetime), like this:
```js
const spacetime = require('spacetime')
const soft = require('timezone-soft')

let display = soft.display('montreal')[0]
let show = display.standard.abbreg

// are we in standard time, or daylight time?
let s = spacetime.now(display.iana)
if(display.daylight && s.isDST()){
  show = display.daylight.abbreg
}
console.log(s.time() + ' ' + show)
// '4:20pm EDT'
```

<!-- spacer -->
<img height="25px" src="https://user-images.githubusercontent.com/399657/68221862-17ceb980-ffb8-11e9-87d4-7b30b6488f16.png"/>

work-in-progress!

MIT

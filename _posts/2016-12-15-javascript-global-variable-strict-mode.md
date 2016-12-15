---
layout: post
title: "JavaScript Global Variable Strict Mode"
date: 2016-12-15
---

### Solusi 1

```javascript
var GLOB = null;

(function(){
	'use strict';
	GLOB =' not null';
	console.log(GLOB);
}());

(function(){
	'use strict';
	console.log(GLOB);
})();
```

### Solusi 2

```javascript
(function(){
	"use strict";
	window.glb = null;
}());

(function(){
	'use strict';
	glb = "window glb";
	console.log(glb);
})();

(function(){
	'use strict';
	console.log(glb);
})();
```

### Solusi 3

```javascript
(function(_vg) {
	'use strict';
	_vg.globalz = null;
})(this);

(function(){
	'use strict';
	globalz = "_vg globalz";
	console.log(globalz);
})();

(function(){
	'use strict';
	console.log(globalz);
})();
```
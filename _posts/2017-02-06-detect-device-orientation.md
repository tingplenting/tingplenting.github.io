---
layout: post
title: Detect Device Orientation
date: 2017-02-06 
---

```javascript
function cekOrientation() {
    if (window.matchMedia("(orientation: portrait)").matches) {
        console.log("portrait");
    } else {
        console.log("landscape");
    }
}
```
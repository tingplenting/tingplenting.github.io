---
layout: post
title: Call function when ng-repeat has finished AngularJS
date: 2017-02-06 
---

```javascript
var module = angular.module('app', [])
    .directive('onFinishRender', function ($timeout) {
    return {
        restrict: 'A',
        link: function (scope, element, attr) {
            if (scope.$last === true) {
                $timeout(function () {
                    scope.$emit(attr.onFinishRender);
                });
            }
        }
    }
});
```

### On controller

```javascript
$scope.$on('ngRepeatFinished', function(ngRepeatFinishedEvent) {
    //you also get the actual event object
    //do stuff, execute functions -- whatever...
});
```

### Template

```html
<div ng-repeat="item in items" on-finish-render="ngRepeatFinished">
    <div>{{item.name}}}<div>
</div>
```

[Sumber](http://stackoverflow.com/questions/15207788/calling-a-function-when-ng-repeat-has-finished)
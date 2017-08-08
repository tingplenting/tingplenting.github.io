---
layout: post
title: How to set focus on input field? AngularJS
date: 2017-02-06 
---

### app.js

```javascript
angular
    .module('app')
    .directive('focusMe',focusMe);

focusMe.$inject = ['$timeout','$parse'];

function focusMe ($timeout,$parse) {
    return {
        //scope: true,   // optionally create a child scope
        link: function (scope, element, attrs) {
            var model = $parse(attrs.focusMe);
            scope.$watch(model, function (value) {
                console.log('value: ' + value);
                if (value === true) {
                    $timeout(function () {
                        element[0].focus();
                    });
                }
            });
            // to address @blesh's comment, set attribute value to 'false'
            // on blur event:
            element.bind('blur', function () {
                console.log('blur');
                scope.$apply(model.assign(scope, false));
            });
        }
    };
}
```

### Controller

```javascript
$scope.open = function() {
    $scope.shouldBeOpen = true;
}

$scope.close = function() {
    $scope.shouldBeOpen = false;
}
```
## OR

```javascript
$scope.shouldBeOpen = false;
$scope.open = function() {
    $scope.shouldBeOpen = !$scope.shouldBeOpen;
}
```

### Template

```html
<input type="text" ng-model="myInput" focus-me="focusInput">
```

[Sumber](http://stackoverflow.com/questions/14833326/how-to-set-focus-on-input-field)
[Plunkr](http://plnkr.co/edit/V8PSie?p=preview)

---
layout: post
title: "PHP Variable Scope"
date: 2017-03-12
---

### What is "variable scope"?

Variables have a limited "scope", or "places from which they are accessible". Just because you wrote `$foo = 'bar';` once somewhere in your application doesn't mean you can refer to `$foo` from everywhere else inside the application. The variable `$foo` has a certain scope within which it is valid and only code in the same scope has access to the variable.

### How is a scope defined in PHP?

Very simple: PHP has function scope. That's the only kind of scope separator that exists in PHP. Variables inside a function are only available inside that function. Variables outside of functions are available anywhere outside of functions, but not inside any function. This means there's one special scope in PHP: the global scope. Any variable declared outside of any function is within this global scope.

### Example:

```php
<?php

$foo = 'bar';

function myFunc() {
    $baz = 42;
}
```

`$foo` is in the global scope, `$baz` is in a local scope inside `myFunc`. Only code inside `myFunc` has access to `$baz`. Only code outside `myFunc` has access to $foo. Neither has access to the other:

```php
<?php

$foo = 'bar';

function myFunc() {
    $baz = 42;

    echo $foo;  // doesn't work
    echo $baz;  // works
}

echo $foo;  // works
echo $baz;  // doesn't work
```

### Scope and included files

File boundaries do not separate scope:

*a.php*

```php
<?php

$foo = 'bar';
```

*b.php*

```php
<?php

include 'a.php';

echo $foo;  // works!
```

The same rules apply to `include`d code as applies to any other code: only `functions` separate scope. For the purpose of scope, you may think of including files like copy and pasting code:

*c.php*

```php
<?php

function myFunc() {
    include 'a.php';

    echo $foo;  // works
}

myFunc();

echo $foo;  // doesn't work!
```

In the above example, `a.php` was included inside `myFunc`, any variables inside `a.php` only have local function scope. Just because they *appear* to be in the global scope in `a.php` doesn't necessarily mean they are, it actually depends in which context that code is included/executed.

### What about functions inside functions and classes?

Every new `function` declaration introduces a new scope, it's that simple.

##### (anonymous) functions inside functions

```php
function foo() {
    $foo = 'bar';

    $bar = function () {
        // no access to $foo
        $baz = 'baz';
    };

    // no access to $baz
}
```

##### classes

```php
$foo = 'foo';

class Bar {

    public function baz() {
        // no access to $foo
        $baz = 'baz';
    }

}

// no access to $baz
```

### What is scope good for?

Dealing with scoping issues may seem annoying, but **limited variable scope is essential to writing complex applications!** If every variable you declare would be available from everywhere else inside your application, you'd be stepping all over your variables with no real way to track what changes what. There are only so many sensible names you can give to your variables, you probably want to use the variable "`$name`" in more than one place. If you could only have this unique variable name once in your app, you'd have to resort to really complicated naming schemes to make sure your variables are unique and that you're not changing the wrong variable from the wrong piece of code.

Observe:

```php
function foo() {
    echo $bar;
}
```

If there was no scope, what would the above function do? Where does `$bar` come from? What state does it have? Is it even initialized? Do you have to check every time? This is not maintainable. Which brings us to...

### Crossing scope boundaries

##### The right way: passing variables in and out

```php
function foo($bar) {
    echo $bar;
    return 42;
}
```

The variable `$bar` is explicitly coming into this scope as function argument. Just looking at this function it's clear where the values it works with originate from. It then explicitly *returns* a value. The caller has the confidence to know what variables the function will work with and where its return values come from:

```php
$baz   = 'baz';
$blarg = foo($baz);
```

##### Extending the scope of variables into anonymous functions

```php
$foo = 'bar';

$baz = function () use ($foo) {
    echo $foo;
};

$baz();
```

The anonymous function explicitly includes `$foo` from its surrounding scope. Note that this is not the same as *global* scope.

### The wrong way: global

As said before, the global scope is somewhat special, and functions can explicitly import variables from it:

```php
$foo = 'bar';

function baz() {
    global $foo;
    echo $foo;
    $foo = 'baz';
}
```

This function uses and modifies the global variable `$foo`. **Do not do this!** (Unless you really really really really know what you're doing, and even then: don't!)

All the caller of this function sees is this:

```php
baz(); // outputs "bar"
unset($foo);
baz(); // no output, WTF?!
baz(); // outputs "baz", WTF?!?!!
```

There's no indication that this function has any *side effects*, yet it does. This very easily becomes a tangled mess as some functions keep modifying and *requiring* some global state. You want functions to be stateless, acting only on their inputs and returning defined output, however many times you call them.

You should avoid using the global scope in any way as much as possible; most certainly you should not be "pulling" variables out of the global scope into a local scope.

source: [http://stackoverflow.com/a/16959577](http://stackoverflow.com/a/16959577)

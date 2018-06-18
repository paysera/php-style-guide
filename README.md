Paysera PHP style guide
=====

1. [PSR-1](#psr-1)

    1.1 [Overview](#1-overview)
    
    1.2 [Files](#2-files)
    
    1.3 [Namespace and Class Names](#3-namespace-and-class-names)
    
    1.4 [Class Constants, Properties and Methods](#4-class-constants-properties-and-methods)
        
2. [PSR-2](#psr-2)

    2.1 [Overview](#1-overview)
    
    2.2 [General](#2-general)
    
    2.3 [Namespace and Use Declarations](#3-namespace-and-use-declarations)
    
    2.4 [Classes, Properties and methods](#4-classes-properties-and-methods)
    
    2.5 [Control Structures](#5-control-structures)
    
    2.6 [Closures](#6-closures)
    
    2.7 [Conclusion](#7-conclusion)
         
3. [PHP basics](#php-basics)
    
    3.1 [Basics](#basics)
        
    3.2. [Code style](#code-style)
    
    3.3 [Usage of PHP features](#usage-of-php-features)
       
    3.4 [Comments](#comments)
      
    3.5 [IDE Warnings](#ide-warnings)
    
4. [Main patterns](#main-patterns)

    4.1 [Thin model](#thin-model)
    
    4.2 [Services without run-time state](#services-without-run-time-state)
    
    4.3 [Composition over inheritance](#composition-over-inheritance)
    
    4.4 [Services (objects) over classes, configuration over run-time parameters](#services-(objects)-over-classes-configuration-over-run-time-parameters)
    
    4.5 [Small, understandable methods](#small-understandable-methods)
    
    4.6 [Dependencies](#dependencies)
    
    4.7 [Services](#services)

5. [REST in PHP](#rest-in-php)
    
    5.1 [REST controllers](#rest-controllers)
    
    5.2 [Result providers](#result-providers)
    
    5.3 [Extending model](#extending-model)
    
    5.4 [Permissions](#permissions)
    
    5.5 [Pagination](#pagination)
    
6. [Smartweb related conventions](https://github.com/paysera/php-style-guide/tree/master/smartweb-related-conventions)
    
    6.1 [New features](#new-features)
    
    6.2 [External dependencies](#external-dependencies)
    
    6.3 [Database migrations](#database-migrations)
    
    6.4 [Bank integration](#bank-integration)
    
7. [Symfony related conventions](#symfony-related-conventions)

    7.1 [Configuration](#configuration)
        
    7.2 [Repositories](#repositories)
        
    7.3 [Entities](#entities)
        
    7.4 [Doctrine](#doctrine)
        
    7.5 [Controllers](#controllers)
        
    7.6 [Events](#events)
        
    7.7 [Directory structure](#directory-structure)
        
    7.8 [Twig](#twig)
        
    7.9 [Commands](#commands)
        
    7.10 [Symfony version and new projects](#symfony-version-and-new-projects)
    
8. [Handling sensitive values](#handling-sensitive-values)
    
    8.1 [Logging](#logging)
    
    8.2 [Passing sensitive values as arguments](#passing-sensitive-values-as-arguments)
    
    8.3 [Storing sensitive values in entities](#storing-sensitive-values-in-entities)
    
9. [Composer conventions](#composer-conventions)

    9.1 [Semantic versioning](#semantic-versioning)
    
    9.2 [Backward incompatible changes](#backward-incompatible-changes)
    
    9.3 [Releases](#releases)
    
    9.4 [Reviewing tagging policy](#reviewing-tagging-policy)
    
    9.5 [Initial library development](#initial-library-development)
    
    9.6 [Constraints on library versions](#constraints-on-library-versions)
    
    9.7 [Constraints on vendors](#constraints-on-vendors)
    
    9.8 [Making minor/patch release on previous version](#making-minorpatch-release-on-previous-version)
    
10. [Deprecated conventions](#deprecated-conventions)
    
    10.1 [REST proxy controllers](#rest-proxy-controllers)
    
    10.2 [Splitting controllers by use-case (administration / frontend)](#splitting-controllers-by-use-case-(administration--frontend))
    
    10.3 [--force](#--force)
    
    10.4 [markAs*](#markas)
    
    10.5 [common libraries / bundles](#common-libraries--bundles)

# PSR-1

This section of the standard comprises what should be considered the standard
coding elements that are required to ensure a high level of technical
interoperability between shared PHP code.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md


## 1. Overview

- Files MUST use only `<?php` and `<?=` tags.

- Files MUST use only UTF-8 without BOM for PHP code.

- Files SHOULD *either* declare symbols (classes, functions, constants, etc.)
  *or* cause side-effects (e.g. generate output, change .ini settings, etc.)
  but SHOULD NOT do both.

- Namespaces and classes MUST follow an "autoloading" PSR: [[PSR-0], [PSR-4]].

- Class names MUST be declared in `StudlyCaps`.

- Class constants MUST be declared in all upper case with underscore separators.

- Method names MUST be declared in `camelCase`.


## 2. Files


### 2.1. PHP Tags

PHP code MUST use the long `<?php ?>` tags or the short-echo `<?= ?>` tags; it
MUST NOT use the other tag variations.

### 2.2. Character Encoding

PHP code MUST use only UTF-8 without BOM.

### 2.3. Side Effects

A file SHOULD declare new symbols (classes, functions, constants,
etc.) and cause no other side effects, or it SHOULD execute logic with side
effects, but SHOULD NOT do both.

The phrase "side effects" means execution of logic not directly related to
declaring classes, functions, constants, etc., *merely from including the
file*.

"Side effects" include but are not limited to: generating output, explicit
use of `require` or `include`, connecting to external services, modifying ini
settings, emitting errors or exceptions, modifying global or static variables,
reading from or writing to a file, and so on.

The following is an example of a file with both declarations and side effects;
i.e, an example of what to avoid:

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


## 3. Namespace and Class Names

Namespaces and classes MUST follow an "autoloading" PSR: [[PSR-0], [PSR-4]].

This means each class is in a file by itself, and is in a namespace of at
least one level: a top-level vendor name.

Class names MUST be declared in `StudlyCaps`.

Code written for PHP 5.3 and after MUST use formal namespaces.

For example:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

Code written for 5.2.x and before SHOULD use the pseudo-namespacing convention
of `Vendor_` prefixes on class names.

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

## 4. Class Constants, Properties, and Methods


The term "class" refers to all classes, interfaces, and traits.

### 4.1. Constants

Class constants MUST be declared in all upper case with underscore separators.
For example:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Properties

This guide intentionally avoids any recommendation regarding the use of
`$StudlyCaps`, `$camelCase`, or `$under_score` property names.

Whatever naming convention is used SHOULD be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level.

### 4.3. Methods

Method names MUST be declared in `camelCase()`.

# PSR-2

This guide extends and expands on [PSR-1](#psr-1), the basic coding standard.

The intent of this guide is to reduce cognitive friction when scanning code
from different authors. It does so by enumerating a shared set of rules and
expectations about how to format PHP code.

The style rules herein are derived from commonalities among the various member
projects. When various authors collaborate across multiple projects, it helps
to have one set of guidelines to be used among all those projects. Thus, the
benefit of this guide is not in the rules themselves, but in the sharing of
those rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md


## 1. Overview

- Code MUST follow a "coding style guide" PSR [[PSR-1]].

- Code MUST use 4 spaces for indenting, not tabs.

- There MUST NOT be a hard limit on line length; the soft limit MUST be 120
  characters; lines SHOULD be 80 characters or less.

- There MUST be one blank line after the `namespace` declaration, and there
  MUST be one blank line after the block of `use` declarations.

- Opening braces for classes MUST go on the next line, and closing braces MUST
  go on the next line after the body.

- Opening braces for methods MUST go on the next line, and closing braces MUST
  go on the next line after the body.

- Visibility MUST be declared on all properties and methods; `abstract` and
  `final` MUST be declared before the visibility; `static` MUST be declared
  after the visibility.
  
- Control structure keywords MUST have one space after them; method and
  function calls MUST NOT.

- Opening braces for control structures MUST go on the same line, and closing
  braces MUST go on the next line after the body.

- Opening parentheses for control structures MUST NOT have a space after them,
  and closing parentheses for control structures MUST NOT have a space before.

### 1.1. Example

This example encompasses some of the rules below as a quick overview:

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

## 2. General


### 2.1 Basic Coding Standard

Code MUST follow all rules outlined in [PSR-1].

### 2.2 Files

All PHP files MUST use the Unix LF (linefeed) line ending.

All PHP files MUST end with a single blank line.

The closing `?>` tag MUST be omitted from files containing only PHP.

### 2.3. Lines

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers
MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD
be split into multiple subsequent lines of no more than 80 characters each.

There MUST NOT be trailing whitespace at the end of non-blank lines.

Blank lines MAY be added to improve readability and to indicate related
blocks of code.

There MUST NOT be more than one statement per line.

### 2.4. Indenting

Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> N.b.: Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line 
> alignment.

### 2.5. Keywords and True/False/Null

PHP [keywords] MUST be in lower case.

The PHP constants `true`, `false`, and `null` MUST be in lower case.

[keywords]: http://php.net/manual/en/reserved.keywords.php



## 3. Namespace and Use Declarations

When present, there MUST be one blank line after the `namespace` declaration.

When present, all `use` declarations MUST go after the `namespace`
declaration.

There MUST be one `use` keyword per declaration.

There MUST be one blank line after the `use` block.

For example:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```


## 4. Classes, Properties, and Methods

The term "class" refers to all classes, interfaces, and traits.

### 4.1. Extends and Implements

The `extends` and `implements` keywords MUST be declared on the same line as
the class name.

The opening brace for the class MUST go on its own line; the closing brace
for the class MUST go on the next line after the body.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

Lists of `implements` MAY be split across multiple lines, where each
subsequent line is indented once. When doing so, the first item in the list
MUST be on the next line, and there MUST be only one interface per line.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```

### 4.2. Properties

Visibility MUST be declared on all properties.

The `var` keyword MUST NOT be used to declare a property.

There MUST NOT be more than one property declared per statement.

Property names SHOULD NOT be prefixed with a single underscore to indicate
protected or private visibility.

A property declaration looks like the following.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

### 4.3. Methods

Visibility MUST be declared on all methods.

Method names SHOULD NOT be prefixed with a single underscore to indicate
protected or private visibility.

Method names MUST NOT be declared with a space after the method name. The
opening brace MUST go on its own line, and the closing brace MUST go on the
next line following the body. There MUST NOT be a space after the opening
parenthesis, and there MUST NOT be a space before the closing parenthesis.

A method declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```    

### 4.4. Method Arguments

In the argument list, there MUST NOT be a space before each comma, and there
MUST be one space after each comma.

Method arguments with default values MUST go at the end of the argument
list.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis
and opening brace MUST be placed together on their own line with one space
between them.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```

### 4.5. `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations MUST precede the
visibility declaration.

When present, the `static` declaration MUST come after the visibility
declaration.

```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

### 4.6. Method and Function Calls

When making a method or function call, there MUST NOT be a space between the
method or function name and the opening parenthesis, there MUST NOT be a space
after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before
each comma, and there MUST be one space after each comma.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

## 5. Control Structures

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- There MUST be one space between the closing parenthesis and the opening
  brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how
the structures look, and reduces the likelihood of introducing errors as new
lines get added to the body.


### 5.1. `if`, `elseif`, `else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `elseif` are on the same line as the
closing brace from the earlier body.

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

The keyword `elseif` SHOULD be used instead of `else if` so that all control
keywords look like single words.


### 5.2. `switch`, `case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement MUST be indented once
from `switch`, and the `break` keyword (or other terminating keyword) MUST be
indented at the same level as the `case` body. There MUST be a comment such as
`// no break` when fall-through is intentional in a non-empty `case` body.

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


### 5.3. `while`, `do while`

A `while` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```php
<?php
while ($expr) {
    // structure body
}
```

Similarly, a `do while` statement looks like the following. Note the placement
of parentheses, spaces, and braces.

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

A `for` statement looks like the following. Note the placement of parentheses,
spaces, and braces.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`
    
A `foreach` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

A `try catch` block looks like the following. Note the placement of
parentheses, spaces, and braces.

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```

## 6. Closures

Closures MUST be declared with a space after the `function` keyword, and a
space before and after the `use` keyword.

The opening brace MUST go on the same line, and the closing brace MUST go on
the next line following the body.

There MUST NOT be a space after the opening parenthesis of the argument list
or variable list, and there MUST NOT be a space before the closing parenthesis
of the argument list or variable list.

In the argument list and variable list, there MUST NOT be a space before each
comma, and there MUST be one space after each comma.

Closure arguments with default values MUST go at the end of the argument
list.

A closure declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

Argument lists and variable lists MAY be split across multiple lines, where
each subsequent line is indented once. When doing so, the first item in the
list MUST be on the next line, and there MUST be only one argument or variable
per line.

When the ending list (whether or arguments or variables) is split across
multiple lines, the closing parenthesis and opening brace MUST be placed
together on their own line with one space between them.

The following are examples of closures with and without argument lists and
variable lists split across multiple lines.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

Note that the formatting rules also apply when the closure is used directly
in a function or method call as an argument.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


## 7. Conclusion

There are many elements of style and practice intentionally omitted by this
guide. These include but are not limited to:

- Declaration of global variables and global constants

- Declaration of functions

- Operators and assignment

- Inter-line alignment

- Comments and documentation blocks

- Class name prefixes and suffixes

- Best practices

Future recommendations MAY revise and extend this guide to address those or
other elements of style and practice.

# PHP Basics

## Basics

### PHP code-level

For our projects and libraries we use PHP 5.5.

Exception is for our libraries for integrating with services we provide - there we use PHP 5.2 (libwebtopay, Wallet API client).

### Globals

We do not use global variables, constants and functions.

### Files

We put every class to it’s own file.

### Exceptions for code style usage

If we modify legacy code where some other conventions are used, we can use the same style as it is used these.

## Code style

### Commas in arrays

If we split array items into separate lines, all items comes in it’s own line. Comma must be after each (including last) element.

```php
<?php
['a', 'b', 'c'];
[
    'a',
    'b',
    'c', // notice the comma here
];
```

### Splitting in several lines

If we split statement into separate lines, we use these rules:

-   `&&`, `||` etc. comes in the beginning of the line, not the end

-   if we split some condition into separate lines, every part comes in it’s separate line (including first - on the new line, and last - new line after). All of the parts are indented

Some examples:

```php
<?php

return ($a && $b || $c && $d);

return (
    $a && $b
    || $c && $d
);

return ((
    $a
    && $b
) || (
    $c
    && $d
));

return (
    (
        $a
        && $b
    )
    || (
        $c
        && $d
    )
);

return ($a && in_array($b, [1, 2, 3]));

return (
    $a
    && in_array($b, [1, 2, 3])
);

return ($a && in_array(
    $b,
    [1, 2, 3]
));

return ($a && in_array($b, [
    1,
    2,
    3,
]));

return ($a && in_array(
    $b,
    [
        1,
        2,
        3,
    ]
));

return (
    $a
    && in_array(
        $b,
        [
            1,
            2,
            3,
        ]
    )
);
```

This is wrong:

```php
<?php
return [
        'a',    // use 4 spaces, not 8 here
        'b',
    ];
```

### Chained method calls

When making chain method calls, we put semicolon on it’s own separate line, chained method calls are indented and comes in it’s own line.

Example:

```php
<?php
return $this->createQueryBuilder('a')
    ->join('a.items', 'i')
    ->andWhere('i.param = :param')
    ->setParameter('param', $param)
    ->getQuery()
    ->getResult()
;  // semicolon here
```

### Constructors

We always add `()` when constructing class, even if constructor takes no arguments:

```php
<?php
$value = new ValueClass();
```

### Variable, class and function naming

#### Full names

We use full names, not abbreviations: `$entityManager` instead of `$em`, `$exception` instead of `$e`.

#### Class naming

We use nouns for class names.

For services we use some suffix to represent the job of that service, usually \*er:

-   `manager`

-   `normalizer`

-   `provider`

-   `updater`

-   `controller`

-   `registry`

-   `resolver`

We do not use `service` as a suffix, as this does not represent anything (for example `PageService`).

We use object names only for entities, not for services (for example `Page`).

#### Interface naming

We always add suffix `Interface` to interfaces, even if interface name would be adjective.

> **Why?** If we have base class witch implements the interface, we would have name clash. For example, `ContainerAware` and `ContainerAwareInterface`.


#### Property naming

We use nouns or adjectives for property names, not verbs or questions.

```php
<?php
class Entity
{
    private $valid;       // NOT $isValid
    private $checkNeeded; // NOT $check
}
```

#### Method naming

We use verbs for methods that perform action and/or return something, questions only for methods which return boolean.

Questions start with `has`, `is`, `can` - these cannot make any side-effect and always return `boolean`.

For entities we use `is*` or `are*` for boolean getters, `get*` for other getters, `set*` for setters, `add*` for adders, `remove*` for removers.

We always make correct English phrase from method names, this is more important that naming method to `'is' + propertyName`.

```php
<?php
interface EntityInterface
{
    public function isValid();
    public function isCheckNeeded();
    public function getSomeValue();
    public function canBeChecked();
    public function areTransactionsIncluded();

    // WRONG:
    public function isCheck();
    public function isNeedsChecking();
    public function isTransactionsIncluded();
    public function getIsValid();
}
```

```php
<?php
interface ControllerInterface
{
    /**
     * @return boolean
     */
    public function canAccess($groupId);

    /**
     * @throws AccessDeniedException
     */
    public function checkPermissions($groupId);
}
```

### Order of methods

We provide methods and fields in the following order:

-   constants
-   static fields
-   fields
-   constructor
-   constructing class methods
-   static methods
-   class methods

Constructing class methods are those used in service construction phase, usually by dependency injection container.

> **Why no ordering by visibility?** `protected` and `private` methods usually are used from single place in the code, so ordering by functionality (versus by visibility) makes understanding the class easier. If we just want to see class public interface, we can use IDE features (method names ordered by visibility or/and name, including or excluding inherited methods etc.)


### Directories and namespaces

#### Singular namespaces

We use singular for namespaces: `Service`, `Bundle`, `Entity`, `Controller` etc.

Exception: if English word does not have singular form.

#### No `*Interface` namespaces

We do not make directories just for interfaces, we put them together with services by related functionality (no `ServiceInterface` namespace).

#### Different namespaces and service names

We use abstractions for namespaces, not service names. For example, `UserMerge` or `UserMerging`, not `UserMergeManager`.

### Comparison order

If we compare to static value (`true`, `false`, `null`, hard-coded integer or string), we put static value in the right of comparison operator.

Wrong: `null === $something`

Right: `$something === null`

### Namespaces and use statements

If class has a namespace, we use `use` statements instead of providing full namespace. This applies to php-doc comments, too.

Wrong:

```php
<?php

namespace Some/Namespace;

// ...

/**
 * @var \Vendor\Namespace\Entity\Value $value
 */
public function setSomething(\Vendor\Namespace\Entity\Value $value);
```

Right:

```php
<?php

namespace Some/Namespace;

use Vendor\Namespace\Entity\Value;

/**
 * @var Value $value
 */
public function setSomething(Value $value);
```

## Usage of PHP features

### Condition results

If condition result is boolean, we do not use condition at all.

Do NOT do this:

```php
<?php
$a = $d && $e ? false : true;

if ($a && $b) {
    return true;
}

return false;
```

Do this:

```php
<?php
$a = !($d && $e);

return $a && $b;
```

### Logical operators

We use `&&` and `||` instead of `and` and `or`.

### Strict comparison operators

We use `===` (`!==`) instead of `==` (`!=`) everywhere, except cases where we explicitly want to check ignoring the type.

Same applies for `in_array` - we always pass third argument as `true` for strict checking.

We convert or validate types when data is entering the system - on normalizers, forms or controllers.

> **Why?** Due to security reasons and to avoid bugs.


```php
php > var_dump(md5('240610708') == md5('QNKCDZO'));
bool(true)
php > file_put_contents('/tmp/a', '<?xml version="1.0" encoding="UTF-8"?><B4B xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://swedbankgateway.net/valid/hgw-response.xsd"><Alert><AccountInfo><IBAN>abc</IBAN></AccountInfo></Alert></B4B>');
php > $a = simplexml_load_file('/tmp/a');
php> var_dump($a == false);
bool(true)
php > var_dump(in_array(1, ['1a2b']));
bool(true)
php > var_dump(in_array(true, ['a']));
bool(true)
```

### Converting to boolean

We use type casting operator to convert between types:

```php
<?php
return (bool)$something;
```

> This should not be used at all. If variable is not `boolean` already, check it’s available values explicitly.


### Comparing to boolean

We do not use `true`/`false` keywords when checking variable which is already `boolean`.

Wrong:

```php
<?php
return $valid === true;
return $valid === false;
return false !== $valid;
```

Correct:

```php
<?php
return $valid;
return !$valid;
```

Exception: When variable can not only be boolean, but also `int` or `null`.

```php
<?php
return strpos('needle', 'haystack') === false;
```

### Comparing to `null`

When comparing to `null`, we always compare explicitly.

```php
<?php

function func(ValueClass $value = null)
{
    return $value !== null ? $value->getSomething() : null;
}
```

We also do not use `is_null` function for comparing.

### Assignments in conditions

We do not use assignments inside conditional statements.

Exception: in a `while` loop condition.

Wrong:

```php
<?php
if (($b = $a->get()) !== null && ($c = $b->get()) !== null) {
    $c->do();
}
if ($project = $this->findProject()) {

}
```

Correct:

```php
<?php
$b = $a->get();
if ($b !== null) {
    $c = $b->get();
    if ($c !== null) {
        $c->do();
    }
}

$project = $this->findProject();
if ($project !== null) {

}
```

> **Why?** We save a few lines of code but code is less understandable - we make several actions at once. Furthermore, as we explicitly compare to `null`, conditional-assignment statements becomes complicated.


### Unnecessary variables

We avoid unnecessary variables.

Wrong:

```php
<?php

function find($needle, $haystack)
{
    $found = false;
    foreach ($haystack as $item) {
        if ($needle === $item) {
            $found = true;
            break;
        }
    }
    return $found;
}

function getSomething()
{
    $a = get();
    return $a;
}
```

Correct:

```php
<?php

function find($needle, $haystack)
{
    foreach ($haystack as $item) {
        if ($needle === $item) {
            return true;
        }
    }
    return false;
}

function getSomething()
{
    return get();
}
```

### Reusing variables

We do not set value to variable passed as an argument.

We do not change the type of the variable.

```php
<?php

// ...

public function thisIsWrong($number, $text, Request $request)
{
    $number = (int)$number;  // Illegal: we 1) change argument value 2) change it's type
    $text .= ' ';  // Illegal: we change argument value
    $document = $request->get('documentId');
    $document = $this->repository->find($document); // Illegal: we change variable's type
    // ...
}

public function thisIsCorrect($numberText, $text, Request $request)
{
    $number = (int)$numberText;
    $modifiedText = $text . ' ';
    $documentId = $request->get('documentId');
    $document = $this->repository->find($documentId);
    // ...
}
```

### Unnecessary structures

We avoid unnecessary structures.

Wrong:

```php
<?php
if ($first) {
    if ($second) {
        do();
    }
}
```

Correct:

```php
<?php
if ($first && $second) {
    do();
}
```

### Static methods

We do use static methods only in these cases:

-   to create an entity for fluent interface, if PHP version in the project is lower thant 5.4. We use `(new Entity())->set('a')` in 5.4 or above

-   to give available values for some field of an entity, used in validation

### Converting to string

We do not use `__toString` method for main functionality, only for debugging purposes.

> **Why?** We cannot throw exceptions in this method, we cannot put it in interface, we cannot change return type or give any arguments - refactoring is impossible. Also if debugging uses \_\_toString method, we cannot change it to get any additional information.


### Double quotes

We do not use double quotes in simple strings.

We use double quotes only under these conditions:

-   Single quote is used repeatedly inside the string
-   Some special symbols are used, like `"\n"`

If we use double quotes, we do not use auto variable includes (`"Hello $name!"` or `"Hello {$object->name}!"`).

### Visibility

#### Public properties

We don’t use public properties. We avoid magic methods and dynamic properties - all used properties must be defined.

Example:

```php
<?php
class A {
    private $a;      // this must be defined as a property
    public $b;       // this is illegal
    public static $availableValues = ['a', 'b'];       // this is illegal

    public function __construct($a)
    {
        $this->a = $a;
    }
}
```

#### Protected vs private

We prefer `private` over `protected` as it constraints the scope - it's easier to refactor, find usages, plan possible changes in code. Also IDE can warn about unused methods or properties.

We use `protected` when we intend some property or method to be overwritten if necessary.

### Functions

#### `count`

We use `count` instead of `sizeof`.

#### `is_null`

We use compare to `null` using `===` instead of `is_null` function. For example:

```php
if ($result === null) {
    // ...
```

### `str_replace`

We do not use `str_replace` if we need to remove prefix or suffix - this can lead to replacing more content unintentionally.

```php
<?php
function removePrefix($text, $prefix)
{
    if (substr($text, 0, strlen($prefix)) === $prefix) { // strpos === 0 is also possible
        $text = substr($text, strlen($prefix));
    }
    return $text;
}

assertSame('Some asd text', removePrefix('asdSome asd text', 'asd'));
```

### Return and argument types

#### Return types

We always return value of one type. Optionally, we can return `null` when using any other return type, too.

For example, we can*not* return `boolean|string` or `SuccessResult|FailureResult` (if `SuccessResult` and `FailureResult` has no common class or interface; if they do, we document to return that interface instead).

We can return `SomeClass|null` or `string|null`.

#### Argument types

Same applies for any argument.

#### Passing ID

Exception: When making query from repository, both `int|Entity` can be taken (object or it’s ID). We give priority to object in this case if it’s available. Usually this should not be done as it would probably violate some other convention.

#### Typehinting optional arguments

If argument is optional, we provide default value for it.

If optional argument is object, we typehint it with required class and add default to `null`.

If argument is not optional, but just nullable, we can typehint it with default value `null`, but when using, we pass `null` explicitly.

Example:

```php
<?php

class Service
{
    public function __construct(SomeService $s, LoggerInterface $logger = null)
    {

    }
}
```

```php
<?php

class Entity
{
    /**
     * @param ValueClass|null $value
     */
    public function setValue(ValueClass $value = null)
    {

    }
}

$entity->setValue($someValue);
$entity->setValue(null);
// but we do not use $entity->setValue();
```

#### Void result

We always return something or return nothing. If method does not return anything ("returns" `void`), we do not return `null`, `false` or any other value in that case.

If method must return some value, we always specify what to return, even when returning `null`.

Wrong:

```php
<?php
function makeSomething()
{
    if (success()) {
        return true;
    }
}
function getValue(MyObject $object)
{
    if (!$object->has()) {
        return;
    }
    return $object->get();
}
/**
 * @throws PaybackUnavailableException
 */
function payback($requestId)
{
    makePayback($requestId);
    return true;
}
```

Correct:

```php
<?php
function makeSomething()
{
    if (success()) {
        return true;
    }
    return null;
}
function getValue(MyObject $object)
{
    if (!$object->has()) {
        return null;
    }
    return $object->get();
}
/**
 * @throws PaybackUnavailableException
 */
function payback($requestId)
{
    makePayback($requestId);
}
```

> **Why?** If method result is not used and you return something anyway, other programmer may assert that return value is indeed taken into account. For example, return `false` on failure, even if this failure is not handled anyhow by the base functionality.


### Typehinting

We always typehint narrowest possible interface which we use inside the function or class.

> **Why?** This allows us to refactor easier. We can just provide another class, which implements same interface. Also we can find real usages quicker if we want to change the interface itself (for example `RouterInterface` vs `UrlGeneratorInterface` when we change declaration of `RouterInterface::matches`).


#### Dependencies with several interfaces

If we have dependency on service with several responsibilities (read which implements several interfaces), we should inject it twice. For example:

```php
class ResultNormalizer implements NormalizerInterface, DenormalizerInterface
{
    // ...
}

class MyService
{
    public function __construct(
        NormalizerInterface $normalizer,
        DenormalizerInterface $denormalizer
    ) {
        // ...
    }
    // ...
}

$resultNormalizer = new ResultNormalizer();
$myService = new MyService($resultNormalizer, $resultNormalizer);
```

### Dates

We use `\DateTime` object to represent date or date and time inside system.

### Exceptions

#### Throwing

We never throw base `\Exception` class except if we don’t intend for it to be caught.

#### Catching

We never catch base `\Exception` class except where it’s thrown from vendor code.

In any case, we never catch exception if there are few throwing places possible and we only expect one of them.

Wrong:

```php
<?php

try {
    // code
    // some more code
    $this->service->someDeepMethod();
    // more code here
} catch (\Exception $exception) {
    return null; // not found
}
```

### Checking things explicitly

We use only functions or conditions that are designed for specific task we are trying to accomplish. We don’t use unrelated features, even if they give required result with less code.

We avoid side-effects even if in current situation they are practically impossible.

For example, we use `isset` versus `empty` if we only want to check if array element is defined.

For example, we use `$x !== ''` instead of `strlen($x) > 0` - length of `$x` has nothing to do with what we are trying to check here, even if it gives us needed result.

For example, we use `count($array) > 0` to check if array is not empty and not `!empty($array)`, as we do not want to check whether `$array` is `0`, `false`, `''` or even not defined at all (in which case IDE would possibly hide some warnings that could help noticing possible bugs).

> **Why?** We avoid side-effects if some related code changes or is refactored. Code is much easier to understand if we see specific checks or function calls instead of something unrelated that happens to give the needed result.


### Calling parent constructor

If we need to call parent constructor, we do it as first statement in constructor. For example:

```php
public function __construct($arg1, $arg2)
{
    parent::__construct($arg1);
    $this->setArg2($arg2);
}
```

> **Why?** Parent class can have some mandatory stuff to do before we can use it's functionality. See following example. Also, this is the only way to call parent constructor in some (probably most) of other languages (etc. JAVA)


Example of parent class, with which calling constructor later would fail:

```php
protected $params;
public function __construct($arg1)
{
    $this->params = [$arg1];
}
public function setArg2($arg2)
{
    $this->params[] = $arg2;
}
```

### Traits

The only valid case for traits is in unit test classes. We do not use traits in our base code.

> **Why?** We use (classical) OOP to refactor functionality to avoid duplicated code.


### Arrays

We always use `[1, 2]` instead of `array(1, 2)` where PHP code-level allows it.

## Default property values

If we need to define some default value for class property, we do this in constructor, not in property declaration.

> **Why?** Some default values cannot be set when declaring properties, so this way everything is in one place. It especially makes sense when entity has lots of properties.


```php
class MyClass
{
    const TYPE_TWO = 'two';
 
    /**
     * @var ArrayCollection
     */
    private $one;
 
    /*
     * @var string
     */
    private $two;
 
    /*
     * @var int
     */
    private $three;
 
    /*
     * @var bool
     */
    private $four;
 
    /*
     * @var SomeObject
     */
    private $five;
 
    /*
     * @var \DateTime
     */
    private $createdAt;
 
    public function __construct()
    {
        $this->one = new ArrayCollection();
        $this->two = self::TYPE_TWO;
        $this->three = 3;
        $this->four = false;
        $this->createdAt = new \DateTime();
    }
}
```

## Comments

### PhpDoc on methods

We put PhpDoc comment on all methods with exception of constructors (can be skipped in some cases).

We put PhpDoc comment on constructors when IDE (for example PhpStorm) cannot guess classes of attributes, return type etc. It’s optional otherwise. For example, if we inject some scalar type, we must put PhpDoc comment.

### PhpDoc contents

If we use phpdoc comment, it must contain all information about parameters, return type and exceptions that the method throws. If method does not return anything, we skip `@return` comment.

> scalar types are not autocompleted by IDE, but must be provided by this requirement. Example: `@param string $param1`


### PhpDoc on properties

We use PhpDoc on properties that are not injected via constructor.

We do *not* put PhpDoc on services, that are type-casted and injected via constructor, as they are automatically recognised by IDE and desynchronization between typecast and PhpDoc can cause warnings to be silenced.

We may add PhpDoc on properties that are injected via constructor and are scalar, but this is not necessary as IDE gets the type from constructor's PhpDoc.

### Fluid interface

If method returns `$this`, we use `@return $this` that IDE could guess correct type if we use this method for objects of sub-classes.

### Multiple return types

If we return or take as an argument value that has few types or can be one of few types, we provide all of them separated by `|`.

```php
<?php
/**
 * Description of the method - optional
 *
 * @param string $param1 description of param1. Description is optional, type is required (string)
 *
 * @return SomeObject[]|Collection|null Optional description of return type
 */
public function getSomething($param1)
{
    // ...
    return $collection;
}
```

> **Why?** This way we 1) know if `null` value can be returned or passed as an argument 2) can use methods with autocompletion from both of specified types. This is very convenient for collections - we can use `$collection->count()` and `foreach ($collection as $a) {$a->someMethod();}`


### Deprecations

If method is deprecated, we mark this in phpdoc as `@deprecated` with description or additional `@see` with new method to use. For example:

```php
@deprecated use DI to create and inject this service
```

```php
@deprecated
@see \Evp\Component\SomeComponent::someMethod
```

### Additional comments

We do not add file comments.

> **Why?** We use class comments - they are used if PhpDoc is autogenerated from PHP code, also can be used by IDE.


We do not use `@package`, `@namespace`, `@date` or `@author` annotations anywhere.

> **Why?** Namespace is provided by PHP keyword. Both date and author can be seen via annotations and becomes stale really quickly. After refactoring, the only name in the file can be totally irrelevant.


We do not use `@inheritdoc`.

> **Why?** It does not provide information about the method - what parameter does it take and what the return type is.


### Comment styles

We use multi-line `/** */` comments for method, property and class annotations.

We use single-line `/** @var Class $object */` annotation for local variables.

We can use `//` single line comments in the code.

We do not use `/* */` or `#` comments at all.

> **Why no multi-line comment?** We try to keep functions small and comment them in PhpDoc. If some things are to be explained in function body, single line comments should be enough. If we want to comment-out something, we just delete it (and use VCS to revert if needed) or disable functionality in configuration.


## IDE Warnings

We always avoid IDE warnings if possible (not possible is only when there is an IDE bug or some similar case). We provide `@var` php-doc comment if IDE cannot guess the type of variable and we cannot fix method declaration for type to be correctly guessed (vendor code etc.)

# Main patterns

## Thin model

We use Plain Value Objects (we name them Entities) for moving information/data around functionality.

We do not put any logic into those objects - all logic is handled outside, in services.

This allows to change and configure behaviour in different context.

## Services without run-time state

In general we try to make services without run-time state. That is, once configured, it should not change it’s behaviour in run-time.

This does not apply to specific services, which have, for example, collect some events and redispatch them in the end of request etc. - in those cases where collecting the state in run-time are their primary objective.

Instead of:

```php
<?php
class BadExample
{
    protected $name;
    public function setManagerName($name)
    {
        $this->name = $name;
    }
    public function search()
    {
        // do something with $this->name
    }
}
```

we use:

```php
<?php
class GoodExample
{
    public function search($managerName)
    {
        // ...
    }
}
```

> **Why?** When services becomes context-aware unintentionally, this makes unpredicted side-effects. More bugs, which can be very hard to debug and/or test. Much more test scenarios. Much harder to refactor.

> This is similar to using globals - if we store run-time context, we become unaware of what can make influence on what.


## Composition over inheritance

We always try to use composition instead of inheritance.

If we want to make some abstract method - we inject interface into constructor with that method.

This helps to follow single responsibility principle. Also this often allows to configure object by injecting different functionality without explosion of classes. If we have 2 abstract methods and 2 possible logic for each of them, we need 4 classes with 8 methods total and duplicated code. If we inject services, we need only 4 classes with 1 method each with no duplicated code.

Also, this allows to test code much easier, as we can test each functionality separately.

## Services (objects) over classes, configuration over run-time parameters

This related to `composition over inheritance` - we try to configure each service instead of hard-coding any of parameters in the code itself.

For example, instead of:

```php
class ServiceA
{
    public function doSomething()
    {
        $a = Manager::getInstance();
        $a->doSomething('a');
    }
}
class ServiceB
{
    public function doSomething()
    {
        $a = Manager::getInstance();
        $a->doSomething('b');
    }
}
$a = new ServiceA();
$b = new ServiceB();
```

we use:

```php
class Service
{
    private $manager;
    public function __construct(Manager $manager)
    {
        $this->manager = $manager;
    }
    public function doSomething()
    {
        $this->manager->doSomething();
    }
}
$a = new Service(new Manager('a')); // we use Dependency Injection Container for creating services, this is just an example
$b = new Service(new Manager('b'));
```

This allows to reuse already tested code in different scenarios just by configuring it inside DIC.

Of course, we still need to test the configuration itself (functional/integration testing), as it gets more complicated.

### Constant usage

We do not store configuration in constants.

> **Why?** As configuration can change, it is not constant. End result: `const SOFT = 'soft'; const HARD = 'soft';`

## Small, understandable methods

We try to write code that is self-explanatory. This implies writing small methods - we should understand what the method does by looking at it's code. We also try to name methods by their meaning, which also helps understanding the code.

## Dependencies

We always take into account component or bundle dependencies.

### No unnecessary dependencies

No unnecessary dependencies between components or bundles must be created.

If there is some abstract functionality, it must not depend upon it’s implementations.

### No circular dependencies

No circular dependencies between components or bundles must be created.

## Services

### Service creation

Services can be created only by factory classes or dependency injection container. We do not create services in other services not dedicated to do that (for example, in constructor).

### Changing entity state

#### Use of managers

We make changes to entity state in managers or some similar designated services.

Manager can make the following actions:

-   check initial entity state before changing it, validate it
-   change the state
-   dispatch event
-   make some additional actions

If state is changed in a controller or some other place, duplicated code can occur when functionality is required in another place.

#### Methods for changing state

We prefer concrete methods instead of one abstract method to change state.

This allows us to see available actions, also code is clearer as `switch` statements, event maps, complex validation rules, nested \`if\`s and similar magic is avoided.

```php
<?php
class Manager
{
    // simple short methods with clear logic:
    public function accept(Request $request)
    {
        $this->assertPending($request);
        $request->setStatus(Request::STATUS_ACCEPTED);
        $this->eventDispatcher->dispatch(RequestEvents::ACCEPTED, new RequestEvent($request));
    }
    public function deny(Request $request)
    {
        $this->assertPending($request);
        $request->setStatus(Request::STATUS_DENY);
        $this->eventDispatcher->dispatch(RequestEvents::DENIED, new RequestEvent($request));
    }

    // more complicated and totally unclear from interface
    public function changeStatus(Request $request, $status)
    {
        $this->assertPending($request);
        switch ($status) {
            case Request::STATUS_ACCEPTED:
                $eventName = RequestEvents::ACCEPTED;
                break;
            case Request::STATUS_DENY:
                $eventName = RequestEvents::DENIED;
                break;
            default:
                throw new \InvalidArgumentException('Invalid status provided ' . $status);
        }
        $request->setStatus($status);
        $this->eventDispatcher->dispatch($eventName, new RequestEvent($request));
    }
}
```

To make example more complicated, we can imagine that different validation groups must be provided for validator for each of given status and some other event class should be provided if request is denied. Furthermore, denial could take one more argument - `DenialReason $reason`.

### Data types

We use objects to represent data when possible. For example, if data can be represented by both scalar value and an object (like date), we use object. When several variables belong to single item, we use an object representing them and do not pass or operate on these separately (like money object versus amount and currency as separate variables).

We always try to use single representation of that data item - we avoid having multiple classes which represent same object in the system (for example Transfer as DTO in common library and as an entity - these could both co-exist, but we try to have an Entity as soon as possible in the system).

We normalize data as soon as possible in the system (input level) and denormalize it as later as possible (output level). This means that in service level, where our business logic resides, we always operate on these objects and not their concrete representations.

#### Identifier usage

We operate with objects inside services, not their identifiers. Objects must be resolved by their identifiers in controller, normalizer or similar input layer. In business objects (entities or simple DTOs, like `Filter`) we already have other objects, taken from database. If we do not find object by identifier, we throw exception and return `404` as a response (or specific error code with `400`).

#### Date and time

We use integer timestamp to represent date with time. When creating `\DateTime`, we must not use constructor with `@` as it ignores the time zone of the server.

For example:

```php
<?php
$createdAt = new \DateTime();
$createdAt->setTimestamp($data['created_at']);
$entity->setCreatedAt($createdAt);
```

#### Money

Always amount and currency, never only amount.

### One-to-many Relation in Services

#### Structure

If there is functionality that can be implemented in different ways and should be selected at runtime, we create Interface for it and use Manager to collect all those services that implement it.

From code, we do not call those services directly, we call Manager which chooses the correct service and redirects call to it.

This way if Interface changes, we only have to make changes in the Manager. Also, if we need to do some action every time some method is called, we can add it to manager, no need to put it to all those services.

Example:

```php
class Manager
{
    protected $providers = array();
    public function addProvider(ProviderInterface $provider, $providerKey)
    {
        $this->providers[$providerKey] = $provider;
        // alternative way: (only one argument in this case)
        // $this->providers[$provider->getKey()] = $provider;
    }

    public function doSomething(Data $data)
    {
        // todo: check if provider with such key exists
        return $this->providers[$data->getProviderKey()]->doSomething($data);
    }
}
```

#### Tags

We use dependency injection container tags to add all those services to the manager.

### Event dispatcher

We use events to notify about some result or to optionally change behaviour before making some action.

We do not use events to actually make some action happen, which is mandatory for the place which dispatches the event.

We should not make assumptions about listeners of some event. If we require a result from event listeners, this indicates that we should refactor functionality and use interfaces with tags or some similar solution.

# REST in PHP

## REST controllers

For REST controllers, we use [`PayseraRestBundle`](https://github.com/paysera/lib-rest-bundle) and normalizers.

Controller methods return entities or scalar variables.

Each method is configured with normalizer and denormalizer if needed.

We do not use request object from the controller. We define normalizers to give only the needed information to the controller. We can use plain normalizer or plain item normalizer if needed.

## Result providers

We use result provider to give `Result` entities from REST controller.

## Extending model

In server side we do not use subclasses and class-maps - we use services that give or process needed information.

On the client side, on the other hand, we have full model (via subclasses or just put into one class).

On the server side:

```php
<?php
class MainEntity
{
    protected $id;
    protected $type;
}

class SubEntity
{
    protected $id;
    protected $mainEntity;
    protected $subValue;
}

class AnotherSubEntity
{
    protected $id;
    protected $mainEntity;
    protected $anotherSubValue;
}
```

On the client side:

```php
<?php
class Entity
{
    protected $id;
    protected $type;
    protected $subValue;
    protected $anotherSubValue;
}
```

OR

```php
<?php
class Entity
{
    protected $id;
}
class SubEntity extends Entity
{
    protected $subValue;
}
class AnotherSubEntity extends Entity
{
    protected $anotherSubValue;
}
```

## Permissions

We always check permissions in REST controllers (access to resource) and/or listeners (access to API).

We check permissions using `SecurityContext` and writing custom voters, which implement `VoterInterface`.

## Pagination

We prefer cursor-based pagination.

# Smartweb related conventions

## New features

All new features are implemented as new Symfony bundles.

## External dependencies

### No hardcoded external dependencies

When creating bundle, it must have clear dependencies. Bundle can depend only on components (Evp/Component namespace), vendors or other bundles (except SymfonyIntegrationBundle).

SymfonyIntegrationBundle, on the other hand, can depend on any smartweb functionality.

In other words, Edit → Find must return no results when searching in new bundle for `smartweb`, `Evp_`, `m_pay` etc.

In other words, we must be able to get the bundle and move it to some other project/repository.

### Interfaces

If bundle need information about something available only in smartweb, Bundle defines an interface without any implementation and other services depend on this interface.

This interface can be defined in some other bundle that this bundle depends on (for example, UserBundle).

### Configuration

We provide semantical configuration for this bundle via Configuration and Extension classes.

In the configuration we take service ID, which implements our needed interface.

We add alias in Extension class to provided service ID.

```php
evp_questionnaire:
    user_provider: evp_smartweb_integration.user_provider
```

```php
<?php
$container->setAlias('evp_questionnaire.user_provider', $config['user_provider']);
```

```php
<service id="evp_questionnaire.service" class="Service">
    <argument type="service" id="evp_questionnaire.user_provider"/>
</service>
```

```php
<?php
class Service
{
    public function __construct(UserProviderInterface $provider) {}
}
```

```php
<?php
namespace Evp\Bundle\SmartwebIntegrationBundle;
class UserProvider implements \Evp\Bundle\QuestionnaireBundle\UserProviderInterface
{
// ...
}
```

## Database migrations

We do not use update scripts for database migrations. We use doctrine migrations for that.

## Bank integration

### Bank naming

Bank keys should begin with country code (when available), followed by underscore and bank name, eg.: "ee\_krediidi" or just "webmoney".

Bank keys in app-mokejimai and in app-evpbank should be the same.

### Accounting template naming

Accounting template names are generated during payment with AccountingTemplateNameResolver::getAddPaymentBillTemplateName method. Use this method to verify and/or check if supplied accounting template name is correct, generated accounting template name should be available in database - "m\_bill\_templates" table, "TemplateTitle" and "Title" columns. A good practise is to append new data to AccountingTemplateNameResolverTest::nameResolutionDataProvider and use new test case to verify that new template name exists in database.

# Symfony related conventions

This page describes conventions related directly with Symfony framework and related components (Doctrine, Twig).

It also includes some closely related conventions that can be used independently (for example in libraries).
    
## Configuration


### Configuration inside bundles

We use xml configuration inside bundles.

> **Why not yaml?** We use xml, because it is explicit and has schema definition - many mistakes can be avoided. yaml has many different variations and does not include validation nor autocomplete.


> **Why not annotations?** We don’t use annotations, as we want to leave configuration and logic separate. Moreover, class and object/service are not the same, this makes it difficult to configure few services for one class.

### Configuration in app directory

We use yaml configuration inside app directory for semantical configuration files.

> **Why?** Configuration is semantical here and thus not very complex. Most of examples use only this format, it’s default for Symfony standard.


We still use xml for service definitions inside app directory.

### Parameters dist files

#### File naming

We use `{origName}.dist.{origExt}` name for distribution versions of parameters.

> **Why?** Original extension is left the same so we can use all IDE features. It is configurable (or not important) so we **can** do that.


#### Parameter naming

We do not put bundle prefix to parameters that we only use outside the bundle, for example to configure `config.yml` file.

Prefixed parameters with a bundle name are used inside the bundle, use them as they would be private to only that bundle.

**Incorrect**:

`config.yml`:

```yaml
evp_cool_feature:
    parameter: %evp_cool_feature.parameter%
    option: %evp_cool_feature.option%
```

`parameters.dist.yml`:

```yaml
evp_cool_feature.parameter: A
evp_cool_feature.option: B
```

**Correct**:

`config.yml`:

```yaml
evp_cool_feature:
    parameter: %cool_feature_parameter%
    option: %cool_feature_option%
```

`parameters.dist.yml`:

```yaml
cool_feature_parameter: A
cool_feature_option: B
```
We can use some other naming, as long as it has no bundle prefix.

**Why?**

Because when using bundle prefix, we make assumptions about their usage inside the bundle itself. It would brake or have unintended effects in a case like this:

```php
// ...
$definition->setArguments(array($config['parameter'], $config['option']));
$container->setParameter('evp_cool_feature.parameter', 'ABC');	// here we have a problem - our parameter from parameters.yml unintentionally overwrites this parameter in the bundle
// ...
```

In other words, we have prefixes to avoid parameter clashing, by using prefix in `config.yml` (or other bundles - that's the same), we can break things.

#### Contents

We use parameters for development environment.

> **Why?** We cannot use production as this would be security issue. All developers can add default values and change them only if needed.


We use value `pass` for default passwords in development environment.

We use default domains in parameters - if everywhere configured the same, no changes in parameters would be needed.

We remove `app-` prefix from repository name and add `.dev.docker` as top-level domain. For example `app-wallet-api` ⇒ `http://wallet-api.dev.docker/`

### Files

#### Imports

When configuring services, we split definitions into separate files and import to main `services.xml` or `routing.xml` files. For example:

```txt
config/services/controllers.xml
config/services/forms.xml
config/services/repositories.xml
config/services/normalizers.xml
```

We put routing prefix into `<import/>` directive.

#### Directories

We put files to subdirectory - `services/`, `routing/` etc.

### Naming

Service ID always begins with bundle identifier, like `evp_contextual_information`, followed by dot.

If type of service is repository, controller, listener or normalizer, these are following parts:

-   type of service, like `manager`, `repository`, `controller`
-   name of the service, excluding the type.

If service is a manager, registry or some other unique service for that bundle, we use it’s identifier directly without the type.

Valid service names: `evp_page.page_manager`, `evp_page.repository.page`

### Services

#### Class names

We explicitly state class names for our services, we do not put them in separate parameters.

Instead of this:

```xml
<parameter key="bundle.service.class">Namespace\ClassName</parameter>

<service id="..." class="%bundle.service.class%"/>
```

we use this:

```xml
<service id="..." class="Namespace\ClassName"/>
```

> **Why?** This makes unnecessary difficulty when understanding available services and their structure. If we need to overwrite service, we overwrite it by it's ID, so we could change not only the class name, but also constructor arguments or make additional method calls. If functionality is to be changed by circumstances, we should make ability to change this service using bundle semantic configuration.


#### Factory services

We use new syntax for defining factory services:

```xml
<service id="newsletter_manager" class="NewsletterManager">
    <factory class="NewsletterManagerFactory" method="createNewsletterManager"/>
</service>
```

#### ID as FQCN (class name)

We do not use service IDs as class names by default.

> **Why?** As this allows easier configuration in some cases and avoids naming concerns for our services, it could be hard to refactor code when another service for the same class is added to our project. This would make one of the services "default" one, which would make debugging harder and understanding of internal working more difficult.

> For example, if we have `BalanceProvider` and we would add another service for providing balance with debts included (using same class with different configuration), this would require to go over every usage of the first service and changing it's ID.

We can use class name as service ID (or alias it) when we are providing open-source bundle. This allows others to use autowiring features if needed, even if we don't use them. We should only use this for public services - ones that are to be used outside of our bundle.

#### Autoconfiguration and autowiring

We don't use autowiring and autoconfiguration features of Dependency Injection component.

> **Why?** First of all, we don't use FQCN as service IDs, so this would not work. Secondly, it might be quicker at the beginning to use it, but to understand the code, we would need to guess where the service really comes from. If we would need to refactor something, it might require to write DI configuration for already (automatically) defined services, too.

### Routing

#### Route prefix

We always use bundle name as route prefix, just like in services. We leave out vendor prefix.

Exception: we drop out `-common` suffix, if one exists. Routes should not clash with corresponding bundle without `-common`.

#### Route naming

We try to use names to identify the action taken by route, not methods to take it.

For example, `create_page` instead of `page_post`.

### Production configuration

We do not put specific configuration in bundles - we provide semantic configuration for those and define them inside `app/config.yml`.

We do not put secret parts of production configuration anywhere in repository - no private key files, common secret strings, passwords etc. They must be ignored by git and provided in `parameters.yml`, shared directory or in some other way.

### Always full service IDs

We never dynamically build parameter names or service IDs. We use tags if we need abtraction. We define map or state all things explicitly if needed.

Do not do this:

```php
$container->get('my_prefix.' . $type . '.provider');
```

> **Why?** Because this is magic, not some nice pattern. It's hard to find all possible services which are registered (so hard to refactor - many potential pitfalls), we do not control if service is even registered in container, we cannot add some other configuration, we do not test if service implements some specific interface, we cannot get all possible types available or all possible services.


Instead do this:

```php
public function addProvider(SomeInterface $provider, $type)
{
    $this->providers[$type] = $provider;
}

public function getProvider($type)
{
    // todo: throw if missing or return null
    return $this->providers[$type];
}

// ...

public function build(ContainerBuilder $container)
{
    $container->addCompilerPass(new AddTaggedCompilerPass(
        'my_prefix.my_registry',
        'my_prefix.tag_name',
        'addProvider',
        ['type']
    ));
}

// ...

$someRegistry->getProvider($type);
```

And tag services:

```xml
<service class="..." id="...">
    <tag name="my_prefix.tag_name" type="my_awesome_provider"/>
    <!-- arguments etc. -->
</service>
```

## Repositories

### Injection

We inject repository objects inside the services via constructor.

Thus, we do not use `EntityManager::getRepository` in controllers or services.

### Configuration

We configure repository classes with `lazy=true`.

### Finding by ID

For finding entity by ID we always use `find` method, not `findOneById` etc.

Exception: If we want to preload some related entities.

Another exception: If we override `findOneById` to contain call to `find`. This could be used to provide PhpDoc for IDE to guess the type returned.

### Queries

We use query builder and queries inside repositories only - we don’t build queries inside controllers or other services.

### Return types

#### Basic usage

Repository methods starting with `find` returns instance(-s) of the class, that this repository is related to, or scalar values.

#### Advanced usage

If repository must use entity from other bundle, that this bundle does not depend upon, we make separate service in Repository namespace and write query in this service. For example:

-   PosBundle has entity Spot. This bundle has extension points and does not depend on WorapayBundle.

-   WorapayBundle has entity WorapaySpot. This entity has relation one-to-one with the Spot entity with no reverse relation (Spot does not know about WorapaySpot).

-   We need method findSpotsByWorapayIdentifier(\$identifier). It returns Spot[], so should be in SpotRepository. But as it’s in SpotBundle, which does not depend on WorapayBundle, we make new service WorapayBundle/Repository/SpotRepository, inject base SpotRepository and build corresponding query there. In this case, we would need 2 from clauses as join would not work (no relation from Spot to WorapaySpot).

### Method naming

We use `findOneBy*` to find one object and `findBy*` to find list of objects.

We use `getQueryBuilderFor*` to get query builders (for example for pagination), `findCountBy*` etc. to get scalar results.

### Only custom methods from outside of repository

We do not use `findOneBy` or `findBy` methods from outside of repository. We create method inside repository for that use-case - in that method, we can call `$this->findBy(...)` with given parameters.

> **Why?** This allows to add more constraints in one single place, for example after adding `deleted` column we can update all queries inside repository to filter those records out by default. Also we can see all possible queries in one place - this allows to easier review which indexes should be defined and which are unused.


### Repositories from other bundles

We try to inject repositories only into services that are in the same bundle the repository is.

If we need to get information from another bundle, we try to use services (which, in that bundle, can inject that repository).

> **Why?** This creates proxy service for getting needed information. Even if at first it would only pass method call to repository itself, if something changes we can refactor it easier - we can inject other services, handle filtering of arguments or results etc. This helps a lot if method in repository is used from many many places inside whole project and we need to change some logic with other injected service.


### Prefetching and filtering at the same time

We do not select related entities if we filter by them. For example, do **not** do this:

```php
return $this->createQueryBuilder('c')
    ->select('c, a')
    ->leftJoin('c.accounts', 'a')
    ->where('a.type = :accountType')
    ->setParameters(array(
        'accountType' => 'technical',
    ))
    ->getQuery()
    ->getResult()
;
```

Because if we iterate over client entities that were returned, and call `getAccounts()` on them, we will **not** get all accounts for that client. We will get only those that matched the query (technical accounts in this case). Furthermore, it breaks code such as this later in the process:

```php
$client = $clientRepository->find(123);
$client->getAccounts();  // this will also return *only* technical accounts, as the client is cached in doctrine's identity map
```

What are the options?

1) change `->select('c, a')` to `->select('c')`, so we do not pre-fetch accounts;
2) pre-fetch them with a separate join:

```php
return $this->createQueryBuilder('c')
    ->select('c, a')
    ->leftJoin('c.accounts', 'a')
    ->leftJoin('c.accounts', 'at')
    ->where('at.type = :accountType')
    ->setParameters(array(
        'accountType' => 'technical',
    ))
    ->getQuery()
    ->getResult()
;
```

Keep in mind that pre-fetching is OK and recommended in most of the cases where we will use those relations. It's just not right if we also filter by that relation.


### Searching / paginating by date

When searching by date period, we take beginning inclusively and ending exclusively.

> **Why?** We do not need to add or remove one second from time periods, code gets much more readable. If both would be inclusive or exclusive, when iterating through periods, some results would be provided in both periods and some results on no periods.


On the other hand, in the user interface, if user wants to set dates inclusively, frontend logic has to map between UI and backend/API convention.


## Entities


### Setters

All setters in entity returns `$this` for fluent interface.

### Setters vs adders

Setters always resets the previous value. Adders leaves previous value and only adds provided to already existing collection.

### Relations in setters

In many-to-one relation, `many` side (the owning one) always fixes relations. This is done in the `add*` method, adding reverse relation to `$this`.

In this case `set*` contains code to reset collection and `add*` every item in provided collection, so that 1) every item in collection would have relation fixed 2) collection object would be the same as before `set*`

```php
<?php
class Tree
{
    private $leaves;

    public function __construct()
    {
        $this->leaves = new ArrayCollection();
    }

    /**
     * @param Leaf[] $leaves we do not typecast to array or Collection as this can be any iterable
     */
    public function setLeaves($leaves)
    {
        $this->leaves->clear();
        foreach ($leaves as $leaf) {
            $this->addLeaf($leaf);
        }
        return $this;
    }

    public function addLeaf(Leaf $leaf)
    {
        $this->leaves[] = $leaf;
        $leaf->setTree($this);
        return $this;
    }
}
```

### States, types etc.

We save states, types and other choice-type information as strings in database.

We provide constants with possible property values in Entity.

Constant name starts with property name, followed by the value itself.

```php
<?php
class Entity
{
    const TYPE_SIMPLE = 'simple';
    const TYPE_COMPLICATED = 'complicated';

    private $type = self::TYPE_SIMPLE;
}
```

### ID

We do not provide `setId` method as we must not use it - we either persist new entity or take it from repository and update it.

### Creation date

We set `createdAt` property in constructor if we need creation date. We can also set `createdAt` in manager if entity is always created in that one place (it should be).

> **Why not doctrine extension?** This works faster and more reliable. Furthermore, even new not-yet-persisted entities has `createdAt` value, so we can assert that it’s always available. Also this makes unit testing easier as we do not need to mock extensions.


### Updated at

We do not put `updatedAt` to entities unless needed. Valid use-case is if we need to see when **any** of the fields was changed, for example if synchronizing relational database via cron job into some other storage etc.

In any other use-case `updatedAt` does not give needed information. Either it is not used at all, either it is used where it shouldn't. For example, if we need to see when some state has changed, we need to put extra column or even one-to-many relation to be sure that the date saved really represents that state change, not some other change in any other of entity fields. Even if there is one (besides `id` and `updatedAt`) field in entity, extra one can be added later and this would break the functionality (or naming).

## Doctrine

### Flush

#### Number of flushes

We always avoid more than one `flush` in one process.

#### Where to flush

We use `flush` only in controllers, commands and workers. These are the top input layer to the system, and is used usually only once per request life-cycle. Services (and especially listeners) can be used in many different ways, so we cannot make assumption that a few of them will not get called in the same request.

#### Multiple flushes

Services that do flush the database or use other services that flush the database (in any depth) should not live in namespace Service/, but in separate namespaces such as Processor/.

```php
<?php

// namespace Evp\Bundle\AccountingBundle\Service; // Incorrect
namespace Evp\Bundle\AccountingBundle\Processor; // Correct

use Doctrine\ORM\EntityManagerInterface;

class FooProcessor
{
    protected $entityManager;
    protected $remoteServiceClient;

    public function __construct(EntityManagerInterface $entityManager, RemoteServiceClient $remoteServiceClient)
    {
        $this->entityManager = $entityManager;
    }

    public function process()
    {
        $foo = new Foo('name');
        $createdFoo = $this->remoteServiceClient->createFoo($foo);
        $foo->setRemoteId($createdFoo['id']);
        $this->entityManager->flush();

        $this->remoteServiceClient->confirmFoo($foo);
        $foo->setStatus('done');
        $this->entityManager->flush();
    }
}
```

### Database types and definitions

When we use Doctrine `string` type, we use length of `255` or bigger if needed.

> **Why?** There is no real advantage in using less than `255`. It does not add performance, does not save space and can only cause difficulties if some smaller defined max length is reached.


### Database naming

#### Underscored

We use underscored names for both table names and column names.

#### Plural

We use plural nouns as table names.

> **Why?** This makes SQL statements more natural language - we select some items from a collection.


#### Prefix

We add prefix to all databases, consisting of bundle name without vendor. `EvpPageBundle:PageRoute` -> `page_page_routes`.

#### SQL keywords

We avoid SQL keywords in table and column names, but leave them as is in PHP-side.

```php
<?php
class Entity
{
    private $key;
}
```

```xml
<field name="key" column="entity_key" type="string"/>
```

### Class-map

We avoid Doctrine class-map and extending entities.

> **Why?** Extending comes with very bad performance (many relations are queried automatically even if they are never used afterwards), and many intanceof checks in the code. Also, this is not extendable, as base bundle must know about all others extending it’s entity. Moreover, we cannot change instance of Entity (change it's subclass) once it's created, which also makes things unnecessarily difficult.


### Extendability pattern instead of class-map

We use such a pattern:

-   Base entity has only the information, which is common to all entities. Thus this information is used inside base bundle in some services.

-   Base entity has field type, providerKey or similar. It defines, which bundle/manager/provider created this entity and knows more information.

-   There is Manager or similar service in base bundle, which provides methods to manage this entity (and it’s subclasses). This service has method addProvider or similar, accepting ProviderInterface (or similar)

-   There is a compiler pass, which adds Providers (etc., implementing interface from base bundle) to Manager via add\* method. We can always use the same class from lib-dependency-injection repository, no need to create separate for each case, only if there is some custom logic.

-   Providers live in their bundles, optionally having additional information in some separate entities, which possibly have relation to the base entity.

Example:

-   PageBundle has entity Block, which has id and providerKey.

-   BlockManager has method renderBlock(Block \$block) and addProvider(ProviderInterface \$provider). It lives in PageBundle. Methods from ProviderInterface are used only in this manager, (bad: manager→getProvider(block)→render(block), good: manager→render(block)). This allows to refactor some of the code more easily if needed.

-   There is compiler pass registered, which adds all tagged providers to manager via addProvider method.

-   ProviderInterface lives in PageBundle and has 2 methods: getKey() and renderBlock(Block \$block)

-   SmartwebIntegrationBundle has Entity SmartwebPageBlock, which has fields block and pageId.

-   SmartwebBlockProvider implements ProviderInterface and lives in SmartwebIntegrationBundle. renderBlock method searches for SmartwebPageBlock related to the given block and renders page via pageId.

Such structure let’s extend services without reverse dependencies. As base bundle has no dependencies, it can be moved to separate bundles. Also the code is not changed in the base bundle - we can only test the new providers etc., no need to test if we didn’t brake anything in the base bundle.

### Saving Money instances

#### No getters for amount and currency

We only provide getter and setter for Money object, not for it's fields

#### No handling with events

We convert to and from Money object only in getters and setters, we do not use eventing system for that. It could lead to unsaved fields, as doctrine does not watch object property, it only watches amount and currency properties, so in some cases there can be no event at all. Also this would make understanding structure, debugging and testing harder.

#### We do not store Money objects themselves

As money object is 1) immutable 2) never compared by reference, we do not store the object itself in Entity. We store only it's internal fields and recreate it when needed. This makes code simpler.

```php
class Entity
{
    private $priceAmount;
    private $priceCurrency;

    /**
     * @var Money|null $price
     */
    public function setPrice(Money $price = null)
    {
        if ($price === null) {
            $this->priceAmount = null;
            $this->priceCurrency = null;
        } else {
            $this->priceAmount = $price->getAmount();
            $this->priceCurrency = $price->getCurrency();
        }
    }

    public function getPrice()
    {
        return $this->priceAmount !== null && $this->priceCurrency !== null ? new Money($this->priceAmount, $this->priceCurrency) : null;
    }
}
```

#### We store amount as decimal

Inside doctrine configuration:

```xml
<field name="amount" type="decimal" precision="16" scale="6" />
```

This allows to sum and perform other number-based operations inside the database. Also, it auto-validates amount to be numeric (just in case we didn't handle it), and it takes less space, so also performs better if indexing.

We store currency as a string.

### Doctrine migrations

For migrating database structure we always use doctrine migrations. There should be no changes inside generated migration if we run `doctrine:migrations:diff` after `doctrine:migrations:migrate`. We do not put custom `ALTER` or `CREATE` statements inside migration scripts ourselves - we modify `xml` file and let doctrine to generate migration file for us.

If needed, we can add additional `UPDATE` or/and `INSERT` statements for data migration corresponding to our new database structure.

For new projects, we do not put `CREATE DATABASE` into migration scripts, but all tables should be created and then altered by doctrine migrations.

### ID column strategy

We use `IDENTITY` strategy for ID generation. This makes different structure in PostgreSQL - it works in same way as in MySQL. If we use `AUTO`, sequence is used and ID is set as soon as we persist the entity. As this behavior is not reproducible in MySQL database, we use `IDENTITY` to be compatible with more databases. This also affects functional tests as sqlite does not have sequences, too.

Example configuration:

```xml
<id name="id" type="integer">
    <generator strategy="IDENTITY"/>
</id>
```

### Variable types for query parameters

Always make sure that parameters are of correct type when building query.

This is especially important when we pass an integer and column type is `VARCHAR`. In this case database tries to cast column value of every row into an integer and compare to passed value - no indexes get used in such cases. For example:

```sql
SELECT * FROM gateway.ext_log_entries e0_
WHERE e0_.object_id = 21590
      AND e0_.object_class = 'Evp\\Bundle\\ClientBundle\\Entity\\ClientNatural'
ORDER BY e0_.version DESC;
```

Correct usage:

```sql
SELECT * FROM gateway.ext_log_entries e0_
WHERE e0_.object_id = '21590'
      AND e0_.object_class = 'Evp\\Bundle\\ClientBundle\\Entity\\ClientNatural'
ORDER BY e0_.version DESC;
```

For PHP7, we might require that correct type (`string` in this case) would be passed as an argument. For lower PHP versions, we could cast an argument when passing to query.

## Controllers

### As services

We use controllers as a services. That is, every controller must be defined as a service.

If controller methods returns Response objects (that is this is not REST controller), controller can extend base Symfony Controller. In this case we add a call to setContainer in controller definition (services/controllers.xml). We do not use container in the controller’s code - we inject needed services via constructor.

### Static templates

If controller has no logic, we use `FrameworkBundle:Template:template` controller in routing and set `template` argument to twig template name to render.

### Parameters passed to view

We pass original representation of variables to view, if view can itself transform the variables.

```php
<?php
class Controller
{
    public function action()
    {
        // ...
        return $this->render($template, array(
            'options' => array('a' => $a, 'b' => $b), // we do not use json_encode() here
        ));
    }
}
```

```php
<div data-options="{{ options|json_encode }}"></div>
```

> **Why?** Controller just provides variables, it does not know (or shouldn’t know) how they will be used in template. Furthermore, template sometimes needs both encoded and original versions, so controller would duplicate some code in such case.


### Small controllers

We group actions into small controllers - we do not put more than 5-8 actions into one controller.

We try to make controller for each of resources, not just one for whole bundle.

It’s perfectly ok to have controllers containing only one action (such controllers can be easily configured and reused).

> **Why?** As we use constructor injection, big controllers have many dependencies, which are rarely used in such cases. This is bad for performance.


### Class naming

Controller class name must always end with `Controller`.

## Events

### Available events

We put all available events in `final` class, containing only constants with available events.

### Event naming

We name events in past-tense verbs, prefixed by resource for which some action happened.

> **Why?** Events should be used only after something happened (exception: before something happening). Present tense verb would indicate that event should make something, which would be a misuse of event system.


We do not start constants with `ON_`.

> **Why?** `on*` indicates action performed when event is dispatched, not the event itself.


We can start constants with `BEFORE_` or `AFTER_` if it indicated event that is dispatched before or after some action.

We give constant the same value as the constant name, prefixed with bundle name.

> **Why?** For find-replace to find both constant and it’s value usage (in tags). Bundle prefix is needed to avoid collisions.


```php
<?php
final class PageEvents
{
    const PAGE_CREATED = 'evp_page.page_created';
}
```

### Listener naming

We use `on*`, `before*` or `after*` method names for event listeners.

We can use the same method for several events if logic is the same.

### Event class naming

We name event classes by their data, not some concrete use case.

Wrong:

```php
<?php
class PageCreatedEvent extends Event
{
    private $page;
}
```

Correct:

```php
<?php
class PageEvent extends Event
{
    private $page;
}
```

Also correct:

```php
<?php
class PageChangedEvent extends Event
{
    private $page;
    private $previousPage;
}
```

## Directory structure

### Root bundle directory

We put following classes to root bundle directory:

-   Bundle definition
-   Final Events classes for events, raised by this bundle

### Basic directories for code

-   `Command` - for commands
-   `Controller` - for controllers
-   `Entity` - for entities
-   `Event` - for events
-   `Listener` - for listeners
-   `Normalizer` - for normalizers
-   `Repository` - for repositories
-   `Service` - for main services of this bundle
-   `Worker` - for workers (handles jobs from RabbitMQ)

### Integration with other bundles

We use custom namespaces for integrating with other bundles or when our service has some specific type, is not main facade for that bundle.

```txt
Callback/CallbackValidator.php
Callback/CallbackProvider.php
DynamicAsset/AssetGroupProvider.php
PageBlock/MenuProvider.php
```

### Special directories

-   `Resources` - for resources
-   `Tests` - for tests. Test for `Vendor/Bundle/SomeBundle/Namespace/Service` goes to `Vendor/Bundle/SomeBundle/Tests/Namespace/ServiceTest`

### Example

```txt
Evp/Bundle/PageBundle
    Controller
        PageController.php
        PageApiController.php
    DynamicAsset
        AssetGroupProvider.php
    Entity
        Page.php
        Block.php
    Event
        PageEvent.php
    Listener
        LocaleListener.php
        RequestSubscriber.php
    Menu
        PageMenuProvider.php
    Normalizer
        BlockNormalizer.php
        PageNormalizer.php
    Resources
        ...
    Service
        PageManager.php
        BlockProviderInterface.php
        BlockManager.php
    Tests
        Menu
            PageMenuProviderTest.php
        Service
            PageManagerTest.php
    PageEvents.php
    EvpPageBundle.php
```

## Twig

### Calling methods

We use either `.something` or `.getSomething()`, we do not use `.getSomething`.

We prefer `.something` for getters without arguments.

We prefer using static templates with JS framework and REST API over twig.

## Commands

### Naming

If command name consists of several words, we use dashes to separate them. For example, `some-namespace:do-something`

### Dependencies

If project uses Sf 2.4 and above, we register commands as services with dependencies injected into them.

For Sf 2.3 we take dependencies from service container.

## Symfony version and new projects

### Version and structure

We use only stable releases for our projects.

We use only supported (at least for security fixes) versions of Symfony.

We prefer LTS version of Symfony framework as it does not require much maintenance on updating vendors.

We can use, if applicable, newer stable version of Symfony, but we must update it periodically (more often than LTS) so that is would still be supported (even if we don't actively maintain that project, so choose carefully).

We prefer to use only 2 different major versions of Symfony for our projects. For example:

* let's say that currently Symfony 3.4 is newest LTS version of Symfony. Symfony 4.1 is stable, but not LTS version. Symfony 2.8 is older LTS version and is still supported;
* some projects are still ran on Symfony 2.8 version;
* we are migrating these projects to Symfony 3.4, some projects are already using it;
* we avoid using Symfony 4.1 for new projects (or avoid updating current projects to this version) as this would make 3 different major versions in use;
* when all the projects are updated to Symfony 3.4, we could update our project to use Symfony 4.1 (even non-LTS) or use this for new projects.

> **Why not using 3 different major versions?** This would make quite hard to maintain several Symfony versions in our libraries and bundles. If we would drop 2.x support (to support only 3.x and 4.x), this would make difficult to correct some bug or add improvement to those projects that still use 2.x version.


### Configuration

We always use skeleton to bootstrap new projects as this allows to easily use any (configuration) fixes or improvements that were made to each and every project during the years. It also includes all needed basic features that could be depended on by our libraries or bundles which could be installed afterwards.

Keep in mind that there are different skeletons for WEB, REST API and processing nodes.

# Handling sensitive values

Sensitive values are those that are secret and should not be known or read in plain text. For example passwords, tokens, secret generated codes etc. Any values that can affect security of the system.

## Logging

We never log content where sensitive values can be found. For example, request content for all requests, as these include call to authentication. We must either 1) do not log at all 2) make whitelist what to log 3) make blacklist when to avoid logging (this is not recommended as can be easily forgotten).

When we configure loggers, we always use normalizers that does not extract private fields from any given objects by default.

## Passing sensitive values as arguments

As we log exception stack trace, any scalar values passed as arguments can be used in stack trace. To avoid that, we put sensitive values to `SensitiveValue` object as soon as they enter our system. We pass them as this object. We get the value itself only in lowest level possible.

If we cannot pass `SensitiveValue` (for example, to vendor library), we catch `\Exception` and re-throw a new one, without providing last exception from before (we can copy the message itself, if it does not contain sensitive values).

## Storing sensitive values in entities

If we really need to store sensitive value in an entity, we make that property private. We also make that setter and getter would operate with `SensitiveValue` and not scalar type itself.

Better approach is to always use hash of sensitive values, if we just need to check if provided value matches it. If we need to resend generated code or use original value later, only in that case we save code in database unchanged.

# Composer Conventions

## Semantic versioning

We always use semantic versioning on library repositories.

> **Why?** When backward incompatible change is made in library, we have two options: 1) update all client side projects with new library version 2) every time when we update library check for previous backward incompatible changes, not related to currently added feature. As it happens, sometimes none of these 2 takes place.

## Backward incompatible changes

If we make backward incompatible change, we always provide some basic information about what was changed in `CHANGELOG.md` or `UPGRADE.md` file.

## Releases

We tag each and every commit (except instant bug-fixes or commits before separate merge commits).

1. Right before tagging, we make sure we're on master branch and always run `git tag --sort=v:refname` to see current tags.
2. Before tagging, we see available branches on repository as version can be created by branch alias, not only by tags.
3. We bump MAJOR, MINOR or PATCH version by one from latest version available (or parent commit if there are few active branches).

    3.1. If we make backward incompatible change, we bump the MAJOR version.

    3.2. If we add new feature (any new arguments, method calls, classes etc.), we bump MINOR version.

    3.3. If we do not change API of library in any way, just change the internals (usually bug-fixes), we bump PATCH version.

We use annotated tags so that time and author would be present. Example command, `1.2.3` taken as example tag:

```
git tag -a -m "" 1.2.3
```

`-m ""` sets message to the tag, but as all commits are already with messages, we can leave it empty.
 
After tagging we need to call `git push origin (tag)` - this pushes our tags to origin. We only do this after we've landed our changes.
 
After pushing, we need to manually update library in toran: go to toran, select "Private repositories", find yours and press "Update".

## Reviewing tagging policy

In differential revision we always comment on intended tagging policy - `MAJOR`, `MINOR` or `PATCH`. As this can change (wrong initial policy denied by review or changed after additional commits in review), we provide this information in a summary or in a comment, not in commit message (title of revision).

Reviewer must reject a diff, if `MAJOR` bump is needed and no information is provided about backward incompatible changes.

## Initial library development

Until library is stable, versions can change quite often. In this case we still use tags and semantic versioning, but use `0` as MAJOR version. In this case any backward incompatible change can be made in any MINOR version bump. If API of library is quite stable and is not to change much, we use `1` as a MAJOR version initially.

## Constraints on library versions

We use as generic constraints as possible for required libraries. This avoids updating some library graph just because of some conservative constraint, and not because it is really needed.

For example, we have `lib-a@1.0.0` and `lib-b@1.0.0`, which depends on `lib-a: ^1.0`. If we roll out MAJOR release for `lib-a` (aka `lib-a@2.0.0`), at some time we need to modify constraint on `lib-b`:

1. If `lib-b` works with both `lib-a@1.0.0` and `lib-a@2.0.0` (change did not affect this library), we use `lib-a: >=1.0.0,<3.0`. This allows to use both `1.0` and `2.0` with our new version of `lib-b`.
2. If `lib-b` works only with `lib-a@2.0.0` and not with `lib-a@1.0.0`, we modify constraint as usual to `lib-a: ^2.0.0`.
3. If `lib-b` does not work with `lib-a@2.0.0`, we can either leave constraint to `^1.0` or update code and use (1) or (2) depending on compatibility with `1.0` version. Latter is recommended, as this allows to use newest possible versions (sooner or later we will need to update them).

Version in `lib-b` in any of these cases is not required to make a MAJOR bump - if API of `lib-b` is left the same, we can make PATCH update. We can even entirely drop out `lib-a` and use some alternative library, as long as API leaves the same.

This means that if `app-x` (or any other library) uses functions or classes from `lib-a`, it must require it in `composer.json`. If it just depends on `lib-b`, it does not care about `lib-a` versions or even if it is installed at all.

## Constraints on vendors

If we require vendor library, we use constraint depending on versioning strategy of that library. If it states, that it follows semantic versioning (and we believe it does), we use something like `^1.2.3` - this is added by default if we run `composer require vendor/library`. If it has some other strategy, we must correct the constraint so that we would not get unexpected backward incompatible changes. For example, Symfony components did break compatibility on minor releases (at least in 2.X ones). In that case, we must not use `^` in constraint.

Basically, we must always assume that `composer update` can be run at any time and everything should still work as expected.

Due to the same reason, we never require `dev-master`. If vendor library has no versions defined, we require specific commit:

```
"vendor/package": "dev-master#8e45af93e4dc39c22effdc4cd6e5529e25c31735"
```
 
## Making minor/patch release on previous version
 
Usually we try to have single branch in origin. This way we only add features to the latest code and upgrade our software to use the newest versions of our libraries.
 
In some cases, it might be needed to update/change code in some previous version of the library.
 
Let's say, that library has versions 1.0, 2.0, 3.0, 3.1. We need to add change to 1.0 and make 1.1 version. In that case:
 
1. Checkout tag we want to change (1.0 in example). Command: `git checkout tags/<tag_name> -b <branch_name>`
2. Allow Dangerous changes for that library in Phabricator
3. Push immediately with no changes to create new branch. Command: `git push --set-upstream <remote-name> <local-branch-name>`. Remote name should be `1.x` or `v1` in this example.
4. Make changes, arc diff...
5. Add git tag
 
This creates additional branch so we can add fixes / new features and release 1.1 without changing 3.x versions.

# Deprecated conventions

Some convetions that are used thoughout the systems, either explicitly defined in conventions, either just used from practive, are deprecated.

This page lists those conventions and reasons why we should avoid them.

## REST proxy controllers

Example:
```
Browser --(request with cookie)--> FrontendApiController --(request with header)--> BackendApiController --(SQL)--> DB
```

There was architecture decision made, that, for security reasons, we should make separate systems that are accessible from the Internet and those that can access the Internet themselves. Furthermore, APIs were splitted into backend-API (accessible only from our internal systems), public-API (accessible for users) and admin-API (accessible for administrators).

In some cases, to migrate to this architecture, there were (still are) several different bundles in the same system, which, later on, could be separated into several different systems. Thus, even if system itself can access some functionality directly, it makes HTTP request to the same system (just a different endpoint). This makes lots of issues:

- there were no clear way to guarantee that separation really works. In some cases frontend-API bundle injected backend functionality, so separation did not serve it's purpose;
- authentication is different for public-APIs (cookie/session-based) and backend-APIs (headers with Basic/MAC authentication). It takes some effort to configure them correctly in the first place, and security holes can occur due to misconfiguration and complexity;
- as backend-APIs did not know anything about user authentication (cookies etc.), frontend-APIs became trusted party. Frontend-API authenticates user and passes just her ID to the backend-API. This makes frontend-API credentials (of the system itself, for authenticating to backend-API) really critical - you can impersonate any user just with these single credentials;
- as system is the same, backend-APIs (which just authenticates these single credentials) are not separated in terms of network - they are still accessible from the network. IP restrictions are configured inside the system, but, as there are many frontend-APIs (which are accessible) and backend-APIs and something in-between, usually this also gets misconfigured;
- performance degrades, as frontend-API makes additional request to backend-API.


Instead, we make single multi-purpose backend-API, accessible directly from Internet.
For authentication, we use JWT or client credentials (configured to access only needed features, not everything for everyone).

```
Browser --(request with header)--> BackendApiController --(SQL)--> DB
```

## Splitting controllers by use-case (administration / frontend)

We should not make `AdministrationApiController`, ever.

This violates other conventions, as it's named after use-case, not functionality. In fact, in cases where it was named like that, one of the following happens:

- at least one endpoint gets used not only by administrators;
- code gets duplicated as the same endpoint is repeated in another controller;
- code is refactored to avoid this pattern (of course, this is preferred).

Additionally, it makes routing and prefixes complicated, duplicated, sometimes even conflicting.

So, we make single multi-purpose backend-API. We can split it into several controllers, but it should be done by functionality itself, not their current usage.

As with every endpoint, we configure authorization (permissions needed to perform that action). Administrators just get permission to perform that action.

If needed, partners, other systems or even user herself could also reuse these endpoints.

### URL path prefixes

Some deprecated rules:

- We use `/{_locale}/bundle-name/rest` prefix for all front-end REST routing.
- We use `/admin/{_locale}/bundle-name/rest` prefix for all administration front-end REST routing.


As we avoid splitting APIs, these are deprecated.
Now we just make single backend API with `/bundle-name/rest/v#` prefix. We pass language(s) in the `Accept-Language` header.

## `--force`

In some commands `--force` command line option is used to "really" perform something. This got misused and is almost never really needed.

As it's used in many places, now we always add this - not only in code before doing something, but when calling, also. And it teaches us that running command will not have any concequences, unless we pass some option. We must not just run some commands without knowing what they do or how they are supposed to work. If we need explanation, we just add `--help` option. By default, command should do what it's supposed to do.

Furthermore, if we always add this, it cannot be used when it would really make sense. This could be, for example, generating file, where `--force` option would always overwrite existing one without even asking.

## `markAs*`

In entities, we used to add `markAsDone` and similar methods for changing entity state. We tend to keep "long" version of this - `setStatus(Entity::STATUS_DONE)` so that we could easily find usages in the project without doing 2 iterations (first by `setStatus` and/or constant, then by `markAsX` method).

## common libraries / bundles

As we use classes in both backend applications and PHP client code, we used to move some common parts to separate library. For example:

```
AuthorizationBundle
    Entity
        Card
    Transformer
        CardToCommonCardTransformer
        CommonCardToCardTransformer
AuthorizationCommonBundle
    Entity
        Card
    Normalizer
        CardNormalizer
AuthorizationRestClientBundle
    AuthorizationClient
```

This way we can reuse `CardNormalizer` from common library. As `Card` entity is ususally different in backend and common, we can either extend from common one, or use transformers to convert back and forth.

Such dependencies have several problems:

- additional management for updating several vendor libraries when needed;
- more complicated than straight-forward mapping from array to objects;
- we do not need additional normalizer class, but we need transformer classes which usually contain very similar code;
- normalizers can sometimes differ on backend and client side. When mapping from object to array, backend should include ID and status. On the client side, this is not needed, as we do not pass these fields when creating resource.


Better practice is to completely separate client from backend without any common dependencies for models.

To do this, we write RAML specifications for our API endpoints and completely generate client code from RAML.

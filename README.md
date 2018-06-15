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
    
4. [Main patterns](https://github.com/paysera/php-style-guide/tree/master/main-patterns)

    4.1 [Thin model](#thin-model)
    
    4.2 [Services without run-time state](#services-without-run-time-state)
    
    4.3 [Composition over inheritance](#composition-over-inheritance)
    
    4.4 [Services (objects) over classes, configuration over run-time parameters](#services-(objects)-over-classes-configuration-over-run-time-parameters)
    
    4.5 [Small, understandable methods](#small-understandable-methods)
    
    4.6 [Dependencies](#dependencies)
    
    4.7 [Services](#services)

1. [REST in PHP](https://github.com/paysera/php-style-guide/tree/master/rest-in-php)
1. [Smartweb related conventions](https://github.com/paysera/php-style-guide/tree/master/smartweb-related-conventions)
1. [Symfony related conventions](https://github.com/paysera/php-style-guide/tree/master/symfony-related-conventions)
1. [Handling sensitive values](https://github.com/paysera/php-style-guide/tree/master/handling-sensitive-values)
1. [Composer conventions](https://github.com/paysera/php-style-guide/tree/master/composer-conventions)
1. [Deprecated conventions](https://github.com/paysera/php-style-guide/tree/master/deprecated-conventions)

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


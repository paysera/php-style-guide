## Table of Contents

1. [Basics](#basics)

    1.1. [PHP code-level](#php-code-level)

    1.2. [Globals](#globals)

    1.3. [Files](#files)

    1.4. [Exceptions for code style usage](#exceptions-for-code-style-usage)

2. [Code style](#code-style)

    2.1. [Commas in arrays](#commas-in-arrays)
    
    2.2. [Splitting in several lines](#splitting-in-several-lines)
    
    2.3. [Chained method calls](#chained-method-calls)
    
    2.4. [Constructors](#constructors)
    
    2.5. [Variable, class and function naming](#variable,-class-and-function-naming)
    
    2.5.1. [Full names](#full-names)
    
    2.5.2. [Class naming](#class-naming)
    
    2.5.3. [Interface naming](#interface-naming)
    
    2.5.4. [Property naming](#property-naming)
    
    2.5.5. [Method naming](#method-naming)

    2.6. [Order of methods](#order-of-methods)

    2.7. [Directories and namespaces](#directories-and-namespaces)

    2.7.1. [Singular namespaces](#singular-namespaces)

    2.7.2. [No *Interface namespaces](#no-*interface-namespaces)

    2.7.3. [Different namespaces and service names](#different-namespaces-and-service-names)

    2.8. [Comparison order](#comparison-order)

    2.9. [Namespaces and use statements](#namespaces-and-use-statements)

3. [Usage of PHP features](#usage-of-php-features)

    3.1. [Condition results](#condition-results)

    3.2. [Logical operators](#logical-operators)
    
    3.3. [Strict comparison operators](#strict-comparison-operators)
    
    3.4. [Converting to boolean](#converting-to-boolean)
    
    3.5. [Comparing to boolean](#comparing-to-boolean)
    
    3.6. [Comparing to null](#comparing-to-null)
    
    3.7. [Assignments in conditions](#assignments-in-conditions)
    
    3.8. [Unnecessary variables](#unnecessary-variables)
    
    3.9. [Reusing variables](#reusing-variables)
    
    3.10. [Unnecessary structures](#unnecessary-structures)
    
    3.11. [Static methods](#static-methods)
    
    3.12. [Converting to string](#converting-to-string)
    
    3.13. [Double quotes](#double-quotes)
    
    3.14. [Visibility](#visibility)
    
    3.14.1. [Public properties](#public-properties)
    
    3.14.2. [Protected vs private](#protected-vs-private)
    
    3.15. [Functions](#functions)
    
    3.15.1. [count](#count)
    
    3.15.2. [is_null](#is_null)
    
    3.16. [str_replace](#str_replace)
    
    3.17. [Return and argument types](#return-and-argument-types)
    
    3.17.1. [Return types](#return-types)
    
    3.17.2. [Argument types](#argument-types)
    
    3.17.3. [Passing ID](#passing-id)
    
    3.17.4. [Typehinting optional arguments](#typehinting-optional-arguments)
    
    3.17.5. [Void result](#void-result)
    
    3.18. [Typehinting](#typehinting)
    
    3.18.1. [Dependencies with several interfaces](#dependencies-with-several-interfaces)
    
    3.19. [Dates](#dates)
    
    3.20. [Exceptions](#exceptions)
    
    3.20.1. [Throwing](#throwing)
    
    3.20.2. [Catching](#catching)
    
    3.21. [Checking things explicitly](#checking-things-explicitly)
    
    3.22. [Calling parent constructor](#calling-parent-constructor)
    
    3.23. [Traits](#traits)
    
    3.24. [Arrays](#arrays)
    
    3.25. [Default property values](#default-property-values)
    
4. [Comments](#comments)

    4.1. [PhpDoc on methods](#phpdoc-on-methods)
    
    4.2. [PhpDoc contents](#phpdoc-contents)
    
    4.3. [PhpDoc on properties](#phpdoc-on-properties)
    
    4.4. [Fluid interface](#fluid-interface)
    
    4.5. [Multiple return types](#multiple-return-types)
    
    4.6. [Deprecations](#deprecations)
    
    4.7. [Additional comments](#additional-comments)
    
    4.8. [Comment styles](#comment-styles)
   
5. [IDE Warnings](#ide-warnings)


Basics
=====

## PHP code-level

For our projects and libraries we use PHP 5.5.

Exception is for our libraries for integrating with services we provide - there we use PHP 5.2 (libwebtopay, Wallet API client).

## Globals

We do not use global variables, constants and functions.

## Files

We put every class to it’s own file.

## Exceptions for code style usage

If we modify legacy code where some other conventions are used, we can use the same style as it is used these.

Code style
=====

## Commas in arrays

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

## Splitting in several lines

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

## Chained method calls

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

## Constructors

We always add `()` when constructing class, even if constructor takes no arguments:

```php
<?php
$value = new ValueClass();
```

## Variable, class and function naming

### Full names

We use full names, not abbreviations: `$entityManager` instead of `$em`, `$exception` instead of `$e`.

### Class naming

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

### Interface naming

We always add suffix `Interface` to interfaces, even if interface name would be adjective.

> **Why?** If we have base class witch implements the interface, we would have name clash. For example, `ContainerAware` and `ContainerAwareInterface`.


### Property naming

We use nouns or adjectives for property names, not verbs or questions.

```php
<?php
class Entity
{
    private $valid;       // NOT $isValid
    private $checkNeeded; // NOT $check
}
```

### Method naming

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

## Order of methods

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


## Directories and namespaces

### Singular namespaces

We use singular for namespaces: `Service`, `Bundle`, `Entity`, `Controller` etc.

Exception: if English word does not have singular form.

### No `*Interface` namespaces

We do not make directories just for interfaces, we put them together with services by related functionality (no `ServiceInterface` namespace).

### Different namespaces and service names

We use abstractions for namespaces, not service names. For example, `UserMerge` or `UserMerging`, not `UserMergeManager`.

## Comparison order

If we compare to static value (`true`, `false`, `null`, hard-coded integer or string), we put static value in the right of comparison operator.

Wrong: `null === $something`

Right: `$something === null`

## Namespaces and use statements

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

Usage of PHP features
=====

## Condition results

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

## Logical operators

We use `&&` and `||` instead of `and` and `or`.

## Strict comparison operators

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

## Converting to boolean

We use type casting operator to convert between types:

```php
<?php
return (bool)$something;
```

> This should not be used at all. If variable is not `boolean` already, check it’s available values explicitly.


## Comparing to boolean

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

## Comparing to `null`

When comparing to `null`, we always compare explicitly.

```php
<?php

function func(ValueClass $value = null)
{
    return $value !== null ? $value->getSomething() : null;
}
```

We also do not use `is_null` function for comparing.

## Assignments in conditions

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


## Unnecessary variables

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

## Reusing variables

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

## Unnecessary structures

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

## Static methods

We do use static methods only in these cases:

-   to create an entity for fluent interface, if PHP version in the project is lower thant 5.4. We use `(new Entity())->set('a')` in 5.4 or above

-   to give available values for some field of an entity, used in validation

## Converting to string

We do not use `__toString` method for main functionality, only for debugging purposes.

> **Why?** We cannot throw exceptions in this method, we cannot put it in interface, we cannot change return type or give any arguments - refactoring is impossible. Also if debugging uses \_\_toString method, we cannot change it to get any additional information.


## Double quotes

We do not use double quotes in simple strings.

We use double quotes only under these conditions:

-   Single quote is used repeatedly inside the string
-   Some special symbols are used, like `"\n"`

If we use double quotes, we do not use auto variable includes (`"Hello $name!"` or `"Hello {$object->name}!"`).

## Visibility

### Public properties

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

### Protected vs private

We prefer `private` over `protected` as it constraints the scope - it's easier to refactor, find usages, plan possible changes in code. Also IDE can warn about unused methods or properties.

We use `protected` when we intend some property or method to be overwritten if necessary.

## Functions

### `count`

We use `count` instead of `sizeof`.

### `is_null`

We use compare to `null` using `===` instead of `is_null` function. For example:

```php
if ($result === null) {
    // ...
```

## `str_replace`

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

## Return and argument types

### Return types

We always return value of one type. Optionally, we can return `null` when using any other return type, too.

For example, we can*not* return `boolean|string` or `SuccessResult|FailureResult` (if `SuccessResult` and `FailureResult` has no common class or interface; if they do, we document to return that interface instead).

We can return `SomeClass|null` or `string|null`.

### Argument types

Same applies for any argument.

### Passing ID

Exception: When making query from repository, both `int|Entity` can be taken (object or it’s ID). We give priority to object in this case if it’s available. Usually this should not be done as it would probably violate some other convention.

### Typehinting optional arguments

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

### Void result

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


## Typehinting

We always typehint narrowest possible interface which we use inside the function or class.

> **Why?** This allows us to refactor easier. We can just provide another class, which implements same interface. Also we can find real usages quicker if we want to change the interface itself (for example `RouterInterface` vs `UrlGeneratorInterface` when we change declaration of `RouterInterface::matches`).


### Dependencies with several interfaces

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

## Dates

We use `\DateTime` object to represent date or date and time inside system.

## Exceptions

### Throwing

We never throw base `\Exception` class except if we don’t intend for it to be caught.

### Catching

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

## Checking things explicitly

We use only functions or conditions that are designed for specific task we are trying to accomplish. We don’t use unrelated features, even if they give required result with less code.

We avoid side-effects even if in current situation they are practically impossible.

For example, we use `isset` versus `empty` if we only want to check if array element is defined.

For example, we use `$x !== ''` instead of `strlen($x) > 0` - length of `$x` has nothing to do with what we are trying to check here, even if it gives us needed result.

For example, we use `count($array) > 0` to check if array is not empty and not `!empty($array)`, as we do not want to check whether `$array` is `0`, `false`, `''` or even not defined at all (in which case IDE would possibly hide some warnings that could help noticing possible bugs).

> **Why?** We avoid side-effects if some related code changes or is refactored. Code is much easier to understand if we see specific checks or function calls instead of something unrelated that happens to give the needed result.


## Calling parent constructor

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

## Traits

The only valid case for traits is in unit test classes. We do not use traits in our base code.

> **Why?** We use (classical) OOP to refactor functionality to avoid duplicated code.


## Arrays

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

Comments
=====

## PhpDoc on methods

We put PhpDoc comment on all methods with exception of constructors (can be skipped in some cases).

We put PhpDoc comment on constructors when IDE (for example PhpStorm) cannot guess classes of attributes, return type etc. It’s optional otherwise. For example, if we inject some scalar type, we must put PhpDoc comment.

## PhpDoc contents

If we use phpdoc comment, it must contain all information about parameters, return type and exceptions that the method throws. If method does not return anything, we skip `@return` comment.

> scalar types are not autocompleted by IDE, but must be provided by this requirement. Example: `@param string $param1`


## PhpDoc on properties

We use PhpDoc on properties that are not injected via constructor.

We do *not* put PhpDoc on services, that are type-casted and injected via constructor, as they are automatically recognised by IDE and desynchronization between typecast and PhpDoc can cause warnings to be silenced.

We may add PhpDoc on properties that are injected via constructor and are scalar, but this is not necessary as IDE gets the type from constructor's PhpDoc.

## Fluid interface

If method returns `$this`, we use `@return $this` that IDE could guess correct type if we use this method for objects of sub-classes.

## Multiple return types

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


## Deprecations

If method is deprecated, we mark this in phpdoc as `@deprecated` with description or additional `@see` with new method to use. For example:

```php
@deprecated use DI to create and inject this service
```

```php
@deprecated
@see \Evp\Component\SomeComponent::someMethod
```

## Additional comments

We do not add file comments.

> **Why?** We use class comments - they are used if PhpDoc is autogenerated from PHP code, also can be used by IDE.


We do not use `@package`, `@namespace`, `@date` or `@author` annotations anywhere.

> **Why?** Namespace is provided by PHP keyword. Both date and author can be seen via annotations and becomes stale really quickly. After refactoring, the only name in the file can be totally irrelevant.


We do not use `@inheritdoc`.

> **Why?** It does not provide information about the method - what parameter does it take and what the return type is.


## Comment styles

We use multi-line `/** */` comments for method, property and class annotations.

We use single-line `/** @var Class $object */` annotation for local variables.

We can use `//` single line comments in the code.

We do not use `/* */` or `#` comments at all.

> **Why no multi-line comment?** We try to keep functions small and comment them in PhpDoc. If some things are to be explained in function body, single line comments should be enough. If we want to comment-out something, we just delete it (and use VCS to revert if needed) or disable functionality in configuration.


# IDE Warnings

We always avoid IDE warnings if possible (not possible is only when there is an IDE bug or some similar case). We provide `@var` php-doc comment if IDE cannot guess the type of variable and we cannot fix method declaration for type to be correctly guessed (vendor code etc.)




Paysera PHP style guide
=====

Paysera PHP style guide extends [PSR-1](https://www.php-fig.org/psr/psr-1/) and [PSR-12](https://www.php-fig.org/psr/psr-12/),
so code must also follow these standards to be compatible with this style guide.

Guide is divided into several parts:
- [PHP Basics](#php-basics). This defines additional style rules for the code, some restrictions for basic PHP usage;
- [Main patterns](#main-patterns). This describes patterns and architecture decisions that must be followed;
- [Symfony related conventions](#symfony-related-conventions). As Paysera use Symfony as the main framework,
this chapter describes rules to be used when developing for this framework;
- [Composer conventions](#composer-conventions). Describes usage rules related with composer:
releasing libraries or requiring ones.

# Table of contents

<!-- toc -->

- [Paysera PHP style guide](#paysera-php-style-guide)
- [Table of contents](#table-of-contents)
- [PHP Basics](#php-basics)
  - [Basics](#basics)
    - [PHP code-level](#php-code-level)
    - [Globals](#globals)
    - [Files](#files)
    - [Exceptions for code style usage](#exceptions-for-code-style-usage)
    - [Built-in language/framework feature usage](#built-in-languageframework-feature-usage)
  - [Code style](#code-style)
    - [Commas in arrays](#commas-in-arrays)
    - [Splitting in several lines](#splitting-in-several-lines)
    - [Chained method calls](#chained-method-calls)
    - [Constructors](#constructors)
    - [Variable, class and function naming](#variable-class-and-function-naming)
      - [Full names](#full-names)
      - [Class naming](#class-naming)
      - [Interface naming](#interface-naming)
      - [Property naming](#property-naming)
      - [Method naming](#method-naming)
    - [Order of methods](#order-of-methods)
    - [Directories and namespaces](#directories-and-namespaces)
      - [Singular namespaces](#singular-namespaces)
      - [No `*Interface` namespaces](#no-interface-namespaces)
      - [Different namespaces and service names](#different-namespaces-and-service-names)
    - [Comparison order](#comparison-order)
    - [Namespaces and use statements](#namespaces-and-use-statements)
  - [Usage of PHP features](#usage-of-php-features)
    - [Condition results](#condition-results)
    - [Logical operators](#logical-operators)
    - [Strict comparison operators](#strict-comparison-operators)
    - [Converting to boolean](#converting-to-boolean)
    - [Comparing to boolean](#comparing-to-boolean)
    - [Comparing to `null`](#comparing-to-null)
    - [Assignments in conditions](#assignments-in-conditions)
    - [Unnecessary variables](#unnecessary-variables)
    - [Reusing variables](#reusing-variables)
    - [Unnecessary structures](#unnecessary-structures)
    - [Static methods](#static-methods)
    - [Converting to string](#converting-to-string)
    - [Double quotes](#double-quotes)
    - [Visibility](#visibility)
      - [Public properties](#public-properties)
      - [Method visibility](#method-visibility)
    - [Functions](#functions)
      - [`count`](#count)
      - [`is_null`](#is_null)
    - [String manipulation](#string-manipulation)
    - [Return and argument types](#return-and-argument-types)
      - [Return types](#return-types)
      - [Argument types](#argument-types)
      - [Passing ID](#passing-id)
      - [Type-hinting optional arguments](#type-hinting-optional-arguments)
      - [Void result](#void-result)
    - [Type-hinting classes and interfaces](#type-hinting-classes-and-interfaces)
      - [Dependencies with several interfaces](#dependencies-with-several-interfaces)
    - [Dates](#dates)
    - [Exceptions](#exceptions)
      - [Throwing](#throwing)
      - [Catching](#catching)
    - [Checking things explicitly](#checking-things-explicitly)
    - [Calling parent constructor](#calling-parent-constructor)
    - [Traits](#traits)
    - [Arrays](#arrays)
    - [Default property values](#default-property-values)
    - [Scalar typehints](#scalar-typehints)
      - [Strict types declaration is required](#strict-types-declaration-is-required)
      - [Always use scalar typehints where appropriate](#always-use-scalar-typehints-where-appropriate)
    - [Void typehints](#void-typehints)
  - [Comments](#comments)
    - [PhpDoc on methods](#phpdoc-on-methods)
      - [No PhpDoc on fully strictly typed methods](#no-phpdoc-on-fully-strictly-typed-methods)
      - [Additional information in PhpDoc](#additional-information-in-phpdoc)
      - [PhpDoc on arrays](#phpdoc-on-arrays)
    - [PhpDoc contents](#phpdoc-contents)
    - [PhpDoc on properties](#phpdoc-on-properties)
    - [Fluid interface](#fluid-interface)
    - [Multiple available types](#multiple-available-types)
    - [Deprecations](#deprecations)
    - [Additional comments](#additional-comments)
    - [Comment styles](#comment-styles)
  - [IDE Warnings](#ide-warnings)
- [Main patterns](#main-patterns)
  - [Thin model](#thin-model)
  - [Services without run-time state](#services-without-run-time-state)
  - [Composition over inheritance](#composition-over-inheritance)
  - [Services (objects) over classes, configuration over run-time parameters](#services-objects-over-classes-configuration-over-run-time-parameters)
    - [Constant usage](#constant-usage)
  - [Small, understandable methods](#small-understandable-methods)
  - [Dependencies](#dependencies)
    - [No unnecessary dependencies](#no-unnecessary-dependencies)
    - [No circular dependencies](#no-circular-dependencies)
  - [Services](#services)
    - [Service creation](#service-creation)
    - [Changing entity state](#changing-entity-state)
      - [Use of managers](#use-of-managers)
      - [Methods for changing state](#methods-for-changing-state)
    - [Data types](#data-types)
      - [Identifier usage](#identifier-usage)
      - [Date and time](#date-and-time)
      - [Money](#money)
    - [One-to-many Relation in Services](#one-to-many-relation-in-services)
      - [Structure](#structure)
      - [Tags](#tags)
    - [Event dispatcher](#event-dispatcher)
  - [Handling sensitive values](#handling-sensitive-values)
    - [Logging](#logging)
    - [Passing sensitive values as arguments](#passing-sensitive-values-as-arguments)
    - [Storing sensitive values in entities](#storing-sensitive-values-in-entities)
- [Symfony related conventions](#symfony-related-conventions)
  - [Configuration](#configuration)
    - [Configuration inside bundles](#configuration-inside-bundles)
    - [Configuration in `app` directory](#configuration-in-app-directory)
    - [Parameters dist files](#parameters-dist-files)
      - [File naming](#file-naming)
      - [Parameter naming](#parameter-naming)
      - [Contents](#contents)
    - [Files](#files-1)
      - [Imports](#imports)
      - [Directories](#directories)
    - [Naming](#naming)
    - [Services](#services-1)
      - [Class names](#class-names)
      - [Factory services](#factory-services)
      - [ID as FQCN (class name)](#id-as-fqcn-class-name)
      - [Autoconfiguration and autowiring](#autoconfiguration-and-autowiring)
    - [Routing](#routing)
      - [Route prefix](#route-prefix)
      - [Route naming](#route-naming)
    - [Production configuration](#production-configuration)
    - [Always full service IDs](#always-full-service-ids)
  - [Repositories](#repositories)
    - [Injection](#injection)
    - [Configuration](#configuration-1)
    - [Finding by ID](#finding-by-id)
    - [Queries](#queries)
    - [Return types](#return-types-1)
      - [Basic usage](#basic-usage)
      - [Advanced usage](#advanced-usage)
    - [Method naming](#method-naming-1)
    - [Only custom methods from outside of repository](#only-custom-methods-from-outside-of-repository)
    - [Repositories from other bundles](#repositories-from-other-bundles)
    - [Prefetching and filtering at the same time](#prefetching-and-filtering-at-the-same-time)
    - [Searching / paginating by date](#searching--paginating-by-date)
  - [Entities](#entities)
    - [Setters](#setters)
    - [Setters vs adders](#setters-vs-adders)
    - [Relations in setters](#relations-in-setters)
    - [States, types etc.](#states-types-etc)
    - [ID](#id)
    - [Creation date](#creation-date)
    - [Updated at](#updated-at)
  - [Doctrine](#doctrine)
    - [Flush](#flush)
      - [Number of flushes](#number-of-flushes)
      - [Where to flush](#where-to-flush)
      - [Multiple flushes](#multiple-flushes)
    - [Database types and definitions](#database-types-and-definitions)
    - [Database naming](#database-naming)
      - [Underscored](#underscored)
      - [Plural](#plural)
      - [Prefix](#prefix)
      - [SQL keywords](#sql-keywords)
    - [Class-map](#class-map)
    - [Extendability pattern instead of class-map](#extendability-pattern-instead-of-class-map)
    - [Saving Money instances](#saving-money-instances)
      - [No getters for amount and currency](#no-getters-for-amount-and-currency)
      - [No handling with events](#no-handling-with-events)
      - [We do not store Money objects themselves](#we-do-not-store-money-objects-themselves)
      - [We store amount as decimal](#we-store-amount-as-decimal)
    - [Doctrine migrations](#doctrine-migrations)
    - [ID column strategy](#id-column-strategy)
    - [Variable types for query parameters](#variable-types-for-query-parameters)
  - [Controllers](#controllers)
    - [As services](#as-services)
    - [Static templates](#static-templates)
    - [Parameters passed to view](#parameters-passed-to-view)
    - [Small controllers](#small-controllers)
    - [Class naming](#class-naming-1)
  - [Events](#events)
    - [Available events](#available-events)
    - [Event naming](#event-naming)
    - [Listener naming](#listener-naming)
    - [Event class naming](#event-class-naming)
  - [Directory structure](#directory-structure)
    - [Root bundle directory](#root-bundle-directory)
    - [Basic directories for code](#basic-directories-for-code)
    - [Integration with other bundles](#integration-with-other-bundles)
    - [Special directories](#special-directories)
    - [Bundle namespace](#bundle-namespace)
    - [Example](#example)
  - [Twig](#twig)
    - [Calling methods](#calling-methods)
  - [Commands](#commands)
    - [Naming](#naming-1)
    - [Commands as services](#commands-as-services)
  - [Symfony version and new projects](#symfony-version-and-new-projects)
    - [Version and structure](#version-and-structure)
    - [Configuration](#configuration-2)
  - [REST in PHP](#rest-in-php)
    - [REST controllers](#rest-controllers)
    - [Result providers](#result-providers)
    - [Extending model](#extending-model)
    - [Permissions](#permissions)
    - [Pagination](#pagination)
  - [Migrating from legacy code](#migrating-from-legacy-code)
  - [New features](#new-features)
  - [Managing dependencies](#managing-dependencies)
    - [No hardcoded dependencies on legacy code](#no-hardcoded-dependencies-on-legacy-code)
    - [Interfaces](#interfaces)
    - [Configuration](#configuration-3)
    - [Example](#example-1)
  - [Database migrations](#database-migrations)
- [Composer Conventions](#composer-conventions)
  - [Semantic versioning](#semantic-versioning)
  - [Changelog](#changelog)
  - [Releases](#releases)
  - [Reviewing tagging policy](#reviewing-tagging-policy)
  - [Initial library development](#initial-library-development)
  - [Constraints on library versions](#constraints-on-library-versions)
  - [Constraints on vendors](#constraints-on-vendors)
  - [Library version incrementing](#library-version-incrementing)

<!-- tocstop -->

# PHP Basics

## Basics

### PHP code-level

For our libraries we use lowest PHP version that is used in any of currently actively maintained projects.

### Globals

We do not use global variables, constants and functions.

### Files

We put every class to it’s own file.

### Exceptions for code style usage

If we modify legacy code where some other conventions are used, we can use the same style as it is used there.

### Built-in language/framework feature usage
We prefer to use built-in features if they exist instead of inventing our own solutions, unless we need a very specific functionality.


## Code style

### Commas in arrays

If we split array items into separate lines, each item comes in it’s own line.
Comma must be after each (including last) element.

```php
<?php
['a', 'b', 'c'];
[
    'a',
    'b',
    'c', // notice the comma here
];
```

> **Why?**
>
> - each element in it's own line allows to easily scan all the elements - more understanding, less bugs;
> - comma after last element allows to easily reorder or add new elements to the end of the array.

### Splitting in several lines

If we split statement into separate lines, we use these rules:

-   `&&`, `||` etc. comes in the beginning of the line, not the end;

-   if we split some condition into separate lines, every part comes in it’s separate line
(including first - on the new line, and last - new line after). All of the parts are indented.

Some examples:

```php
<?php

return ($alpha && $beta || $gamma && $delta);

return (
    $alpha && $beta
    || $gamma && $delta
);

return ((
    $alpha
    && $beta
) || (
    $gamma
    && $delta
));

return (
    (
        $alpha
        && $beta
    )
    || (
        $gamma
        && $delta
    )
);

return ($alpha && in_array($beta, [1, 2, 3]));

return (
    $alpha
    && in_array($beta, [1, 2, 3])
);

return ($alpha && in_array(
    $beta,
    [1, 2, 3]
));

return ($alpha && in_array($beta, [
    1,
    2,
    3,
]));

return ($alpha && in_array(
    $beta,
    [
        1,
        2,
        3,
    ]
));

return (
    $alpha
    && in_array(
        $beta,
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

When making chain method calls, we put semicolon on it’s own separate line,
each of chained method calls is indented and comes in it’s own line.

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

> **Why?**
> Same as with arrays: allows to easily scan all the calls, reorder or add new ones when needed.

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

We use object names only for entities (value objects), not for services (for example `Page`).

#### Interface naming

We always add suffix `Interface` to interfaces, even if interface name would be adjective.

> **Why?** If we have base class witch implements the interface, we would have name clash.
> For example, `ContainerAware` and `ContainerAwareInterface`.


#### Property naming

We use nouns or adjectives for property names, not verbs or questions.

```php
<?php
declare(strict_types=1);

class Entity
{
    private bool $valid;       // NOT $isValid
    private bool $checkNeeded; // NOT $check
}
```

#### Method naming

We use verbs for methods that perform action and/or return something, questions only for methods which return boolean.

Questions start with `has`, `is`, `can` - these cannot make any side-effect and always return `boolean`.

For entities we use `is*` or `are*` for boolean getters, `get*` for other getters, `set*` for setters,
`add*` for adders, `remove*` for removers.

We always make correct English phrase from method names, this is more important that naming method to
`'is' + propertyName`.

```php
<?php
interface EntityInterface
{
    public function isValid(): bool;
    public function isCheckNeeded(): bool;
    public function getSomeValue(): string;
    public function canBeChecked(): bool;
    public function areTransactionsIncluded(): bool;

    // WRONG:
    public function isCheck(): bool;
    public function isNeedsChecking(): bool;
    public function isTransactionsIncluded(): bool;
    public function getIsValid(): bool;
}
```

```php
<?php
interface ControllerInterface
{
    public function canAccess(int $groupId): bool;

    /**
     * @throws AccessDeniedException
     */
    public function checkPermissions(int $groupId): void;
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

> **Why no ordering by visibility?** `protected` and `private` methods usually are used from single place in the code,
> so ordering by functionality (versus by visibility) makes understanding the class easier.
> If we just want to see class public interface, we can use IDE features
> (method names ordered by visibility or/and name, including or excluding inherited methods etc.)


### Directories and namespaces

#### Singular namespaces

We use singular for namespaces: `Service`, `Bundle`, `Entity`, `Controller` etc.

Exception: if English word does not have singular form.

#### No `*Interface` namespaces

We do not make directories just for interfaces, we put them together with services by related functionality
(no `ServiceInterface` namespace).

#### Different namespaces and service names

We do not use service names for namespaces, instead we use abstractions.
For example, namespace should be `UserMerge` or `UserMerging`, not `UserMergeManager`.

### Comparison order

If we compare to static value (`true`, `false`, `null`, hard-coded integer or string),
we put static value in the right of comparison operator.

Wrong: `null === $something`

Right: `$something === null`

> **Why?** Speak clearly to be understood correctly, you should. Yeesssssss.

### Namespaces and use statements

If class has a namespace, we use `use` statements instead of providing full namespace.
This applies to PhpDoc comments, too.

Wrong:

```php
<?php

namespace Some\Namespace;

// ...

public function setSomething(\Vendor\Namespace\Entity\Value $value);
```

Right:

```php
<?php

namespace Some\Namespace;

use Vendor\Namespace\Entity\Value;

public function setSomething(Value $value);
```

## Usage of PHP features

### Condition results

If condition result is boolean, we do not use condition at all.

Do **not** do this:

```php
<?php
$alpha = $delta && $gamma ? false : true;

if ($alpha && $beta) {
    return true;
}

return false;
```

Do this:

```php
<?php
$alpha = !($delta && $gamma);

return $alpha && $beta;
```

### Logical operators

We use `&&` and `||` instead of `and` and `or`.

> **Why?** `and` and `or` have different priorities and commonly leads to unintended consequences

### Strict comparison operators

We use `===` (`!==`) instead of `==` (`!=`) everywhere. If we want to compare without checking the type, we should
cast both of the arguments (for example to a string) and compare with `===`.

Same applies for `in_array` - we always pass third argument as `true` for strict checking.

We convert or validate types when data is entering the system - on normalizers, forms or controllers.

> **Why?** Due to security reasons and to avoid bugs


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

Exception: When variable can be not only boolean, but also `int` or `null`.

```php
<?php
return strpos('needle', 'haystack') === false;
```

### Comparing to `null`

When comparing to `null`, we always compare explicitly.

```php
<?php

function func(?ValueClass $value = null): ?string
{
    return $value !== null ? $value->getSomething() : null;
}

//or in php8
function func(?ValueClass $value = null): ?string
{
    return $value?->getSomething();
}
```

We also do not use `is_null` function for comparing.

### Assignments in conditions

We do not use assignments inside conditional statements.

Exception: in a `while` loop condition.

Wrong:

```php
<?php
if (($beta = $alpha->get()) !== null && ($gamma = $beta->get()) !== null) {
    $gamma->do();
}
if ($project = $this->findProject()) {

}
```

Correct:

```php
<?php
$beta = $alpha->get();
if ($beta !== null) {
    $gamma = $beta->get();
    if ($gamma !== null) {
        $gamma->do();
    }
}

$project = $this->findProject();
if ($project !== null) {

}
```

> **Why?** We save a few lines of code but code is less understandable - we make several actions at once.
> Furthermore, as we explicitly compare to `null`, conditional-assignment statements become complicated.


### Unnecessary variables

We avoid unnecessary variables.

Wrong:

```php
<?php

function getSomething(): string
{
    $a = get();
    return $a;
}
```

Correct:

```php
<?php

function getSomething(): string
{
    return get();
}
```

This does not mean that we cannot use them to make code more understandable. For example,
this way of variable usage is fine:

```php
<?php

function canModify(ValueClass $object): bool
{
    $rightsGranted = isAdmin() || isOwner($object);
    $objectEditable = isNew($object) && !isLocked($object);

    return $rightsGranted && $objectEditable;
}

```

### Reusing variables

We do not set value to variable passed as an argument.

We do not change the type of any variable.

Instead, in both cases, we create new variable with different name.

```php
<?php

public function thisIsWrong(int $number, string $text, Request $request): void
{
    $number = (string)$number;  // Illegal: we 1) change argument value 2) change it's type
    $text .= ' ';  // Illegal: we change argument value
    $document = $request->get('documentId');
    $document = $this->repository->find($document); // Illegal: we change variable's type
    // ...
}

public function thisIsCorrect(string $numberText, string $text, Request $request): void
{
    $number = (int)$numberText;
    $modifiedText = $text . ' ';
    $documentId = $request->get('documentId');
    $document = $this->repository->find($documentId);
    // ...
}
```

> **Why?** To understand code better (quicker, easier) and thus to avoid bugs. This also integrated with IDE better
> so it can give better static analysis support

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
-   `dataProvider` in phpunit 10

### Converting to string

We do not use `__toString` method for main functionality, only for debugging purposes.

> **Why?** We cannot throw exceptions in this method, we cannot put it in interface,
> we cannot change return type or give any arguments - refactoring is impossible (only by changing it to simple method).
> Also if debugging uses \_\_toString method, we cannot change it to get any additional information.


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
declare(strict_types=1);

class Alpha {
    private int $alpha;      // this must be defined as a property
    public int $beta;       // this is illegal

    public function __construct(int $alpha)
    {
        $this->alpha = $alpha;
    }
}
```

#### Method visibility

We prefer `private` over `protected` over `public` as it constraints the scope - it's easier to refactor, find usages,
plan possible changes in code. Also IDE can warn about unused methods or properties.

We use `protected` when we intend some property or method to be overwritten if necessary in extending classes.

We use `public` visibility only when method is called from outside of class.

### Functions

#### `count`

We use `count` instead of `sizeof`.

#### `is_null`

We use compare to `null` using `===` instead of `is_null` function. For example:

```php
if ($result === null) {
    // ...
```

### String manipulation

We use `trim, ltrim, rtrim` or whatever is more modern prefix and suffix removal.
### Return and argument types

#### Return types

We always return value of one type. Optionally, we can return `null` when using any other return type, too.

For example, we can*not* return `boolean|string` or `SuccessResult|FailureResult`
(if `SuccessResult` and `FailureResult` has no common class or interface; if they do, we document to return that
interface instead).

We can return `?SomeClass` or `?string`.

Note: when we are always returning the same class but it's iterable of other classes, we can (and should)
provide `Collection|SomeClass[]|null` as return value.

#### Argument types

Same applies for any argument.

#### Passing ID

Exception: When making query from repository, both `int|Entity` can be taken (object or it’s ID).
We give priority to object in this case if it’s available.

Usually this should not be done as it would probably violate some other convention.

#### Type-hinting optional arguments

If argument is optional, we provide default value for it.

If optional argument is object, we type-hint it with required class and add default to `null`.

If argument is not optional, but just nullable, we can typehint it with default value `null`, but when using,
we pass `null` explicitly.

Example:

```php
<?php
declare(strict_types=1);

class Service
{
    public function __construct(SomeService $s, ?LoggerInterface $logger = null)
    {
        //...
    }
}
```

```php
<?php
declare(strict_types=1);

class Entity
{
    public function setValue(?ValueClass $value): self
    {
        //...
        return $this;
    }
}

$entity->setValue($someValue);
$entity->setValue(null);
// but we do not use $entity->setValue();
```

#### Void result

We always return something or return nothing. If method does not return anything ("returns" `void`),
we do not return `null`, `false` or any other value in that case.

If method must return some value, we always specify what to return, even when returning `null`.

Wrong:

```php
<?php
function makeSomething(): bool
{
    if (success()) {
        return true;
    }
}
function getValue(MyObject $object): string
{
    if (!$object->has()) {
        return;
    }
    return $object->get();
}
/**
 * @throws PaybackUnavailableException
 */
function payback(int $requestId): bool
{
    makePayback($requestId);
    return true;
}
```

Correct:

```php
<?php
function makeSomething(): ?bool
{
    if (success()) {
        return true;
    }
    return null;
}
function getValue(MyObject $object): ?string
{
    if (!$object->has()) {
        return null;
    }
    return $object->get();
}
/**
 * @throws PaybackUnavailableException
 */
function payback($requestId): void
{
    makePayback($requestId);
}
```

> **Why?** If method result is not used and you return something anyway, other programmer may assert that return value
> is indeed taken into account. For example, return `false` on failure, even if this failure is not handled anyhow by
> the functionality that's calling your method.

### Type-hinting classes and interfaces

We always type-hint narrowest possible interface which we use inside the function or class.

> **Why?** This allows us to refactor easier. We can just provide another class, which implements the same interface.
> We can also find real usages quicker if we want to change the interface itself
> (for example `RouterInterface` vs `UrlGeneratorInterface` when we change declaration of `RouterInterface::matches`).


#### Dependencies with several interfaces

If we have dependency on service with several responsibilities (which implements several interfaces),
we should inject it twice. For example:

```php
declare(strict_types=1);

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

We use `\DateTimeImmutable` objects to represent date or date and time inside system.

We use `\DateTimeInterface` where we get the date to be more compatible with functionality that still operates
`\DateTime` objects.

> **Why?** Because immutable version is preventing accidental data modification by design.

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

> **Why?** Because usually in these situations tens or hundreds different situations could have gone wrong but
> we make assumption that it was that single one. This is how we not only make the system behave incorrect on
> unexpected failures, but also ignore the real problems by not logging them.

### Checking things explicitly

We use only functions or conditions that are designed for specific task we are trying to accomplish.
We don’t use unrelated features, even if they give required result with less code.

We avoid side-effects even if in current situation they are practically impossible.

Some examples:
- we use `isset` versus `empty` if we only want to check if array element is defined;

- we use `$x !== ''` instead of `strlen($x) > 0` - length of `$x` has nothing to do with what we are trying
to check here, even if it gives us needed result;

- we use `count($array) > 0` to check if array is not empty and not `!empty($array)`,
as we do not want to check whether `$array` is `0`, `false`, `''` or even not defined at all
(in which case IDE would possibly hide some warnings that could help noticing possible bugs).

> **Why?** We avoid side-effects if some related code changes or is refactored.
> Code is much easier to understand if we see specific checks or function calls instead of something unrelated that
> happens to give the needed result.


### Calling parent constructor

If we need to call parent constructor, we do it as first statement in constructor. For example:

```php
public function __construct(int $arg1, int $arg2)
{
    parent::__construct($arg1);
    $this->setArg2($arg2);
}
```

> **Why?** Parent class can have some mandatory stuff to do before we can use it's functionality.
> See following example. This is the only way to call parent constructor in some (probably most)
> of other languages (etc. JAVA)


Example of parent class, with which calling constructor later would fail:

```php
/**
 * @var int[] $params
 */
protected array $params;
public function __construct(int $arg1)
{
    $this->params = [$arg1];
}
public function setArg2(int $arg2)
{
    $this->params[] = $arg2;
}
```

### Traits

The only valid case for traits is in unit test classes. We avoid using traits in our base code.

> **Why?** We use (classical) OOP to refactor functionality to avoid duplicated code.


### Arrays

We always use short array syntax (`[1, 2]` instead of `array(1, 2)`) where PHP code-level allows it.

### Default property values

If we need to define some default value for class property, we do this in constructor, not in property declaration.

> **Why?** Some default values cannot be set when declaring properties, so this way everything is in one place.
> It especially makes sense when entity has lots of properties.


```php
<?php
declare(strict_types=1);

class MyClass
{
    private const TYPE_TWO = 'two';

    private ArrayCollection $one;
    private string $two;
    private int $three;
    private bool $four;

    private DateTimeImmutable $createdAt;

    public function __construct()
    {
        $this->one = new ArrayCollection();
        $this->two = self::TYPE_TWO;
        $this->three = 3;
        $this->four = false;
        $this->createdAt = new DateTimeImmutable();
    }
}
```
or php8
```php
<?php
declare(strict_types=1);

class MyClass
{
    private const TYPE_TWO = 'two';

    public function __construct(
        private ArrayCollection $one = new ArrayCollection(),
        private string $two = self::TYPE_TWO,
        private int $three = 3,
        private bool $four = false,
        private DateTimeImmutable $createdAt = new DateTimeImmutable()
    ) {
    }
}
```

### Scalar typehints

#### Strict types declaration is required

For newly written files we always add `declare(strict_types=1);` at the top of the file.

We add it for already existing files if we can guarantee that no negative side-effects would be caused by it.
It's safer to type-cast variables in the file to the correct scalar types to keep the consistent behavior.

#### Always use scalar typehints where appropriate

We always use scalar typehints both for arguments and return types, unless our code with them would not work
as intended so we are forced not to use them.

For already existing methods we can add scalar typehints but we're responsible to guarantee that any place
in the project that has strict types declaration and calls this method always passes the correct type as an argument.
We could add typecast in the places where we're not guaranteed to keep the consistent behavior.

If we add scalar typehint for any argument in a public method in our library, this means that major release is needed.
Scalar typehints can be always safely added to return values if needed, though.

```php
<?php

declare(strict_types=1);

class Something
{
    public function increment(int $alpha): int
    {
        return $alpha + 1;
    }
}

// some old interface from some old library we have to implement
interface SomeOldInterface
{
    /**
     * @param string $withSomething
     * @return Result[]
     */
    public function doOldStuff($withSomething);
}

class OurShinyNewClass implements SomeOldInterface
{
    /**
     * @param string $withSomething
     * @return Result[]
     */
    public function doOldStuff($withSomething): array // we can not declare argument type, but we can narrow return type
    {
        return [];
    }
}
```

### Void typehints

For functions and methods that do not return value, we should typehint return value as `: void`.

```php
<?php

declare(strict_types=1);

class SomeClass
{
    public function doSomething(): void // we must typehint return value as `void` here as method does not return anything
    {
        echo "Hi!";
    }
}
```

## Comments

### PhpDoc on methods

#### No PhpDoc on fully strictly typed methods

If PhpDoc does not add any other information that's already provided with typehints, we don't use PhpDoc at all.

Wrong:
```php
/**
 * @param Client $client
 * @param string $type
 *
 * @return Money
 */
public function getLimit(Client $client, string $type): Money;

public function setNumber($number);
```

Correct:
```php
public function getLimit(Client $client, string $type): Money;

public function setNumber(int $number): self;
```

> **Why?** Any comment needs to be maintained – if we add parameter, change parameter or return type, we must also update the PhpDoc. If PhpDoc does not add any additional information, it just duplicates information that's already provided.

#### Additional information in PhpDoc

If method description, parameter descriptions or any other PhpDoc tags other than `@param` and `@return` are used
 we will add a full PhpDoc like the following:

```php
/**
 * Gets a day limit starting from midnight.
 *
 * @see http://example.com/
 *
 * @param Client $client
 *
 * @return Money
 */
public function getDayLimit(Client $client): Money;
```

#### PhpDoc on arrays

When we take as an argument or return an array of strictly typed elements, we should add a PhpDoc to describe the
type of elements in the array.

```php
/**
 * @param Client[] $clients
 *
 * @return Money
 */
public function getDayLimitForClients(array $clients): Money;
```

### PhpDoc contents

If we use PhpDoc comment, it must contain all information about parameters, return type and exceptions that the method
throws. If method does not return anything, we skip `@return` tag.

> sometimes scalar types are not autocompleted by IDE, but must be provided by this requirement.
> Example: `@param string $param1`


### PhpDoc on properties

We use PhpDoc on properties that are not injected via constructor.

We do *not* put PhpDoc on services, that are type-casted and injected via constructor, as they are automatically
recognised by IDE and desynchronization between typecast and PhpDoc can cause warnings to be silenced.

### Fluid interface

If method returns `$this`, we use typehint `: self` that IDE could guess correct type if we use this method
for objects of sub-classes.

### Multiple available types

If we return or take as an argument value that has few types or can be one of few types,
we provide all of them separated by `|`.

```php
<?php
/**
 * Description of the method - optional
 *
 * @param string $param1 description of param1. Description is optional, type is required (string)
 *
 * @return SomeObject[]|Collection|null Optional description of return type
 */
public function getSomething(string $param1)
{
    // ...
    return $collection;
}
```

> **Why?** This way we
> 1) know if `null` value can be returned or passed as an argument;
> 2) can use methods with auto-completion from both of specified types.
> This is very convenient for collections - we can use `$collection->count()`
> and `foreach ($collection as $a) {$a->someMethod();}`


### Deprecations

If method is deprecated, we mark this in PhpDoc as `@deprecated` with description or additional
`@see` with new method to use. For example:

```php
@deprecated use DI to create and inject this service
```

```php
@deprecated
@see \Acme\Component\SomeComponent::someMethod
```

### Additional comments

We do not add file comments.

> **Why?** We use class comments - they are used if PhpDoc is auto-generated from PHP code, also can be used by IDE.


We do not use `@package`, `@namespace`, `@date` or `@author` annotations anywhere.

> **Why?** Namespace is provided by PHP keyword. Both date and author can be seen via annotations and becomes stale
> really quickly. After refactoring, the only name in the file can be totally irrelevant.

We do not use `@inheritdoc`.

> **Why?** It does not contain information for programmer. For IDE, no PhpDoc at all has the same results.


### Comment styles

We use multi-line `/** */` comments for method, property and class annotations.

We use single-line `/** @var Class $object */` annotation for local variables. We always avoid this if possible -
usually PhpDoc is enough (sometimes it's missing, for example in vendor code).

We can use `//` single line comments in the code.

We do not use `/* */` or `#` comments at all.

> **Why no multi-line comment?** We try to keep functions small and comment them in PhpDoc.
> If some things are to be explained in function body, single line comments should be enough.
> If we want to comment-out something, we just delete it (and use VCS to revert if needed) or disable functionality
> in configuration.

Some examples:
```php
<?php

class NewsletterSender
{
    /**
     * @var ProcessorInterface[] this must be multiline - 3 lines, not 1
     */
    private $processors;

    public function sendNewsletter(Newsletter $newsletter): void
    {
        foreach ($this->processors as $processor) {
            /** @var Mail $mail this must be single line - 1 line, not 3 */
            $mail = $processor->createNamedMail($newsletter->getName());
            //..
        }

        // we can add single line comments to explain behavior
    }
}
```

We always prefer refactoring code to be understandable over adding some comments to explain it's behavior.
Most of the cases, code should be clear enough to not need any comments at all.

> **Why should we avoid comments?** They are technical debt - if you cannot keep it updated, it gets stale real quick.
> When you cannot trust some of the comments, you don't know which ones you can really trust.

## IDE Warnings

We always avoid IDE warnings if possible (it's not possible only when there is an IDE bug or some similar case).
We provide `@var` PhpDoc comment if IDE cannot guess the type of variable and we cannot fix method declaration for
type to be correctly guessed (vendor code etc.)

---------


# Main patterns

## Thin model

We use Plain Value Objects (we name them Entities) for moving information/data around functionality.

We do not put any logic into those objects - all logic is handled outside, in services.

This allows to change and configure behaviour in different context.

## Services without run-time state

In general we try to make services without run-time state.
That is, once configured, it should not change it’s behaviour in run-time.

This does not apply to specific services, which have, for example, to collect some events and redispatch them in the
end of request etc. - in those cases where collecting the state in run-time is their primary objective.

Instead of:

```php
<?php
class BadExample
{
    protected string $name;
    public function setManagerName(string $name): self
    {
        $this->name = $name;

        return $this;
    }
    public function search(): void
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
    public function search(string $managerName): void
    {
        // ...
    }
}
```

> **Why?** When services become context-aware unintentionally, this makes unpredicted side-effects.
> More bugs, which can be very hard to debug and/or test. Much more test scenarios. Much harder to refactor.

> This is similar to using globals - if we store run-time context,
> we become unaware of all the things that can influence the behavior.


## Composition over inheritance

We always try to use composition instead of inheritance.

If we want to make some abstract method - we inject interface into constructor with that method.

This helps to follow single responsibility principle. Also this often allows to configure object by injecting different
functionality without explosion of classes. If we have 2 abstract methods and 2 possible logic for each of them,
we would need 4 classes with 8 methods total, also having some duplicated code.
If we inject services, we only need 4 classes with 1 method each with no duplicated code.

Also, this allows to test code much easier, as we can test each functionality separately.

## Services (objects) over classes, configuration over run-time parameters

This is related to *composition over inheritance* - we try to configure each service instead of hard-coding
any of parameters in the code itself.

For example, instead of:

```php
class ServiceA
{
    public function doSomething(): void
    {
        $alpha = Manager::getInstance();
        $alpha->doSomething('a');
    }
}
class ServiceB
{
    public function doSomething(): void
    {
        $alpha = Manager::getInstance();
        $alpha->doSomething('b');
    }
}
$alpha = new ServiceA();
$beta = new ServiceB();
```

we use:

```php
class Service
{
    private Manager $manager;
    public function __construct(Manager $manager)
    {
        $this->manager = $manager;
    }
    public function doSomething(): void
    {
        $this->manager->doSomething();
    }
}
$alpha = new Service(new Manager('a')); // we use Dependency Injection Container for creating services, this is just an example
$beta = new Service(new Manager('b'));
```

This allows to reuse already tested code in different scenarios just by reconfiguring it.

Of course, we still need to test the configuration itself (functional/integration testing), as it gets more complicated.

### Constant usage

We do not store configuration in constants.

> **Why?** As configuration can change, it is not constant. Possible situation: `const SOFT = 'soft'; const HARD = 'soft';`

## Small, understandable methods

We try to write code that is self-explanatory.
This implies writing small methods - we should understand what the method does by looking at it's code.
We also try to name methods by their meaning, which also helps understanding the code.

## Dependencies

We always take into account component or bundle dependencies.

### No unnecessary dependencies

No unnecessary dependencies between components or bundles must be created.

If there is some abstract functionality, it must not depend upon it’s implementations.

### No circular dependencies

No circular dependencies between components or bundles must be created.

## Services

### Service creation

Services can be created only by factory classes or dependency injection container.
We do not create services in other services not dedicated to do that (for example, in constructor).

### Changing entity state

#### Use of managers

We make changes to entity state in managers or some similar designated services.

Manager can make the following actions:

-   check initial entity state before changing it, validate it;
-   change the state;
-   dispatch event;
-   make some additional actions.

If state is changed in a controller or some other place, duplicated code can occur when functionality is
required in another place. With duplicated code, different behavior can occur, which eventually leads to bugs.

#### Methods for changing state

We prefer concrete methods instead of one abstract method to change state.

This allows us to see available actions and make our code clearer:
no `switch` statements, event maps, complex validation rules, nested \`if\`s or similar magic.

```php
<?php
declare(strict_types=1);

class Manager
{
    // simple short methods with clear logic:
    public function accept(Request $request): void
    {
        $this->assertPending($request);
        $request->setStatus(Request::STATUS_ACCEPTED);
        $this->eventDispatcher->dispatch(RequestEvents::ACCEPTED, new RequestEvent($request));
    }
    public function deny(Request $request): void
    {
        $this->assertPending($request);
        $request->setStatus(Request::STATUS_DENY);
        $this->eventDispatcher->dispatch(RequestEvents::DENIED, new RequestEvent($request));
    }

    // more complicated and totally unclear from interface
    public function changeStatus(Request $request, string $status): void
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

To make example more complicated, we can imagine that different validation groups must be provided for validator
for each of the given status and some other event class should be provided if request is denied.
Furthermore, denial could take one more argument - `DenialReason $reason`.

### Data types

We use objects to represent data when possible.
For example, if data can be represented by both scalar value and an object (like date), we use object.
When several variables belong to single item, we use an object representing them and do not pass or operate on
these separately (like money object versus amount and currency as separate variables).

We always try to use single representation of that data item - we avoid having multiple classes which represent the same
object in the system (for example Transfer as DTO in common library and as an Entity - these could both co-exist,
but we try to have an Entity as soon as possible in the system).

We normalize data as soon as possible in the system (input level) and denormalize it as later as possible
(output level). This means that in service level, where our business logic resides,
we always operate on these objects and not their concrete representations.

#### Identifier usage

We operate with objects inside services, not their identifiers.
Objects must be resolved by their identifiers in controller, normalizer or similar input layer.
In business objects (entities or simple DTOs, like `Filter`) we already have other objects, taken from database.

If we do not find object by identifier, we throw exception and return `404` as a response
(or specific error code with `400`).

#### Date and time

In some cases we use integer UNIX timestamp to represent date with time.
When creating `DateTimeImmutable`, we must not use constructor with `@` as it ignores the time zone of the server.

For example:

```php
<?php
$createdAt = new DateTimeImmutable();
$createdAt->setTimestamp($data['created_at']);
$entity->setCreatedAt($createdAt);
```

#### Money

Always amount and currency, never only amount.

### One-to-many Relation in Services

#### Structure

If there is functionality that can be implemented in different ways and should be selected at runtime,
we create Interface for it and use Manager (or separate Registry class) to collect all those services that implement it.

From code, we do not call those services directly, we call Manager which chooses the correct service and
redirects call to it.

This way if Interface changes, we only have to make changes in the Manager.
Also, if we need to do some action every time some method is called, we can add it to manager,
no need to put it to all those services.

Example:

```php
declare(strict_types=1);

class Manager
{
    /**
     * @var ProviderInterface[]
     */
    protected $providers = [];

    public function addProvider(ProviderInterface $provider, string $providerKey): self
    {
        $this->providers[$providerKey] = $provider;

        return $this;
    }

    public function doSomething(Data $data): string
    {
        if (!isset($this->providers[$data->getProviderKey()])) {
            throw new RuntimeException();
        }

        return $this->providers[$data->getProviderKey()]->doSomething($data);
    }
}
```

#### Tags

We use dependency injection container tags to add all those services to the manager.

### Event dispatcher

We use events to notify about some result or to optionally change behaviour before making some action.

We do not use events to actually make some action happen, which is mandatory for the place which dispatches the event.

We should not make assumptions about listeners of some event.
If we require a result from event listeners, this indicates that we should refactor functionality and use interfaces
with tags or some similar solution.


## Handling sensitive values

Sensitive values are those that are secret and should not be known or read in plain text.
For example passwords, tokens, secret generated codes etc. Any values that can affect security of the system.

### Logging

We never log content where sensitive values can be found. For example, request content for all requests, as these
include call to authentication. We must either
1) do not log at all;
2) make whitelist what to log;
3) make blacklist when to avoid logging (this is not recommended as can be easily forgotten).

When we configure loggers, we always use normalizers that does not extract private fields from any given objects
by default.

### Passing sensitive values as arguments

As we log exception stack trace, any scalar values passed as arguments can be used in stack trace.
To avoid that, we put sensitive values to `SensitiveValue` object as soon as they enter our system.
We pass them as this object. We get the value itself only in lowest level possible.

If we cannot pass `SensitiveValue` (for example, to vendor library), we catch `\Exception` and re-throw a new one,
without providing last exception from before (we can copy the message itself, if it does not contain sensitive values).

### Storing sensitive values in entities

If we really need to store sensitive value in an entity, we make that property private.
We also make that setter and getter would operate with `SensitiveValue` and not scalar type itself.

Better approach is to always use hash of sensitive values, if we just need to check if provided value matches it.
If we need to resend generated code or use original value later, only in that case we save generated value (like code)
in plain-text.







# Symfony related conventions

This chapter describes conventions related directly with Symfony framework and related components (Doctrine, Twig).

It also includes some closely related conventions that can be used independently (for example in libraries).

## Configuration

### Configuration inside bundles

We use XML configuration inside bundles.

> **Why not YAML?** We use XML, because it is explicit and has schema definition - many mistakes can be avoided.

> **Why not annotations?** We don’t use annotations, as we want to leave configuration and logic separate.
> Moreover, class and object/service are not the same, this makes it difficult to configure several services
> for a single class.

### Configuration in `app` directory

We use YAML configuration inside `app` directory for semantic configuration files.

> **Why?** Configuration is semantic here and thus not very complex.
> Most of the examples use only this format, it’s default for Symfony standard distribution.

We still use XML for service definitions inside `app` directory.

### Parameters dist files

#### File naming

We use `{origName}.dist.{origExt}` name for distribution versions of parameters.

> **Why?** Original extension is left the same so we can use all IDE features.
> It is configurable (or not important) so we **can** do that.

#### Parameter naming

We do not put bundle prefix to parameters that we only use outside the bundle, for example to configure
`config.yml` file. Treat them as private to that bundle.

**Incorrect**:

`config.yml`:

```yaml
acme_cool_feature:
    parameter: %acme_cool_feature.parameter%
    option: %acme_cool_feature.option%
```

`parameters.dist.yml`:

```yaml
acme_cool_feature.parameter: A
acme_cool_feature.option: B
```

**Correct**:

`config.yml`:

```yaml
acme_cool_feature:
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

Because when using bundle prefix, we make assumptions about their usage inside the bundle itself.
It would brake or have unintended effects in a case like this:

```php
// ...
$definition->setArguments(array($config['parameter'], $config['option']));

// here we have a problem - our parameter from parameters.yml unintentionally overwrites this parameter in the bundle:
$container->setParameter('evp_cool_feature.parameter', 'ABC');
// ...
```

In other words, we have prefixes to avoid parameter clashing, by using prefix in `config.yml`
(or other bundles - that's the same), we can break things.

#### Contents

Default parameters (inside `parameters.dist.yml`) must be configured for development environment.

> **Why?** We cannot use production as this would be security issue.
> All developers can add default values and change them only if needed.

We use value `pass` for default passwords in development environment.

We use default domains in parameters - if everywhere configured the same, no changes in parameters should be needed.

Basic rule: using default parameters must always work for any developer out-of-the-box with default environment set-up.

### Files

#### Imports

When configuring services, we split definitions into separate files and import them in main
`services.xml` and `routing.xml` files. For example:

```txt
config/services/controllers.xml
config/services/forms.xml
config/services/repositories.xml
config/services/normalizers.xml
```

We put routing prefix into `<import/>` directive.

#### Directories

We put configuration files in subdirectories - `services/`, `routing/` etc.

### Naming

Service ID always begins with bundle identifier, like `acme_newsletter`, followed by dot.

If type of service is repository, controller, listener or normalizer, these are following parts:

-   type of service, like `repository`, `controller`, `normalizer`
-   name of the service, excluding the type.

If service is a manager, registry or some other unique service for that bundle, we use it’s identifier directly without the type.

Valid service names: `acme_page.page_manager`, `acme_page.repository.page`

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

> **Why?** This makes unnecessary difficulty when understanding available services and their structure.
> If we need to overwrite service, we overwrite it by it's ID, so we could change not only the class name,
> but also constructor arguments or make additional method calls.
> If functionality is to be changed by circumstances, we should make ability to change this service
> using bundle semantic configuration.


#### Factory services

We use new syntax for defining factory services:

```xml
<service id="newsletter_manager" class="NewsletterManager">
    <factory class="NewsletterManagerFactory" method="createNewsletterManager"/>
</service>
```

#### ID as FQCN (class name)

We do not use service IDs as class names by default.

> **Why?** Even if this allows easier configuration in some cases and avoids naming concerns for our services,
> it could be hard to refactor code when another service for the same class is added to our project.
> This would make one of the services "default" one, which would make debugging harder and understanding of
> internal working more difficult.

> For example, if we have `BalanceProvider` and we would add another service for providing balance with debts included
> (using same class with different configuration), this would require to go over every usage of the first service and
> changing it's ID.

We can use class name as service ID (or alias it) when we are providing open-source bundle.
This allows others to use autowiring features if needed, even if we don't use them.
We should only use this for public services - ones that are to be used outside of our bundle.

Exception: for controllers we use classname as service ID.
> **Why?** Because routing annotations work only when controller's service ID is it's classname
> (otherwise it has no way to know it).

#### Autoconfiguration and autowiring

We don't use autowiring and autoconfiguration features of Dependency Injection component.

> **Why?** First of all, we don't use FQCN as service IDs, so this would not work.
> Secondly, it might be quicker at the beginning to use it, but to understand the code, we would need to guess where
> the service really comes from. If we would need to refactor something, it might require to write DI configuration
> for already (automatically) defined services, too.

### Routing

#### Route prefix

We always use bundle name as route prefix, just like in services. We leave out vendor prefix.

Exception: we drop out `-common` suffix, if one exists. Routes should not clash with corresponding
bundle without `-common`.

#### Route naming

We try to use names to identify the action taken by route, not methods to take it.

For example, `create_page` instead of `page_post`.

### Production configuration

We do not put specific configuration in bundles - we provide semantic configuration for those and define them inside
`app/config.yml`.

We do not put secret parts of production configuration anywhere in repository - no private key files, common secret
strings, passwords etc. They must be ignored by git and provided in `parameters.yml`, shared directory or in
some other way.

### Always full service IDs

We never dynamically build parameter names or service IDs. We use tags if we need abstraction.
We define map or state all things explicitly if needed.

Do not do this:

```php
$container->get('my_prefix.' . $type . '.provider');
```

> **Why?**
> 1) It's hard to find all possible services which are registered (and thus hard to refactor - many potential pitfalls);
> 2) we do not control if service is even registered in container;
> 3) we cannot add some other configuration;
> 4) we do not test if service implements some specific interface;
> 5) we cannot get all possible types available or all possible services.

Instead do this:

```php
public function addProvider(SomeInterface $provider, string $type): self
{
    $this->providers[$type] = $provider;

    return $this;
}

public function getProvider(string $type): ProviderInterface
{
    if (!isset($this->providers[$type])) {
        throw new RuntimeException();
    }

    return $this->providers[$type];
}

// ...

public function build(ContainerBuilder $container): void
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

> **Why?** Creating repository class requires loading Entity metadata, which requires cache to be loaded at
> service construction

### Finding by ID

For finding entity by ID we always use `find` method, not `findOneById` etc.

Exception: If we want to preload some related entities.

Another exception: If we override `findOneById` to contain call to `find`.
This could be used to provide PhpDoc for IDE to guess the type returned.

> **Why?** `find` uses internal Doctrine cache, while simple queries to database (even only by ID) by default does not

### Queries

We use query builder and queries inside repositories only - we don’t build queries inside controllers or other services.

### Return types

#### Basic usage

Repository methods starting with `find` returns instance(-s) of the class, that this repository is related to,
or scalar values.

#### Advanced usage

Let's take an example:
- we have `UserBundle`, which does not have any dependencies;
- we have `AvatarBundle`. It does depend on `UserBundle`, but reverse is not true (and shouldn't).

We need some repository to return `User` entities, that does not have any avatars.

We cannot put this to `AvatarRepository`, as it should return only `Avatar` entities.
We cannot put it anywhere in the `UserBundle` as it would make a hard (and unwanted) dependency on `AvatarBundle`.

In this case we create a separate `UserRepository` service inside `AvatarBundle`, inject `EntityManager` manually
and make needed queries in there.

### Method naming

We use `findOneBy*` to find one object and `findBy*` to find list of objects.

We use `getQueryBuilderFor*` to get query builders (for example for pagination),
`findCountBy*` etc. to get scalar results.

### Only custom methods from outside of repository

We do not use `findOneBy` or `findBy` methods from outside of repository.
We create method inside repository for that concrete use-case. In that method, we can call `$this->findBy(...)`
with given parameters.

> **Why?** This allows to add more constraints in one single place. For example, after adding `deleted` column we can
> update all queries inside repository to filter those records out by default.
> Also we can see all possible queries in one place - this allows us to easily review which indexes should be
> defined and which are unused.

### Repositories from other bundles

We avoid direct usage of repositories from other bundles than the repository belongs to.

If we need to get information from another bundle,
we tend to use services (which, in that bundle, can inject that repository).

> **Why?** This creates proxy service for getting needed information.
> Even if at first it would only pass method call to repository itself, when something changes we can refactor it
> easier - we can inject other services, handle filtering of arguments or results etc.
> This helps a lot if method in repository is used from many many places inside whole project and we need to change
> some logic with other injected service.

### Prefetching and filtering at the same time

We do not select related entities if we filter by them. For example, do **not** do this:

```php
<?php
return $this->createQueryBuilder('user')
    ->select('user, email')
    ->leftJoin('user.emails', 'email')
    ->where('email.status = :activeStatus')
    ->setParameters([
        'activeStatus' => User::STATUS_ACTIVE,
    ])
    ->getQuery()
    ->getResult()
;
```

If we iterate over user entities that were returned, and call `getEmails()` on them, we will **not** get all emails
for that user. We will get only those that matched the original query (active emails in this case).
Furthermore, it breaks code such as this later in the process:

```php
$user = $userRepository->find(123);
$user->getEmails();  // this will also return *only* active emails, as the user is cached in Doctrine's identity map
```

What are the options?

1) change `->select('c, a')` to `->select('c')`, so we do not pre-fetch accounts;
2) pre-fetch them with a separate join:
Keep in mind that pre-fetching is OK and recommended in most of the cases where we will use those relations.
It's just not good if we also filter by that relation.
Further work and filtering should still be done within the code as results will contain users with at least one active email, but won't have users which all emails are not active - in other words we filter out users without active emails.
```php
<?php
return $this->createQueryBuilder('user')
    ->select('user, email')
    ->leftJoin('user.emails', 'email')
    ->leftJoin('user.emails', 'activeEmail')
    ->where('activeEmail.status = :activeStatus')
    ->setParameters([
        'activeStatus' => User::STATUS_ACTIVE,
    ])
    ->getQuery()
    ->getResult()
;
// ---
foreach ($users as $user) {
    foreach ($user->getEmails() as $email) {
        if ($email->getStatus() === User::STATUS_ACTIVE) {
            // do something
        }
    }
}
```
3) select only those fields which are relevant for you within that specific case:
Entities won't be resolved, plain array would be returned, hence Doctrine's identity map is not built.
```php
$result = $this->createQueryBuilder('user')
    ->select('user.name AS userName, email.email AS userEmail, email.status AS emailStatus')
    ->leftJoin('user.emails', 'email')
    ->where('email.status = :activeStatus')
    ->setParameters([
        'activeStatus' => User::STATUS_ACTIVE,
    ])
    ->getQuery()
    ->getResult()
;

$return = [];
foreach ($result as $item) {
    $return[] = (new NormalizedReturnObject())
        ->setUserName($item['userName'])
        ->setUserEmail($item['userEmail'])
        ->setEmailStatus($item['emailStatus'])
    ;
}

return $return;
```


### Searching / paginating by date

When searching by date period, we take beginning inclusively and ending exclusively.

> **Why?** We do not need to add or remove one second from time periods, code gets much more readable.
> If both would be inclusive or exclusive, when iterating through periods, some results would be provided in both
> periods and some results on no periods.

On the other hand, in the user interface, if user wants to set dates inclusively,
frontend logic has to map between UI and backend/API convention (usually by adding a day to the end date).

## Entities

### Setters

All setters in entity returns `$this` for fluent interface.

### Setters vs adders

Setters always resets the previous value.
Adders leaves previous value and only adds provided to already existing collection.

### Relations in setters

In many-to-one relation, `many` side (the owning one) always fixes relations.
This is done in the `add*` method, adding reverse relation to `$this`.

In this case `set*` contains code to reset collection and `add*` every item in provided collection, so that
1) every item in collection would have relation fixed;
2) collection object would be the same as before `set*`.

```php
<?php
declare(strict_types=1);

class Tree
{
    private ArrayCollection $leaves;

    public function __construct()
    {
        $this->leaves = new ArrayCollection();
    }

    /**
     * @param Leaf[] $leaves we do not typecast to array or Collection as this can be any iterable
     * @return $this
     */
    public function setLeaves(array $leaves): self
    {
        $this->leaves->clear();
        foreach ($leaves as $leaf) {
            $this->addLeaf($leaf);
        }

        return $this;
    }

    public function addLeaf(Leaf $leaf): self
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
declare(strict_types=1);

class Entity
{
    public const TYPE_SIMPLE = 'simple';
    public const TYPE_COMPLICATED = 'complicated';

    private string $type;

    public function __construct()
    {
        $this->type = self::TYPE_SIMPLE;
    }
}
```

### ID

We do not provide `setId` method as we shouldn't use it - we either persist new entity
or take it from repository and update it.

### Creation date

We set `createdAt` property in constructor if we need creation date.
We can also set `createdAt` in manager if entity is always created in that one place (it should be).

> **Why not Doctrine extension?** This works faster and more reliable.
> Furthermore, even new not-yet-persisted entities has `createdAt` value, so we can assert that it’s always available.
> Also this makes unit testing easier as we do not need to mock extensions.

### Updated at

We do not put `updatedAt` to entities unless needed. Valid use-case is if we need to see when **any** of the fields
was changed, for example if synchronizing relational database via cron job into some other storage etc.

In any other use-case `updatedAt` does not give needed information.
Either it is not used at all, either it is used where it shouldn't.

For example, if we need to see when some state has changed, we need to put extra column or even one-to-many relation
to be sure that the date saved really represents that state change, not some other change in any other of entity
fields. Even if there is one (besides `id` and `updatedAt`) field in entity, extra one can be added later and this
would break the functionality (or naming).

## Doctrine

### Flush

#### Number of flushes

We always avoid more than one `flush` in one web request.

#### Where to flush

We use `flush` only in controllers, commands and workers.
These are the top input layer to the system, and is used usually only once per request life-cycle.
Services (and especially listeners) can be used in many different ways, so we cannot make assumption that a few of
them will not get called in the same request.

#### Multiple flushes

Services that do flush the database or use other services that flush the database (in any depth) should not live in
namespace `Service/`, but in separate namespaces such as `Processor/`.

```php
<?php
declare(strict_types=1);

// namespace Acme\FooBundle\Service; // Incorrect
namespace Acme\FooBundle\Processor; // Correct

use Doctrine\ORM\EntityManagerInterface;

class FooProcessor
{
    protected EntityManagerInterface $entityManager;
    protected RemoteServiceClient $remoteServiceClient;

    public function __construct(EntityManagerInterface $entityManager, RemoteServiceClient $remoteServiceClient)
    {
        $this->entityManager = $entityManager;
    }

    public function process(): void
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

When we use Doctrine `string` type, we try to figure out sensible maximum length for each case separately, we
shouldn't just use `255` as default one.

We should always validate or ensure in some other way that the length of the value fits to the maximum length
before storing it to the database.

> **Why?** While maximum length does not affect size in disk, it can affect performance. When INTERNAL TEMPORARY
tables are created (for joins, derived tables, counts, group by, order by etc), it's created in RAM only if intermediate
results are small enough. This size is calculated taking maximum length of fields, and not actually used space.
Thus unnecessary bigger maximum lengths can cause more I/O on the server and slower query times.

### Database naming

#### Underscored

We use underscored names for both table names and column names.

#### Plural

We use plural nouns as table names.

> **Why?** This makes SQL statements more natural language - we select some items from a collection.

#### Prefix

We add prefix to all databases, consisting of bundle name without vendor.
`AcmePageBundle:PageRoute` -> `page_page_routes`.

#### SQL keywords

We avoid SQL keywords in table and column names, but leave them as-is in PHP-side.

```php
<?php
declare(strict_types=1);

class Entity
{
    private string $key;
}
```

```xml
<field name="key" column="entity_key" type="string"/>
```

### Class-map

We avoid Doctrine class-map and extending entities.

> **Why?**
> 1) Extending comes with very bad performance (many relations are queried automatically even if they are
> never used afterwards), and many `instanceof` checks in the code;
> 2) this is not extendable, as base bundle must know about all others extending it’s entity;
> 3) we cannot change instance of Entity (change it's subclass) once it's created, which also makes things
> unnecessarily difficult.

### Extendability pattern instead of class-map

We use such a pattern:

-   Base entity has only the information, which is common to all entities.
Thus this information is used inside base bundle in some services.

-   Base entity has field `type`, `providerKey` or similar. It defines, which bundle/manager/provider created
this entity and knows more information.

-   There is Manager or similar service in base bundle, which provides methods to manage this entity
(and it’s subclasses). This service has method `addProvider` or similar, accepting `ProviderInterface` (or similar)

-   There is a compiler pass, which adds Providers (etc., implementing interface from base bundle)
to Manager via `add\*` method. We can always use the same class from `lib-dependency-injection` repository,
no need to create separate for each case, only if there is some custom logic.

-   Providers live in their bundles, optionally having additional information in some separate entities,
which possibly have relation to the base entity.

Example:

-   `PageBundle` has entity `Block`, which has `id` and `providerKey`.

-   `BlockManager` has method `renderBlock(Block $block)` and `addProvider(ProviderInterface $provider, $key)`.
It lives in `PageBundle`. Methods from `ProviderInterface` are used only in this manager,
(bad: `manager→getProvider(block)→render(block)`, good: `manager→render(block)`).
This allows to refactor some of the code more easily if needed.

-   There is compiler pass registered, which adds all tagged providers to manager via `addProvider` method.

-   `ProviderInterface` lives in `PageBundle` and has single method - `renderBlock(Block $block)`.

-   `TextBundle` has Entity `TextBlock`, which has fields `block` and `textId`.

-   `TextBlockProvider` implements `ProviderInterface` and lives in `TextBundle`.
`renderBlock` method searches for `TextBlock` related to the given block and renders text via `textId`.

Such structure let’s extend services without reverse dependencies.
As base bundle has no dependencies, it can be extracted to library and used in other projects.

Also the code is not changed in the base bundle - we can only test the new providers etc.,
no need to test if we didn’t brake anything in the base bundle.

### Saving Money instances

#### No getters for amount and currency

We only provide getter and setter for `Money` object, not for it's fields

#### No handling with events

We convert to and from `Money` object only in getters and setters, we do not use eventing system for that.
It could lead to unsaved fields, as Doctrine does not watch object property, it only watches amount and currency
properties, so in some cases there can be no event at all.
Also this would make understanding structure, debugging and testing harder.

#### We do not store Money objects themselves

As money object is 1) immutable 2) never compared by reference, we do not store the object itself in Doctrine-persisted Entities.
We store only it's internal fields and recreate it when needed. This makes the code simpler.

```php
<?php
declare(strict_types=1);

class Entity
{
    private ?string $priceAmount;
    private ?string $priceCurrency;

    public function __construct()
    {
        $this->priceAmount = null;
        $this->priceCurrency = null;
    }

    public function setPrice(?Money $price = null): self
    {
        if ($price === null) {
            $this->priceAmount = null;
            $this->priceCurrency = null;
        } else {
            $this->priceAmount = $price->getAmount();
            $this->priceCurrency = $price->getCurrency();
        }

        return $this;
    }

    public function getPrice(): ?Money
    {
        return $this->priceAmount !== null && $this->priceCurrency !== null
            ? new Money($this->priceAmount, $this->priceCurrency)
            : null;
    }
}
```

If Entity is not persisted by Doctrine, we just store Money object itself as usual.

#### We store amount as decimal

Inside Doctrine configuration:

```xml
<field name="amount" type="decimal" precision="16" scale="6" />
```

This allows to sum and perform other number-based operations inside the database.
Also, it auto-validates amount to be numeric (just in case we didn't handle it), and it takes less space,
so also performs better if indexing.

We store currency as a string.

### Doctrine migrations

For migrating database structure we always use Doctrine migrations.
There should be no changes inside generated migration if we run `doctrine:migrations:diff` after
`doctrine:migrations:migrate`.

We do not put custom `ALTER` or `CREATE` statements inside migration scripts ourselves - we modify `xml` file and let
Doctrine to generate migration file for us.

If needed, we can add additional `UPDATE` or/and `INSERT` statements for data migration corresponding
to our new database structure.

For new projects, we do not put `CREATE DATABASE` into migration scripts, but all tables should be created and
then altered by Doctrine migrations.

### ID column strategy

We use `IDENTITY` strategy for ID generation. This makes different structure in PostgreSQL - it works in same way
as in MySQL. If we use `AUTO`, sequence is used and ID is set as soon as we persist the entity.
As this behavior is not reproducible in MySQL database, we use `IDENTITY` to be compatible with more databases.
This also affects functional tests as sqlite does not have sequences, too.

Example configuration:

```xml
<id name="id" type="integer">
    <generator strategy="IDENTITY"/>
</id>
```

### Variable types for query parameters

Always make sure that parameters are of correct type when building query.

This is especially important when we pass an integer and column type is `VARCHAR`.
In this case database tries to cast column value of every row into an integer and compare to passed value - no indexes
get used in such cases. For example:

```sql
SELECT * FROM ext_log_entries e0_
WHERE e0_.object_id = 21590
      AND e0_.object_class = 'Acme\\UserBundle\\Entity\\User'
ORDER BY e0_.version DESC;
```

Correct usage:

```sql
SELECT * FROM ext_log_entries e0_
WHERE e0_.object_id = '21590'
      AND e0_.object_class = 'Acme\\UserBundle\\Entity\\User'
ORDER BY e0_.version DESC;
```

## Controllers

### As services

We use controllers as services. That is, every controller must be defined as a service.

If controller methods return Response objects (not for REST controllers), controller can extend base
Symfony Controller. In this case we add a call to setContainer in controller definition (in `services/controllers.xml`).
We do not use container in the controller’s code - we inject needed services via constructor.

### Static templates

If controller has no logic, we use `FrameworkBundle:Template:template` controller in routing and set `template`
argument to twig template name to render.

### Parameters passed to view

We pass original representation of variables to view, if view can itself transform the variables.

```php
<?php
declare(strict_types=1);

class Controller
{
    public function action(): Response
    {
        // ...
        return $this->render($template, array(
            'options' => array('a' => $a, 'b' => $b), // we do not use json_encode() here
        ));
    }
}
```

```twig
<div data-options="{{ options|json_encode }}"></div>
```

> **Why?** Controller just provides variables, it does not know (or shouldn’t know) how they will be used in template.
> Furthermore, template sometimes needs both encoded and original versions, so controller would duplicate some
> code in such case.


### Small controllers

We group actions into small controllers - we do not put more than 5-8 actions into one controller.

We try to make controller for each of resources, not just one for whole bundle.

It’s perfectly ok to have controllers containing only one action.

> **Why?** As we use constructor injection, big controllers have many dependencies, which are rarely used in such cases.
> This is bad for performance. Furthermore, more methods make code more difficult and dependencies not that clear.

### Class naming

Controller class name must always end with `Controller`.

## Events

### Available events

We put all available events in `final` class, containing only constants with available events.

### Event naming

We name events in past-tense verbs, prefixed by resource for which some action happened.

> **Why?** Events should be used only after something happened (exception: before something happening).
> Present tense verb would indicate that event should make something, which would be a misuse of event system.

We do not start constants with `ON_`.

> **Why?** `on*` indicates action performed when event is dispatched, not the event itself.

We can start constants with `BEFORE_` or `AFTER_` if it indicated event that is dispatched before or after some action.

We give constant the same value as the constant name, prefixed with bundle name.

> **Why?** For find-replace to find both constant and it’s value usage (in tags).
> Bundle prefix is needed to avoid collisions.


```php
<?php
declare(strict_types=1);

final class PageEvents
{
    public const PAGE_CREATED = 'evp_page.page_created';
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
declare(strict_types=1);

class PageCreatedEvent extends Event
{
    private Page $page;
}
```

Correct:

```php
<?php
declare(strict_types=1);

class PageEvent extends Event
{
    private Page $page;
}
```

Correct, because it has special case of additional property:

```php
<?php
declare(strict_types=1);

class PageChangedEvent extends Event
{
    private Page $page;
    private Page $previousPage;
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

We use custom namespaces for integrating with other bundles or when our service has some specific type,
is not main facade for that bundle.

```txt
Callback/CallbackValidator.php
Callback/CallbackProvider.php
DynamicAsset/AssetGroupProvider.php
PageBlock/MenuProvider.php
```

### Special directories

-   `Resources` - for resources
-   `Tests` - for tests. Test for `Vendor/SomeBundle/Namespace/Service` goes to
`Vendor/SomeBundle/Tests/Namespace/ServiceTest`

### Bundle namespace

We don't use `\Bundle\` sub-namespace after the vendor for new projects - we use `Vendor\SomeBundle` directly.

In existing projects we make folder structure the same as it's already used there.

### Example

```txt
Acme/PageBundle
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
    AcmePageBundle.php
```

## Twig

### Calling methods

We use either `.something` or `.getSomething()`, we do not use `.getSomething`.

We prefer `.something` for getters without arguments.

We prefer using static templates with JS framework and REST API over twig.


## Commands

### Naming

If command name consists of several words, we use dashes to separate them. For example, `some-namespace:do-something`

### Commands as services

We register commands as services with dependencies injected into them.

We use lazy loading by always providing attribute with command name in the tag ([see documentation](https://symfony.com/doc/current/console/lazy_commands.html)).

## Symfony version and new projects

### Version and structure

We use only stable releases for our projects.

We use only supported (at least for security fixes) versions of Symfony.

We prefer LTS version of Symfony framework as it does not require much maintenance on updating vendors.

We can use, if applicable, newer stable version of Symfony, but we must update it periodically (more often than LTS)
so that it would still be supported (even if we don't actively maintain that project, so choose carefully).

We prefer to use only 2 different major versions of Symfony for our projects.

> **Why not using 3 different major versions?** This would make quite hard to maintain several Symfony versions in
> our libraries and bundles.

### Configuration

We always use skeleton to bootstrap new projects as this allows to easily use any (configuration) fixes or improvements
that were made to each and every project during the years. It also includes all needed basic features that could
be depended on by our libraries or bundles which could be installed afterwards.

Keep in mind that there are different skeletons for WEB, REST API and processing nodes.

## REST in PHP

### REST controllers

For REST controllers, we use [`PayseraApiBundle`](https://github.com/paysera/lib-api-bundle) and normalizers.

Controller methods return entities or scalar variables.

Each method is configured with normalizer and denormalizer if needed.

We do not use request object from the controller.
We define normalizers to give only the needed information to the controller.
We can use plain normalizer or plain item normalizer if needed.

### Result providers

We use result provider to give `Result` entities from REST controller.

Response is automatically normalized using normalizer related to the classname of the returned object.
We could override it with additional annotation, if needed.

### Extending model

In server side we do not use subclasses and class-maps - we use services that give or process needed information.

On the client side, on the other hand, we have full model (via subclasses or just put into one class).

On the server side:

```php
<?php
declare(strict_types=1);

class MainEntity
{
    protected int $id;
    protected string $type;
}

class SubEntity
{
    protected int $id;
    protected MainEntity $mainEntity;
    protected string $subValue;
}

class AnotherSubEntity
{
    protected int $id;
    protected MainEntity $mainEntity;
    protected string $anotherSubValue;
}
```

On the client side:

```php
<?php
declare(strict_types=1);

class Entity
{
    protected int $id;
    protected string $type;
    protected string $subValue;
    protected string $anotherSubValue;
}
```

OR

```php
<?php
declare(strict_types=1);

class Entity
{
    protected int $id;
}
class SubEntity extends Entity
{
    protected string $subValue;
}
class AnotherSubEntity extends Entity
{
    protected string $anotherSubValue;
}
```

### Permissions

We always check permissions in REST controllers (access to resource) and/or listeners (access to API).

We check permissions using `SecurityContext` and writing custom voters, which implement `VoterInterface`.

### Pagination

We prefer cursor-based pagination.


## Migrating from legacy code

This describes conventions used to migrate from legacy code
(which was written before integrating Symfony framework into the code base).

Assumption is made that Symfony is already integrated and can be fully used.

## New features

All new features are implemented as new Symfony bundles and/or separate components.

New code always follow all of the current conventions.

## Managing dependencies

To continuously move away from legacy code, dependencies must be taken into account. If new code would directly
depend on the legacy one, it would be hard to keep track where one ends and another starts. Also it would be hard
to refactor some part of legacy code without affecting too much of already written (and still considered new) code.

This is why all the code should be split into two parts:
- *current code*. This parts adheres to the conventions and depends only on *current code* or vendors;
- *legacy code*. This is both the legacy code itself and the code for adapting legacy code for newer functionality.

### No hardcoded dependencies on legacy code

When writing new code, it must have clear dependencies.
It can directly depend only on *current code* (other components, vendors or other bundles).

Some bundles and components can belong to *legacy code*. This code **can** depend on legacy code or anything else.
We try to keep it as thin as possible - only for adapting the legacy code or integrating it with Symfony.

*Directly depend* means that there is no dependencies in the code itself, not that in run-time new code could not call
the legacy one (if this would be available, we wouldn't even need it at all).

### Interfaces

If bundle need information about something available only in *legacy code*, bundle defines an interface without any
implementation and other services depend on this interface.

This interface can also be defined in any other bundle that this bundle depends on (for example, UserBundle).

### Configuration

We provide semantic configuration for this bundle via `Configuration` and `Extension` classes.

In the configuration we take service ID, which implements our needed interface.

We add alias in Extension class to provided service ID.

### Example

Imagine that we have some legacy code:
```php
<?php

function get_user_data($user_id) {
    return get_db()->query('SELECT * FROM users WHERE id = ?', $user_id);
}
```

We write some functionality that needs to get user's email and name. We could use legacy code directly like in the
following example, but we shouldn't:

```php
<?php
declare(strict_types=1);

namespace Acme\NewsletterBundle\Service;

class NewsletterSender
{
    public function sendNewsletter(Newsletter $newsletter): void
    {
        $userId = $newsletter->getUserId();
        $userData = get_user_data($userId); // don't do this! ;)
        $userEmail = $userData['email'];
        $userName = $userData['name'];
        // ...
    }
}
```

Instead, we create an interface that provides our needed functionality:

```php
<?php

namespace Acme\NewsletterBundle\Service;

use Acme\NewsletterBundle\Entity\UserData;

interface UserDataProviderInterface
{
    public function getUserData(int $userId): UserData;
}
```

```php
<?php
declare(strict_types=1);

namespace Acme\NewsletterBundle\Entity;

class UserData
{
    private string $email;
    private string $name;

    public function getEmail(): string
    {
        return $this->email;
    }

    public function setEmail(string $email): self
    {
        $this->email = $email;

        return $this;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setName(string $name): self
    {
        $this->name = $name;
        return $this;
    }
}
```

And we refactor our service:

```php
<?php
declare(strict_types=1);

namespace Acme\NewsletterBundle\Service;

class NewsletterSender
{
    private UserDataProviderInterface $userDataProvider;

    public function __construct(UserDataProviderInterface $userDataProvider)
    {
        $this->userDataProvider = $userDataProvider;
    }

    public function sendNewsletter(Newsletter $newsletter): void
    {
        $userData = $this->userDataProvider->getUserData($newsletter->getUserId());
        $userEmail = $userData->getEmail();
        $userName = $userData->getName();
        // ...
    }
}
```

This allows to have new code that has no dependencies on the legacy one.

What we need to do next is implement the interface. To still keep *legacy code* and *current code* separate,
we do that in another bundle.


```php
<?php
declare(strict_types=1);

namespace Acme\IntegrationBundle\Service;

use Acme\NewsletterBundle\Entity\UserData;
use Acme\NewsletterBundle\Service\UserDataProviderInterface;

class UserDataProvider implements UserDataProviderInterface
{
    public function getUserData(int $userId): UserData
    {
        $userData = get_user_data($userId);
        if ($userData === false) {
            throw new \Exception('User not found');  // we could create separate exception for this
        }
        return (new UserData())
            ->setEmail($userData['email'])
            ->setName($userData['name'])
        ;
    }
}
```

We configure the service as usual (inside `IntegrationBundle` in this case):
```xml
<service id="integration.user_data_provider"
         class="Acme\IntegrationBundle\Service\UserDataProvider"/>
```

For our bundle to get the service from outside, we modify `Configuration` and `Extension` classes and configure
the service in `config.yml`:

```php
<?php
declare(strict_types=1);

namespace Acme\NewsletterBundle\DependencyInjection;

// ...

class Configuration implements ConfigurationInterface
{
    public function getConfigTreeBuilder(): TreeBuilder
    {
        $treeBuilder = new TreeBuilder();
        $rootNode = $treeBuilder->root('acme_newsletter');
        $children = $rootNode->children();
        $children->scalarNode('user_data_provider')->isRequired();
        return $treeBuilder;
    }
}
```

```php
<?php
declare(strict_types=1);

namespace Acme\NewsletterBundle\DependencyInjection;

// ...

class AcmeNewsletterExtension extends Extension
{
    public function load(array $configs, ContainerBuilder $container): void
    {
        $configuration = new Configuration();
        $config = $this->processConfiguration($configuration, $configs);

        $loader = new XmlFileLoader($container, new FileLocator(__DIR__ . '/../Resources/config'));
        $loader->load('services.xml');

        $container->setAlias('acme_newsletter.user_data_provider', $config['user_data_provider']);
    }
}
```

```yaml
acme_newsletter:
    user_data_provider: integration.user_data_provider
```

Now we can use our service inside our bundle:

```xml
<service id="acme_newsletter.newsletter_sender"
         class="Acme\NewsletterBundle\Service\NewsletterSender">
    <argument type="service" id="acme_newsletter.user_data_provider"/>
</service>
```

## Database migrations

Even for database tables that are not managed by Doctrine, we use Doctrine migrations for migrating their structure.
This enables clear process for upgrading the database structure and makes it reproducible in all environments.




# Composer Conventions

## Semantic versioning

We always use semantic versioning on library repositories.

> **Why?** When backward incompatible change is made in library, we have two options:
> 1) update all client side projects with new library version;
> 2) every time when we update library check for previous backward incompatible changes,
> not related to currently added feature.
>
> As it happens, sometimes none of these 2 takes place.

## Changelog

For libraries where semantic vensioning is used, we maintain the [`CHANGELOG.md` file](https://keepachangelog.com/en/1.0.0/).

If the file is missing, we create it and port changes from other sources, like `UPGRADE.md` (deleting that file afterwards).

Any commit must also have a change in `CHANGELOG.md` file with described changes.

## Releases

We tag each and every commit (except instant bug-fixes or commits before separate merge commits).

1. Right before tagging, we make sure we're on master branch and always run `git tag --sort=v:refname` to see current
    tags.
2. Before tagging, we see available branches on repository as version can be created by branch alias, not only by tags.
3. We bump MAJOR, MINOR or PATCH version by one from latest version available
    (or parent commit if there are few active branches).

    3.1. If we make backward incompatible change, we bump the MAJOR version.

    3.2. If we add new feature (any new arguments, method calls, classes etc.), we bump MINOR version.

    3.3. If we do not change API of library in any way, just change the internals (usually bug-fixes),
         we bump PATCH version.

We use annotated tags so that time and author would be present. Example command, `1.2.3` taken as example tag:

```
git tag -a -m "" 1.2.3
```

`-m ""` sets message to the tag, but as all commits are already with messages, we can leave it empty.

After tagging we need to call `git push --tags` - this pushes our tags to origin. We only do this after we've
landed our changes.

## Reviewing tagging policy

The tagging policy is always visible in `CHANGELOG.md` file - we don't use `Unreleased` block for internal libraries
that are updated for some concrete purpose (to be updated right afterwards in another project).

It's important to keep it updated and in-sync if any other changes are made in master after our diff - one should
be careful when rebasing this file.

## Initial library development

Until library is stable, versions can change quite often. In this case we still use tags and semantic versioning,
but use `0` as MAJOR version, which allows any backward incompatible change be made in any MINOR version bump.

If API of library is already quite stable, we use `1` as a MAJOR version initially.

## Constraints on library versions

We use as generic constraints as possible for required libraries. This avoids updating some library graph just because
of some conservative constraint, and not because it is really needed.

For example, we have `lib-a@1.0.0` and `lib-b@1.0.0`, which depends on `lib-a: ^1.0`.
If we roll out MAJOR release for `lib-a` (aka `lib-a@2.0.0`), at some time we need to modify constraint on `lib-b`:

1. If `lib-b` works with both `lib-a@1.0.0` and `lib-a@2.0.0` (change did not affect this library),
we use `lib-a: >=1.0.0,<3.0`. This allows to use both `1.0` and `2.0` with our new version of `lib-b`.
2. If `lib-b` works only with `lib-a@2.0.0` and not with `lib-a@1.0.0`,
we modify constraint as usual to `lib-a: ^2.0.0`.
3. If `lib-b` does not work with `lib-a@2.0.0`, we can either leave constraint to `^1.0`
or update code and use (1) or (2) depending on compatibility with `1.0` version.
Latter is recommended, as this allows to use newest possible versions (sooner or later we will need to update them).

Version in `lib-b` in any of these cases is not required to make a MAJOR bump - if API of `lib-b` is left the same,
we can make PATCH update. We can even entirely drop out `lib-a` and use some alternative library, as long as API
leaves the same.

This means that if `app-x` (or any other library) uses functions or classes from `lib-a`,
it must require it in `composer.json`.
If it just depends on `lib-b`, it does not care about `lib-a` versions or even if it is installed at all.

## Constraints on vendors

If we require vendor library, we use constraint depending on versioning strategy of that library.
If it states, that it follows semantic versioning (and we believe it does),
we use something like `^1.2.3` - this is added by default if we run `composer require vendor/library`.

If it has some other strategy, we must correct the constraint so that we would not get unexpected backward incompatible
changes. For example, Symfony components did break compatibility on minor releases (up to 2.3 version).
In that case, we should have not used `^` in constraint.

Basically, we must always assume that `composer update` can be run at any time and everything should still
work as expected.

Due to the same reason, we never require `dev-master`.
If vendor library has no versions defined, we require specific commit:

```
"vendor/package": "dev-master#8e45af93e4dc39c22effdc4cd6e5529e25c31735"
```

## Library version incrementing
As long as public API does not change, but dependency of library is not backward compatible anymore its still minor change

Example:
```
//Was:
//...
"require": {
    "php": "^7.4",
},
"lib-version": "1.2.3"
//...

//Becomes:
//...
"require": {
    "php": "^8.2",
},
"lib-version": "1.2.4"
//...

```

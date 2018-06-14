This page describes conventions related directly with Symfony framework and related components (Doctrine, Twig).

It also includes some closely related conventions that can be used independently (for example in libraries).

Table Of Contents
=====

1. [Configuration](#configuration)

    1.1. [Configuration inside bundles](#configuration-inside-bundles)
    
    1.2. [Configuration in app directory](#configuration-in-app-directory)
    
    1.3. [Parameters dist files](#parameters-dist-files)
    
    1.3.1. [File naming](#file-naming)
    
    1.3.2. [Parameter naming](#parameter-naming)
    
    1.3.3. [Contents](#contents)
    
    1.4. [Files](#files)
    
    1.4.1. [Imports](#imports)
    
    1.4.2. [Directories](#directories)
    
    1.5. [Naming](#naming)
    
    1.6. [Services](#services)
    
    1.6.1. [Class names](#class-names)
    
    1.6.2. [Factory services](#factory-services)
    
    1.6.3. [ID as FQCN (class name)](#id-as-fqcn-(class-name))
    
    1.6.4. [Autoconfiguration and autowiring](#autoconfiguration-and-autowiring)
    
    1.7. [Routing](#routing)
    
    1.7.1. [Route prefix](#route-prefix)
    
    1.7.2. [Route naming](#route-naming)
    
    1.8. [Production configuration](#production-configuration)
    
    1.9. [Always full service IDs](#always-full-service-ids)
    
2. [Repositories](#repositories)

    2.1. [Injection](#injection)
    
    2.2. [Configuration](#configuration)
    
    2.3. [Finding by ID](#finding-by-id)
    
    2.4. [Queries](#queries)
    
    2.5. [Return types](#return-types)
    
    2.5.1. [Basic usage](#basic-usage)
    
    2.5.2. [Advanced usage](#advanced-usage)
    
    2.6. [Method naming](#method-naming)
    
    2.7. [Only custom methods from outside of repository](#only-custom-methods-from-outside-of-repository)
    
    2.8. [Repositories from other bundles](#repositories-from-other-bundles)
    
    2.9. [Prefetching and filtering at the same time](#prefetching-and-filtering-at-the-same-time)
    
    2.10. [Searching / paginating by date](#searching-/-paginating-by-date)
    
3. [Entities](#entities)

    3.1. [Setters](#setters)
    
    3.2. [Setters vs adders](#setters-vs-adders)
    
    3.3. [Relations in setters](#relations-in-setters)
    
    3.4. [States, types etc.](#states,-types-etc.)
    
    3.5. [ID](#iD)
    
    3.6. [Creation date](#creation-date)
    
    3.7. [Updated at](#updated-at)
    
4. [Doctrine](#doctrine)

    4.1. [Flush](#flush)
    
    4.1.1. [Number of flushes](#number-of-flushes)
    
    4.1.2. [Where to flush](#where-to-flush)
    
    4.1.3. [Multiple flushes](#multiple-flushes)
    
    4.2. [Database types and definitions](#database-types-and-definitions)
    
    4.3. [Database naming](#database-naming)
    
    4.3.1. [Underscored](#underscored)
    
    4.3.2. [Plural](#plural)
    
    4.3.3. [Prefix](#prefix)
    
    4.3.4. [SQL keywords](#sql-keywords)
    
    4.4. [Class-map](#class-map)
    
    4.5. [Extendability pattern instead of class-map](#extendability-pattern-instead-of-class-map)
    
    4.6. [Saving Money instances](#saving-Money-instances)
    
    4.6.1. [No getters for amount and currency](#no-getters-for-amount-and-currency)
    
    4.6.2. [No handling with events](#no-handling-with-events)
    
    4.6.3. [We do not store Money objects themselves](#we-do-not-store-money-objects-themselves)
    
    4.6.4. [We store amount as decimal](#we-store-amount-as-decimal)
    
    4.7. [Doctrine migrations](#doctrine-migrations)
    
    4.8. [ID column strategy](#id-column-strategy)
    
    4.9. [Variable types for query parameters](#variable-types-for-query-parameters)
    
5. [Controllers](#controllers)

    5.1. [As services](#as-services)
    
    5.2. [Static templates](#static-templates)
    
    5.3. [Parameters passed to view](#parameters-passed-to-view)
    
    5.4. [Small controllers](#small-controllers)
    
    5.5. [Class naming](#class-naming)
    
6. [Events](#events)

    6.1. [Available events](#available-events)
    
    6.2. [Event naming](#event-naming)
    
    6.3. [Listener naming](#listener-naming)
    
    6.4. [Event class naming](#event-class-naming)
    
7. [Directory structure](#directory-structure)

    7.1. [Root bundle directory](#root-bundle-directory)
    
    7.2. [Basic directories for code](#basic-directories-for-code)
    
    7.3. [Integration with other bundles](#integration-with-other-bundles)
    
    7.4. [Special directories](#special-directories)
    
    7.5. [Example](#example)
    
8. [Twig](#twig)

    8.1. [Calling methods](#calling-methods)
    
9. [Commands](#commands)

    9.1. [Naming](#naming)
    
    9.2. [Dependencies](#dependencies)
    
10. [Symfony version and new projects](#symfony-version-and-new-projects)

    10.1. [Version and structure](#version-and-structure)
    
    10.2. [Configuration](#configuration)
    


Configuration
=====

## Configuration inside bundles

We use xml configuration inside bundles.

> **Why not yaml?** We use xml, because it is explicit and has schema definition - many mistakes can be avoided. yaml has many different variations and does not include validation nor autocomplete.


> **Why not annotations?** We don’t use annotations, as we want to leave configuration and logic separate. Moreover, class and object/service are not the same, this makes it difficult to configure few services for one class.

## Configuration in app directory

We use yaml configuration inside app directory for semantical configuration files.

> **Why?** Configuration is semantical here and thus not very complex. Most of examples use only this format, it’s default for Symfony standard.


We still use xml for service definitions inside app directory.

## Parameters dist files

### File naming

We use `{origName}.dist.{origExt}` name for distribution versions of parameters.

> **Why?** Original extension is left the same so we can use all IDE features. It is configurable (or not important) so we **can** do that.


### Parameter naming

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

### Contents

We use parameters for development environment.

> **Why?** We cannot use production as this would be security issue. All developers can add default values and change them only if needed.


We use value `pass` for default passwords in development environment.

We use default domains in parameters - if everywhere configured the same, no changes in parameters would be needed.

We remove `app-` prefix from repository name and add `.dev.docker` as top-level domain. For example `app-wallet-api` ⇒ `http://wallet-api.dev.docker/`

## Files

### Imports

When configuring services, we split definitions into separate files and import to main `services.xml` or `routing.xml` files. For example:

```txt
config/services/controllers.xml
config/services/forms.xml
config/services/repositories.xml
config/services/normalizers.xml
```

We put routing prefix into `<import/>` directive.

### Directories

We put files to subdirectory - `services/`, `routing/` etc.

## Naming

Service ID always begins with bundle identifier, like `evp_contextual_information`, followed by dot.

If type of service is repository, controller, listener or normalizer, these are following parts:

-   type of service, like `manager`, `repository`, `controller`
-   name of the service, excluding the type.

If service is a manager, registry or some other unique service for that bundle, we use it’s identifier directly without the type.

Valid service names: `evp_page.page_manager`, `evp_page.repository.page`

## Services

### Class names

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


### Factory services

We use new syntax for defining factory services:

```xml
<service id="newsletter_manager" class="NewsletterManager">
    <factory class="NewsletterManagerFactory" method="createNewsletterManager"/>
</service>
```

### ID as FQCN (class name)

We do not use service IDs as class names by default.

> **Why?** As this allows easier configuration in some cases and avoids naming concerns for our services, it could be hard to refactor code when another service for the same class is added to our project. This would make one of the services "default" one, which would make debugging harder and understanding of internal working more difficult.

> For example, if we have `BalanceProvider` and we would add another service for providing balance with debts included (using same class with different configuration), this would require to go over every usage of the first service and changing it's ID.

We can use class name as service ID (or alias it) when we are providing open-source bundle. This allows others to use autowiring features if needed, even if we don't use them. We should only use this for public services - ones that are to be used outside of our bundle.

### Autoconfiguration and autowiring

We don't use autowiring and autoconfiguration features of Dependency Injection component.

> **Why?** First of all, we don't use FQCN as service IDs, so this would not work. Secondly, it might be quicker at the beginning to use it, but to understand the code, we would need to guess where the service really comes from. If we would need to refactor something, it might require to write DI configuration for already (automatically) defined services, too.

## Routing

### Route prefix

We always use bundle name as route prefix, just like in services. We leave out vendor prefix.

Exception: we drop out `-common` suffix, if one exists. Routes should not clash with corresponding bundle without `-common`.

### Route naming

We try to use names to identify the action taken by route, not methods to take it.

For example, `create_page` instead of `page_post`.

## Production configuration

We do not put specific configuration in bundles - we provide semantic configuration for those and define them inside `app/config.yml`.

We do not put secret parts of production configuration anywhere in repository - no private key files, common secret strings, passwords etc. They must be ignored by git and provided in `parameters.yml`, shared directory or in some other way.

## Always full service IDs

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

Repositories
=====

## Injection

We inject repository objects inside the services via constructor.

Thus, we do not use `EntityManager::getRepository` in controllers or services.

## Configuration

We configure repository classes with `lazy=true`.

## Finding by ID

For finding entity by ID we always use `find` method, not `findOneById` etc.

Exception: If we want to preload some related entities.

Another exception: If we override `findOneById` to contain call to `find`. This could be used to provide PhpDoc for IDE to guess the type returned.

## Queries

We use query builder and queries inside repositories only - we don’t build queries inside controllers or other services.

## Return types

### Basic usage

Repository methods starting with `find` returns instance(-s) of the class, that this repository is related to, or scalar values.

### Advanced usage

If repository must use entity from other bundle, that this bundle does not depend upon, we make separate service in Repository namespace and write query in this service. For example:

-   PosBundle has entity Spot. This bundle has extension points and does not depend on WorapayBundle.

-   WorapayBundle has entity WorapaySpot. This entity has relation one-to-one with the Spot entity with no reverse relation (Spot does not know about WorapaySpot).

-   We need method findSpotsByWorapayIdentifier(\$identifier). It returns Spot[], so should be in SpotRepository. But as it’s in SpotBundle, which does not depend on WorapayBundle, we make new service WorapayBundle/Repository/SpotRepository, inject base SpotRepository and build corresponding query there. In this case, we would need 2 from clauses as join would not work (no relation from Spot to WorapaySpot).

## Method naming

We use `findOneBy*` to find one object and `findBy*` to find list of objects.

We use `getQueryBuilderFor*` to get query builders (for example for pagination), `findCountBy*` etc. to get scalar results.

## Only custom methods from outside of repository

We do not use `findOneBy` or `findBy` methods from outside of repository. We create method inside repository for that use-case - in that method, we can call `$this->findBy(...)` with given parameters.

> **Why?** This allows to add more constraints in one single place, for example after adding `deleted` column we can update all queries inside repository to filter those records out by default. Also we can see all possible queries in one place - this allows to easier review which indexes should be defined and which are unused.


## Repositories from other bundles

We try to inject repositories only into services that are in the same bundle the repository is.

If we need to get information from another bundle, we try to use services (which, in that bundle, can inject that repository).

> **Why?** This creates proxy service for getting needed information. Even if at first it would only pass method call to repository itself, if something changes we can refactor it easier - we can inject other services, handle filtering of arguments or results etc. This helps a lot if method in repository is used from many many places inside whole project and we need to change some logic with other injected service.


## Prefetching and filtering at the same time

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


## Searching / paginating by date

When searching by date period, we take beginning inclusively and ending exclusively.

> **Why?** We do not need to add or remove one second from time periods, code gets much more readable. If both would be inclusive or exclusive, when iterating through periods, some results would be provided in both periods and some results on no periods.


On the other hand, in the user interface, if user wants to set dates inclusively, frontend logic has to map between UI and backend/API convention.


Entities
=====

## Setters

All setters in entity returns `$this` for fluent interface.

## Setters vs adders

Setters always resets the previous value. Adders leaves previous value and only adds provided to already existing collection.

## Relations in setters

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

## States, types etc.

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

## ID

We do not provide `setId` method as we must not use it - we either persist new entity or take it from repository and update it.

## Creation date

We set `createdAt` property in constructor if we need creation date. We can also set `createdAt` in manager if entity is always created in that one place (it should be).

> **Why not doctrine extension?** This works faster and more reliable. Furthermore, even new not-yet-persisted entities has `createdAt` value, so we can assert that it’s always available. Also this makes unit testing easier as we do not need to mock extensions.


## Updated at

We do not put `updatedAt` to entities unless needed. Valid use-case is if we need to see when **any** of the fields was changed, for example if synchronizing relational database via cron job into some other storage etc.

In any other use-case `updatedAt` does not give needed information. Either it is not used at all, either it is used where it shouldn't. For example, if we need to see when some state has changed, we need to put extra column or even one-to-many relation to be sure that the date saved really represents that state change, not some other change in any other of entity fields. Even if there is one (besides `id` and `updatedAt`) field in entity, extra one can be added later and this would break the functionality (or naming).

Doctrine
=====

## Flush

### Number of flushes

We always avoid more than one `flush` in one process.

### Where to flush

We use `flush` only in controllers, commands and workers. These are the top input layer to the system, and is used usually only once per request life-cycle. Services (and especially listeners) can be used in many different ways, so we cannot make assumption that a few of them will not get called in the same request.

### Multiple flushes

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

## Database types and definitions

When we use Doctrine `string` type, we use length of `255` or bigger if needed.

> **Why?** There is no real advantage in using less than `255`. It does not add performance, does not save space and can only cause difficulties if some smaller defined max length is reached.


## Database naming

### Underscored

We use underscored names for both table names and column names.

### Plural

We use plural nouns as table names.

> **Why?** This makes SQL statements more natural language - we select some items from a collection.


### Prefix

We add prefix to all databases, consisting of bundle name without vendor. `EvpPageBundle:PageRoute` -> `page_page_routes`.

### SQL keywords

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

## Class-map

We avoid Doctrine class-map and extending entities.

> **Why?** Extending comes with very bad performance (many relations are queried automatically even if they are never used afterwards), and many intanceof checks in the code. Also, this is not extendable, as base bundle must know about all others extending it’s entity. Moreover, we cannot change instance of Entity (change it's subclass) once it's created, which also makes things unnecessarily difficult.


## Extendability pattern instead of class-map

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

## Saving Money instances

### No getters for amount and currency

We only provide getter and setter for Money object, not for it's fields

### No handling with events

We convert to and from Money object only in getters and setters, we do not use eventing system for that. It could lead to unsaved fields, as doctrine does not watch object property, it only watches amount and currency properties, so in some cases there can be no event at all. Also this would make understanding structure, debugging and testing harder.

### We do not store Money objects themselves

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

### We store amount as decimal

Inside doctrine configuration:

```xml
<field name="amount" type="decimal" precision="16" scale="6" />
```

This allows to sum and perform other number-based operations inside the database. Also, it auto-validates amount to be numeric (just in case we didn't handle it), and it takes less space, so also performs better if indexing.

We store currency as a string.

## Doctrine migrations

For migrating database structure we always use doctrine migrations. There should be no changes inside generated migration if we run `doctrine:migrations:diff` after `doctrine:migrations:migrate`. We do not put custom `ALTER` or `CREATE` statements inside migration scripts ourselves - we modify `xml` file and let doctrine to generate migration file for us.

If needed, we can add additional `UPDATE` or/and `INSERT` statements for data migration corresponding to our new database structure.

For new projects, we do not put `CREATE DATABASE` into migration scripts, but all tables should be created and then altered by doctrine migrations.

## ID column strategy

We use `IDENTITY` strategy for ID generation. This makes different structure in PostgreSQL - it works in same way as in MySQL. If we use `AUTO`, sequence is used and ID is set as soon as we persist the entity. As this behavior is not reproducible in MySQL database, we use `IDENTITY` to be compatible with more databases. This also affects functional tests as sqlite does not have sequences, too.

Example configuration:

```xml
<id name="id" type="integer">
    <generator strategy="IDENTITY"/>
</id>
```

## Variable types for query parameters

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

Controllers
=====

## As services

We use controllers as a services. That is, every controller must be defined as a service.

If controller methods returns Response objects (that is this is not REST controller), controller can extend base Symfony Controller. In this case we add a call to setContainer in controller definition (services/controllers.xml). We do not use container in the controller’s code - we inject needed services via constructor.

## Static templates

If controller has no logic, we use `FrameworkBundle:Template:template` controller in routing and set `template` argument to twig template name to render.

## Parameters passed to view

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


## Small controllers

We group actions into small controllers - we do not put more than 5-8 actions into one controller.

We try to make controller for each of resources, not just one for whole bundle.

It’s perfectly ok to have controllers containing only one action (such controllers can be easily configured and reused).

> **Why?** As we use constructor injection, big controllers have many dependencies, which are rarely used in such cases. This is bad for performance.


## Class naming

Controller class name must always end with `Controller`.

Events
=====

## Available events

We put all available events in `final` class, containing only constants with available events.

## Event naming

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

## Listener naming

We use `on*`, `before*` or `after*` method names for event listeners.

We can use the same method for several events if logic is the same.

## Event class naming

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

Directory structure
=====

## Root bundle directory

We put following classes to root bundle directory:

-   Bundle definition
-   Final Events classes for events, raised by this bundle

## Basic directories for code

-   `Command` - for commands
-   `Controller` - for controllers
-   `Entity` - for entities
-   `Event` - for events
-   `Listener` - for listeners
-   `Normalizer` - for normalizers
-   `Repository` - for repositories
-   `Service` - for main services of this bundle
-   `Worker` - for workers (handles jobs from RabbitMQ)

## Integration with other bundles

We use custom namespaces for integrating with other bundles or when our service has some specific type, is not main facade for that bundle.

```txt
Callback/CallbackValidator.php
Callback/CallbackProvider.php
DynamicAsset/AssetGroupProvider.php
PageBlock/MenuProvider.php
```

## Special directories

-   `Resources` - for resources
-   `Tests` - for tests. Test for `Vendor/Bundle/SomeBundle/Namespace/Service` goes to `Vendor/Bundle/SomeBundle/Tests/Namespace/ServiceTest`

## Example

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

Twig
====

## Calling methods

We use either `.something` or `.getSomething()`, we do not use `.getSomething`.

We prefer `.something` for getters without arguments.

We prefer using static templates with JS framework and REST API over twig.

Commands
====

## Naming

If command name consists of several words, we use dashes to separate them. For example, `some-namespace:do-something`

## Dependencies

If project uses Sf 2.4 and above, we register commands as services with dependencies injected into them.

For Sf 2.3 we take dependencies from service container.

Symfony version and new projects
====

## Version and structure

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


## Configuration

We always use skeleton to bootstrap new projects as this allows to easily use any (configuration) fixes or improvements that were made to each and every project during the years. It also includes all needed basic features that could be depended on by our libraries or bundles which could be installed afterwards.

Keep in mind that there are different skeletons for WEB, REST API and processing nodes.




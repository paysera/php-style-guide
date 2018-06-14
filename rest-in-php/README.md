REST in PHP
=====

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


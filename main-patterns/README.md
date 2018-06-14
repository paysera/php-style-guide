# Thin model

We use Plain Value Objects (we name them Entities) for moving information/data around functionality.

We do not put any logic into those objects - all logic is handled outside, in services.

This allows to change and configure behaviour in different context.

# Services without run-time state

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


# Composition over inheritance

We always try to use composition instead of inheritance.

If we want to make some abstract method - we inject interface into constructor with that method.

This helps to follow single responsibility principle. Also this often allows to configure object by injecting different functionality without explosion of classes. If we have 2 abstract methods and 2 possible logic for each of them, we need 4 classes with 8 methods total and duplicated code. If we inject services, we need only 4 classes with 1 method each with no duplicated code.

Also, this allows to test code much easier, as we can test each functionality separately.

# Services (objects) over classes, configuration over run-time parameters

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

## Constant usage

We do not store configuration in constants.

> **Why?** As configuration can change, it is not constant. End result: `const SOFT = 'soft'; const HARD = 'soft';`

# Small, understandable methods

We try to write code that is self-explanatory. This implies writing small methods - we should understand what the method does by looking at it's code. We also try to name methods by their meaning, which also helps understanding the code.

Dependencies
====

We always take into account component or bundle dependencies.

## No unnecessary dependencies

No unnecessary dependencies between components or bundles must be created.

If there is some abstract functionality, it must not depend upon it’s implementations.

## No circular dependencies

No circular dependencies between components or bundles must be created.

Services
=====

## Service creation

Services can be created only by factory classes or dependency injection container. We do not create services in other services not dedicated to do that (for example, in constructor).

## Changing entity state

### Use of managers

We make changes to entity state in managers or some similar designated services.

Manager can make the following actions:

-   check initial entity state before changing it, validate it
-   change the state
-   dispatch event
-   make some additional actions

If state is changed in a controller or some other place, duplicated code can occur when functionality is required in another place.

### Methods for changing state

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

## Data types

We use objects to represent data when possible. For example, if data can be represented by both scalar value and an object (like date), we use object. When several variables belong to single item, we use an object representing them and do not pass or operate on these separately (like money object versus amount and currency as separate variables).

We always try to use single representation of that data item - we avoid having multiple classes which represent same object in the system (for example Transfer as DTO in common library and as an entity - these could both co-exist, but we try to have an Entity as soon as possible in the system).

We normalize data as soon as possible in the system (input level) and denormalize it as later as possible (output level). This means that in service level, where our business logic resides, we always operate on these objects and not their concrete representations.

### Identifier usage

We operate with objects inside services, not their identifiers. Objects must be resolved by their identifiers in controller, normalizer or similar input layer. In business objects (entities or simple DTOs, like `Filter`) we already have other objects, taken from database. If we do not find object by identifier, we throw exception and return `404` as a response (or specific error code with `400`).

### Date and time

We use integer timestamp to represent date with time. When creating `\DateTime`, we must not use constructor with `@` as it ignores the time zone of the server.

For example:

```php
<?php
$createdAt = new \DateTime();
$createdAt->setTimestamp($data['created_at']);
$entity->setCreatedAt($createdAt);
```

### Money

Always amount and currency, never only amount.

## One-to-many Relation in Services

### Structure

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

### Tags

We use dependency injection container tags to add all those services to the manager.

## Event dispatcher

We use events to notify about some result or to optionally change behaviour before making some action.

We do not use events to actually make some action happen, which is mandatory for the place which dispatches the event.

We should not make assumptions about listeners of some event. If we require a result from event listeners, this indicates that we should refactor functionality and use interfaces with tags or some similar solution.


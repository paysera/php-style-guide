Smartweb-Symfony integration
=====

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

Bank integration
=====

## Bank naming

Bank keys should begin with country code (when available), followed by underscore and bank name, eg.: "ee\_krediidi" or just "webmoney".

Bank keys in app-mokejimai and in app-evpbank should be the same.

## Accounting template naming

Accounting template names are generated during payment with AccountingTemplateNameResolver::getAddPaymentBillTemplateName method. Use this method to verify and/or check if supplied accounting template name is correct, generated accounting template name should be available in database - "m\_bill\_templates" table, "TemplateTitle" and "Title" columns. A good practise is to append new data to AccountingTemplateNameResolverTest::nameResolutionDataProvider and use new test case to verify that new template name exists in database.

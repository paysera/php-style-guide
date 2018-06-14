# Deprecated conventions

Some convetions that are used thoughout the systems, either explicitly defined in conventions, either just used from practive, are deprecated.

This page lists those conventions and reasons why we should avoid them.

# REST proxy controllers

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

# Splitting controllers by use-case (administration / frontend)

We should not make `AdministrationApiController`, ever.

This violates other conventions, as it's named after use-case, not functionality. In fact, in cases where it was named like that, one of the following happens:

- at least one endpoint gets used not only by administrators;
- code gets duplicated as the same endpoint is repeated in another controller;
- code is refactored to avoid this pattern (of course, this is preferred).

Additionally, it makes routing and prefixes complicated, duplicated, sometimes even conflicting.

So, we make single multi-purpose backend-API. We can split it into several controllers, but it should be done by functionality itself, not their current usage.

As with every endpoint, we configure authorization (permissions needed to perform that action). Administrators just get permission to perform that action.

If needed, partners, other systems or even user herself could also reuse these endpoints.

## URL path prefixes

Some deprecated rules:

- We use `/{_locale}/bundle-name/rest` prefix for all front-end REST routing.
- We use `/admin/{_locale}/bundle-name/rest` prefix for all administration front-end REST routing.


As we avoid splitting APIs, these are deprecated.
Now we just make single backend API with `/bundle-name/rest/v#` prefix. We pass language(s) in the `Accept-Language` header.

# `--force`

In some commands `--force` command line option is used to "really" perform something. This got misused and is almost never really needed.

As it's used in many places, now we always add this - not only in code before doing something, but when calling, also. And it teaches us that running command will not have any concequences, unless we pass some option. We must not just run some commands without knowing what they do or how they are supposed to work. If we need explanation, we just add `--help` option. By default, command should do what it's supposed to do.

Furthermore, if we always add this, it cannot be used when it would really make sense. This could be, for example, generating file, where `--force` option would always overwrite existing one without even asking.

# `markAs*`

In entities, we used to add `markAsDone` and similar methods for changing entity state. We tend to keep "long" version of this - `setStatus(Entity::STATUS_DONE)` so that we could easily find usages in the project without doing 2 iterations (first by `setStatus` and/or constant, then by `markAsX` method).

# common libraries / bundles

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


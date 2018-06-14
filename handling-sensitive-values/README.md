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


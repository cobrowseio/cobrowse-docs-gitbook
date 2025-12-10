---
description: >-
  Our JWT policy system provides a powerful mechanism to limit what a given
  agent can do. Which devices a support agent can connect to or which sessions
  they are allowed to view are a few examples.
---

# JWT Policies

Sometimes you may want to limit the scope of what a JWT can be used for. For example, you may want to limit which devices a support agent can connect to, or which sessions they are allowed to view. Our JWT policy system provides a powerful mechanism for you to configure these types of rules. &#x20;

The rules are defined as an extra `policy` claim in the [JWT](./), described using the following structure:

```json
{
  version: 3,
  sessions: { ... optional policy description ... },
  devices: { ... optional policy description ... }
}
```

When a policy claim is specified on a JWT, only the resources allowed by that policy can be accessed, i.e. an empty policy will not allow access to anything. &#x20;

The policy description allows you to enforce which values must appear on the resources in order for the JWT to be granted access. Only `custom_data` properties and resource `id`s are supported in policies at present. See the examples below for how to build your policy.

### Example JWT Policies

A policy that limits the JWT to **devices only** that are associated to a particular user id:

```javascript
{
  policy: {
    version: 3,
    devices: { custom_data: { user_id: 'my user id here' } }
  }
}
```

A policy that limits the JWT to **devices** and **sessions** that you have added your own custom data to:

```javascript
{
  policy: {
    version: 3,
    sessions: {
      custom_data: {
        my_custom_data: 'some value here',
        my_other_custom_data: 'some other value here'
      }
    },
    devices: {
      custom_data: {
        my_custom_data: 'some value here',
        my_other_custom_data: 'some other value here'
      }
    }
  }
}
```

### Custom Roles

The examples so far showed how to provide restricted access to specific resources, it's also possible to adjust the permissions of existing roles or create a custom set of permissions. For example a policy that allows access to start a session on any **device** or join any existing **session**:

```javascript
{
  role: null,
  policy: {
    version: 3,
    sessions: { },
    push:     { },
    devices:  { }
  }
}
```

In the same way granular access to each resource can be adjusted through policies. This policy allows full administrator permissions except for the ability to modify dashboard settings:

```javascript
{
  role: 'administrator',
  policy: {
    version: 3,
    features: { permissions: ['read'] }
  }
}
```

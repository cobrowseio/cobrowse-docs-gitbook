# JWT Polices

Sometimes you may want to limit the scope of what a JWT can be used for. For example, you may want to limit which devices a support agent can connect to, or which sessions they are allowed to view. Our JWT policy system provides a powerful mechanism for you to configure these types of rules. &#x20;

The rules are defined as an extra `policy` claim in the [JWT](./), described using the following structure:

```json
{
  version: 2,
  sessions: { ... optional policy description ... },
  devices: { ... optional policy description ... },
}
```

When a policy claim is specified on a JWT, only the resources allowed by that policy can be accessed, i.e. an empty policy will not allow access to anything. &#x20;

The policy description allows you to enforce which values must appear on the resources in order for the JWT to be granted access. Only `custom_data` properties and resource `id`s are supported in policies at present. See the examples below for how to build your policy.

### Example JWT Policies

A policy that limits the JWT to **devices only** that are associated to a particular user id:

```javascript
{
  version: 2,
  devices: { custom_data: { user_id: 'my user id here' } }
}
```

A policy that limits the JWT to **sessions only** that are associated to a particular user id:

```javascript
{
  version: 2,
  sessions: { custom_data: { user_id: 'my user id here' } }
}
```

A policy that limits the JWT to **devices** and **sessions** that you have added your own custom data to:

```javascript
{
  version: 2,
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
```

A policy that allows full access to **sessions**, but no access at all to **devices**:

```javascript
{
  version: 2,
  sessions: { }
}
```

A policy that allows access to a **session** with id _12345_, but no access at all to **devices**:

```javascript
{
  version: 2,
  sessions: { id: '12345' }
}
```

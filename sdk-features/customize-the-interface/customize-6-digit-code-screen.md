# 6-digit code screen

**Note:** The codes expire shortly after creation \(codes last less than 10 minutes\), so wait until your user is ready to share the code with an agent before generating it.

{% tabs %}
{% tab title="Web" %}
The Web SDK provides a promise that will resolve a 6-digit code. You may then show that to the end-user any way you would like.

```javascript
// ensure Cobrowse is loaded
CobrowseIO.client().then(function() {

    // create a code a display it to the user using your own UI
    CobrowseIO.createSessionCode().then(function(code) {
       console.log('your code is', code);
    });

});
```
{% endtab %}

{% tab title="iOS" %}
You can build your own UI to completely replace the default UI we provide for generating 6-digit codes. You can generate a code for your UI by using the `createSession` API.

## Swift

_Not yet documented. Please contact us at_ [_hello@cobrowse.io_](mailto:hello@cobrowse.io)_._

## Objective C

```objectivec
[CobrowseIO.instance createSession:^(NSError * _Nullable err, CBIOSession * _Nullable session) {
    if (err) NSLog(@"Error creating code %@", err);
    else NSLog(@"Created session code: %@", session.code);
}];
```

You can monitor changes in the state of the session you create using the Cobrowse delegate methods:

```objectivec
-(void) cobrowseSessionDidUpdate: (nonnull CBIOSession*) session;
-(void) cobrowseSessionDidEnd: (nonnull CBIOSession*) session;
```

You can get information about the state of the session using the following methods, which may adjust the UI you are showing:

```objectivec
[session isPending] // session has been created but is waiting for agent or user
[session isAuthorizing] // waiting for the user to confirm the session
[session isActive] // session running, frames are streaming to the agent
[session isEnded] // session is over and can no longer be used or edited
```
{% endtab %}

{% tab title="Android" %}
You can build your own UI to completely replace the default UI we provide for generating 6 digit codes. You can generate a code for your UI by using the `createSession` API:

```java
CobrowseIO.instance().createSession(new Callback<Error, Session>() {
    @Override
    public void call(@Nullable Error err, @Nullable Session session) {
        if (err != null) Log.e("CobrowseIO", "Error creating session " + err.getMessage());
        else if (session != null) Log.i("CobrowseIO", "Created session code " + session.code());
    }
});
```

You can monitor changes in the state of the session you create using the Cobrowse delegate methods:

```java
void sessionDidUpdate(final @NonNull Session session);
void sessionDidEnd(final @NonNull Session session);
```

You can get information about the state of the session using the following methods, which may adjust the UI you are showing:

```java
session.isPending() // session has been created but is waiting for agent or user
session.isAuthorizing() // waiting for the user to confirm the session
session.isActive() // session running, frames are streaming to the agent
session.isEnded() // session is over and can no longer be used or edited
```
{% endtab %}

{% tab title="React Native" %}
_Not yet documented. Please contact us at_ [_hello@cobrowse.io_](mailto:hello@cobrowse.io)_._
{% endtab %}

{% tab title="Xamarin" %}
You can build your own UI to completely replace the default UI we provide for generating 6 digit codes. You can generate a code for your UI by using the `CreateSession` API:

```csharp
CobrowseIO.Instance.CreateSession((error, session) =>
{
    if (error != null)
    {
        Debug.WriteLine("Error creating code: {0}", error);
    }
    else
    {
        Debug.WriteLine("Created session code: {0}", session.Code);
    }
});
```

You can monitor changes in the state of the session you create using the CobrowseIO delegate methods:

```csharp
public void SessionDidUpdate (Session session);
public void SessionDidEnd (Session session);
```

In **Xamarin.Forms** you can subscribe to the following `CobrowseIO` events:

```csharp
event EventHandler<ISession> SessionDidUpdate;
event EventHandler<ISession> SessionDidEnd;
```

You can get information about the state of the session using the following properties, which may adjust the UI you are showing:

| Property | Description |
| :--- | :--- |
| Session.IsPending | Session has been created but is waiting for agent or user |
| Session.IsAuthorizing | Waiting for the user to confirm the session |
| Session.IsActive | Session running, frames are streaming to the agent |
| Session.IsEnded | Session is over and can no longer be used or edited |
{% endtab %}
{% endtabs %}


# Listening for events

The Cobrowse SDKs offer a range of events that you can use to hook in to the lifecycle of a Cobrowse session. The lifecycle of a Cobrowse session is based on the following states:

**pending** - The session has been created, but an agent or device has not yet joined

**authorizing** - The session is waiting to start, but confirmation from the user is required

**active** - The screen share session is in progress, frames are streaming

**ended** - The session is finished and can no longer be used or updated

The typical transition between states of a Cobrowse session is: `pending` ( -> `authorizing`) -> `active` -> `ended`. If session authorization is disabled (i.e. no user consent is required), the authorizing step will be skipped.

### Session Lifecycle Events

You can get insight into the state of the session using the following lifecycle events:

#### Session Loaded Events

When a Cobrowse session is requested, either by the agent via a push channel, or via a 6 digit code, an event is triggered in the device-side SDKs. The loaded event signifies that a session is initialising, this event is a good opportunity to any one-time setup for the session. To listen to this event:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.loaded', session => {
    console.log('A session was loaded', session)
})
```
{% endtab %}

{% tab title="iOS / MacOS" %}
```objectivec
-(void) sessionDidLoad: (CBIOSession*) session {
    NSLog(@"A session was loaded %@", session);
}
```
{% endtab %}

{% tab title="Android" %}
```java
// Must implement CobrowseIO.SessionLoadDelegate
@Override
public void sessionDidLoad(@NonNull Session session) {
    Log.i("App", "A session was loaded " + session);
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.addListener('session.loaded', session => {
    console.log('A session was loaded', session)
})
```
{% endtab %}
{% endtabs %}

#### Session Updated Events

The session updated event is trigged multiple times throughout the life of a session. This signifies that a change to the session configuration has been made, potentially to any of the [session properties](../agent-side-integrations/agent-sdk/api-reference.md#interface-session). For example, the session has been switched into full device mode, or remote control was requested, or a range of other properties. You can tap in to this event to check the new state of the session:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.updated', session => {
    console.log('A session was updated', session)
})
```
{% endtab %}

{% tab title="iOS / MacOS" %}
```objectivec
-(void) sessionDidUpdate: (CBIOSession*) session {
    NSLog(@"A session was updated %@", session);
}
```
{% endtab %}

{% tab title="Android" %}
```java
// Must implement CobrowseIO.Delegate
@Override
public void sessionDidUpdate(@NonNull Session session) {
    Log.i("App", "A session was updated " + session);
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.addListener('session.updated', session => {
    console.log('A session was updated', session)
})
```
{% endtab %}

{% tab title="Xamarin" %}
```
public void SessionDidUpdate (Session session);
```

In Xamarin.Forms you can subscribe to the following CobrowseIO events:

```
event EventHandler<ISession> SessionDidUpdate;
```
{% endtab %}
{% endtabs %}

#### Session Ended Events

The session has been ended and the screenshare has stopped. An ended session cannot be restarted or modified. To listen for the session ended event:

{% tabs %}
{% tab title="Web" %}
```javascript
CobrowseIO.on('session.ended', session => {
    console.log('A session was ended', session)
})
```
{% endtab %}

{% tab title="iOS / MacOS" %}
```objectivec
-(void) sessionDidEnd: (CBIOSession*) session {
    NSLog(@"A session was updated %@", session);
}
```
{% endtab %}

{% tab title="Android" %}
```java
// Must implement CobrowseIO.Delegate
@Override
public void sessionDidEnd(@NonNull Session session) {
    Log.i("App", "A session was ended " + session);
}
```
{% endtab %}

{% tab title="React Native" %}
```javascript
CobrowseIO.addListener('session.ended', session => {
    console.log('A session was ended', session)
})
```
{% endtab %}

{% tab title="Xamarin" %}
```
public void SessionDidEnd (Session session);
```

In Xamarin.Forms you can subscribe to the following CobrowseIO events:

```
event EventHandler<ISession> SessionDidEnd;
```
{% endtab %}
{% endtabs %}

### Delegate implementation

Our SDKs offer many API touch points for you to customize the behaviour of your integration. These are primarily exposed through the implementation of a delegate object which you then pass to the Cobrowse SDK.&#x20;

{% tabs %}
{% tab title="iOS / MacOS" %}
On iOS we offer a protocol you can implement called `CobrowseIODelegate`. This offers a range of callbacks you can implement such as the lifecycle methods described above, as well as callbacks to control session UIs, consents and redactions.

To assign your delegate implementation, you should use:

```objectivec
CobrowseIO.instance.delegate = /* your delegate instance */
```
{% endtab %}

{% tab title="Android" %}
On Android we offer a range of delegate interfaces that you can implement depending on the functionality you wish to alter.

{% hint style="info" %}
You can only have one delegate instance configured, so the same instance must be used to implement any of the interfaces that you wish to use from the list here.
{% endhint %}

`CobrowseIO.Delegate` - provides callbacks for session lifecycle events

`CobrowseIO.SessionLoadDelegate` - provides callbacks for session lifecycle events

`CobrowseIO.SessionRequestDelegate` - for handling session consent requests

`CobrowseIO.RemoteControlRequestDelegate` - for session remote control consent

`CobrowseIO.SessionControlsDelegate` - for altering session indicator UIs

`CobrowseIO.RedactionDelegate` - an option for passing redactions to the SDK

To assign your delegate implementation, you should use:

```java
CobrowseIO.instance().setDelegate(/* your delegate implementation */);
```
{% endtab %}
{% endtabs %}

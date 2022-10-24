# How do I...

Our agent SDK is a powerful toolkit to build the experience that you want for your support agents. Here we have put together a number of examples of commonly required functionality.

### List only relevant sessions or devices using filters

Using our [custom data](../../sdk-features/identify-your-devices.md) mechanism you can add metadata that lets you link Cobrowse device and session records with your users. You can use this same data to filter when listing resources via our agent SDK.&#x20;

You can construct a filter query for a custom data entry by prepending "filter\_" to the key of your custom data. For example, `user_id` becomes `filter_user_id`:

```javascript
const sessions = await cobrowse.sessions.list({
    filter_user_id: 'xxxx'
    // other filtering properties are also supported.
    // See the API reference for more.
    state: ['ended']
})
```

The same system applied to both Session resources and Device resources. See the API reference for the available parameters:

{% content-ref url="api-reference.md" %}
[api-reference.md](api-reference.md)
{% endcontent-ref %}

### Listen for updates on a session or device

Often it's useful to know when the state of a session or device resource changes so you can provide a user interface that updates in real time. This is possible through our agent SDK.&#x20;

#### Subscribing to Session resources

```javascript
const sessions = await cobrowse.sessions.list({
    // add your filters in here, e.g.
    filter_user_id: 'xxxx', // customData filters start with "filter_"
    state: ['ended'] // session property filters 
})

// subscribe to updates for these Sessions
sessions.forEach((session) => {
    session.subscribe()
    session.on('updated', () => console.log('session was updated', session.id)
})
```

#### Subscribing to Device resources

```javascript
const devices = await cobrowse.devices.list({
    // add your filters in here, e.g.
    filter_user_id: 'xxxx', // customData filters start with "filter_"
})

// subscribe to updates for these Devices
devices.forEach((device) => {
    device.subscribe()
    device.on('updated', () => console.log('device was updated', device.id)
})
```

### Control an IFrame embedded in my support agents' portal

When integrating Cobrowse to your own custom helpdesk or CRM, many customers need to control some aspects within the Cobrowse IFrames, such as switch the agent tool, or ending the session. We provide an easy mechanism for this (no JWT required!)

```javascript
const cobrowse = new CobrowseAPI() // JWT not required
    
// attach to iframe (make sure it has loaded!)
const frameEl = document.getElementById('myIframe')
const ctx = await cobrowse.attachContext(frameEl)
    
// listen for updates to session
ctx.on('session.updated', (session) => {
    console.log('session was updated', session)
    if (session.ended) {
        console.log('session has ended')
        ctx.destroy()
    }
})
    
// interact with the iframe
await ctx.setTool('laser')
await ctx.clearAnnotations()
await ctx.setFullDevice(true)
await ctx.setRemoteControl('requested')
await ctx.endSession()
```

See the API reference for full details on what you can do [using an IFrame context](https://docs.cobrowse.io/agent-side-integrations/agent-sdk/api-reference#interface-remotecontext). You may also find the embed documentation useful:

{% content-ref url="../custom-iframe-embeds.md" %}
[custom-iframe-embeds.md](../custom-iframe-embeds.md)
{% endcontent-ref %}

### Access and update the session in the IFrame

Sometimes it's useful to access the session that was created in an IFrame in order to modify some properties that cannot be directly controlled through the IFrame methods. First you'll need to get the access to the session from the IFrame:

```javascript
const cobrowse = new CobrowseAPI(...) 
    
// attach to iframe (make sure it has loaded!)
const frameEl = document.getElementById('myIframe')
const ctx = await cobrowse.attachContext(frameEl)

// track the latest session that has been loaded into (or created by)
// the iframe 
let currentSession = null
ctx.on('session.updated', session => {
    latestSession = session
})
```

Then at some point later, when you want to make a change to the session, you use the session object that was passed in the `session.updated` event. **Note**: you will need to configure a JWT in the agent SDK to call some methods that require authentication on the session object.

### Add extra data to a session from the agent side

Sometimes the data that you want to associate with a session may not be available from the user-side to use with the device side SDKs.  In this case it is possible to add custom data from the agent side that will remain associated with a session.

```javascript
// create an API instance
// Note: a valid JWT is required to modify the session
const cobrowse = new CobrowseAPI(token)

// This function can be used to watch a window (from window.open()), or
// an Iframe, for changes in the Cobrowse session. When a new session is
// detected we can then update the custom_data field to include extra
// data not provided by the device directly. 
async function watch (windowOrIframe) {
  const ctx = await cobrowse.attachContext(windowOrIframe)
  console.log('Watching', windowOrIframe, 'for cobrowse session changes')
  ctx.on('session.loaded', async (session) => {
    console.log('Detected a change in Cobrowse session', session)
    // Modify the session however is required, in this example
    // we'll add two new custom_data properties
    const myData = { some_id: '12345', another_id: '54321' }
    // Save the changes back to the session
    await session.update({ custom_data: myData })
    console.log('Updated session custom data with', myData)
  })
}

function openSessionById (sessionId) {
  // In this example we'll open a new window for the Cobrowse session,
  // using the window.open() browser API available in JavaScript.
  // This would work equalliy well when connecting to a device via
  // our /connect or /code embed APIs. Additionally, this would work 
  // when using an Iframe rather than window.open().
  watch(window.open(`${cobrowse.api}/session/${sessionId}?end_action=none&token=${cobrowse.token}`))
}

// Call this function with a valid session ID. You can obtain session IDs
// either via the REST API, the Cobrowse attachContext() APIs,or your own
// out-of-band channel (e.g. your CRM integration or livechat platform)
openSessionById('some session id here')
```

See the links below for more on how to generate a token or choose the best link to embed.

{% content-ref url="../custom-iframe-embeds.md" %}
[custom-iframe-embeds.md](../custom-iframe-embeds.md)
{% endcontent-ref %}

{% content-ref url="../json-web-tokens-jwts/" %}
[json-web-tokens-jwts](../json-web-tokens-jwts/)
{% endcontent-ref %}

### Completely replace the session UI with my own design

Cobrowse is designed to be fully customizable, including the agent side experience. We've put together a small example to show how to build your own in-session UI. Find the full [source code on GitHub](https://github.com/cobrowseio/cobrowse-agent-sdk-examples/tree/master/custom-agent-demo), or try the demo [here](https://cobrowseio.github.io/cobrowse-agent-sdk-examples/custom-agent-demo/).

### Check the number of active sessions

For customers using our concurrency based licenses, it's often useful to know how many active sessions are in progress, and therefore how close to your license limits you are. The sample below provides a basic way to list all `active` sessions for your account.

```javascript
// token must have role: administrator claim
const cobrowse = new CobrowseAPI(token)
const sessions = await cobrowse.sessions.list({
    state: ['active'],
    agent: 'all',
    limit: 1000
})
console.log('Active sessions', sessions.length)
```

{% hint style="info" %}
**Note**: if you have more than 1000 _concurrently_ `active` sessions you will need to add paging to this example to count them all.
{% endhint %}

### End the session after a chat/interaction, or after the browser is closed

There's multiple ways to end a Cobrowse session, which one to use will depend on your use case. The options include:

1. Using the device-side SDKs to call `session.end()` (the exact syntax will depend on the SDK used)
2. Using the default agent-side UI by clicking the red "hang-up" icon
3. Using the agent SDK APIs to programatically end the session, either via the [IFrame context](https://docs.cobrowse.io/agent-side-integrations/agent-sdk/api-reference#endsession), or via an[ API call](https://docs.cobrowse.io/agent-side-integrations/agent-sdk/api-reference#end) with a JWT.
4. Letting the server end the session after a timeout. If the device or agent closes the app or browser the server will clean up sessions automatically after approximately 5 minutes of inactivity.

### Leave without ending the session

Sometimes it's useful to be able to leave the session without ending it, for example when a supervisor is monitoring a number of ongoing sessions needs to switch between them. You can simply close the tab or remove the IFrame without ending the session to achieve this.

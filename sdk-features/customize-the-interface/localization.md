---
description: >-
  Customise and localize the Cobrowse interface to offer a tailored experience.
  The website SDK provides support for more than 20 languages.
---

# Localization / translation

You may use the documentation under [Customize the interface](./) to completely replace our default UI in our SDKs with your own custom UI. However, some clients prefer to use our existing default UI, but simply localize or modify the text strings.

{% tabs %}
{% tab title="Web" %}
Our web SDK contains default language support for the following languages:

* English
* French
* German
* Spanish
* Spanish Mexico (es-mx)
* Italian
* Japanese
* Chinese Simplified (zh-hans)
* Chinese Traditional (zh-hant)
* Hebrew Israel (coming soon!)
* Korean
* Malay (coming soon)
* Marathi India
* Portuguese
* Portuguese Brazil (pt-br)
* Thai
* Vietnamese
* Russian
* Arabic UAE (coming soon!)
* Hindi India
* Dutch
* Czech (coming soon!)
* Slovak (coming soon!)
* Slovenian (coming soon!)
* Lithuanian (coming soon!)
* Polish (coming soon!)
* Kazakh (coming soon!)
* Romanian (coming soon!)
* Ukrainian (coming soon!)
* Estonian (coming soon!)
* Danish (coming soon!)
* Finnish (coming soon!)
{% endtab %}

{% tab title="iOS" %}
Create a new strings file named `CobrowseIO.strings` in your app project and select all localization you want to support in the File Inspector. Refer to [the Apple documentation](https://developer.apple.com/documentation/xcode/localization) to see more details about this approach.

The content of `CobrowseIO.strings` should be the following (replace string values with your own text):

```
"CodeDisplayViewMessageInstruction" = "Provide this code to your support agent to begin screen sharing.";
"ErrorDisplayViewMessage" = "Sorry, something went wrong. Check you're online and try again.";
"ManageSessionViewMessage" = "You're sharing screens from this app with a support agent.";
"ManageSessionViewButtonEnd" = "End Session";
"CobrowseViewButtonClose" = "Close";
"FullDevicePromptViewButtonCancel" = "Cancel";
"FullDevicePromptViewMessageSimulator" = "Full device screen capture is not available in the device simulator.";
"FullDevicePromptViewMessageNoExtension" = "Full device screenshare is not available as a broadcast extension has not been configured.";
"FullDevicePromptViewMessageTap" = "Tap the record icon to manage full device screen sharing.";
"FullDevicePromptViewMessageOpenControlCenter" = "Open the iOS control center and deep press the record icon to start full device screen sharing.";
"FullDevicePromptViewMessageNotSupported" = "This version of iOS doesn't support full device screen sharing.";
"SessionRequestConsentPromptTitle" = "Support Request";
"SessionRequestConsentPromptMessage" = "A support agent would like to use this app with you. Do you wish to allow this?";
"SessionRequestConsentPromptAllow" = "Allow";
"SessionRequestConsentPromptDeny" = "Deny";
"RemoteControlConsentPromptTitle" = "Remote Control Request";
"RemoteControlConsentPromptMessage" = "A support agent would like to remotely control this app. Do you wish to allow this?";
"RemoteControlConsentPromptAllow" = "Allow";
"RemoteControlConsentPromptDeny" = "Deny";
"SessionIndicatorDialogTitle" = "End Session";
"SessionIndicatorDialogMessage" = "Do you want to end the screen share?";
"SessionIndicatorButtonEnd" = "End";
"SessionIndicatorButtonCancel" = "Cancel";
```

### Full Device Screen Share

In order to localize messages in the **broadcast upload extension** used for full device screen sharing, create another file named `CobrowseIO.strings` **in the extension project** and select all localization you want to support in the File Inspector.

The content of CobrowseIO.strings should be the following (replace string values with your own text):

```
"ReplayKitExtensionNoSession" = "No session available";
"ReplayKitExtensionSessionNotFound" = "Cobrowsing session wasn't found";
"ReplayKitExtensionSessionNotActivated" = "Cobrowsing session wasn't activated yet";
"ReplayKitExtensionSessionEnded" = "Session ended";
"ReplayKitExtensionSessionNoLongerFullDevice" = "Cobrowsing full-device session has ended";
"ReplayKitExtensionLicenseKeyMissing" = "License key missing";
"ReplayKitExtensionApiMissing" = "API missing";
```
{% endtab %}

{% tab title="Android" %}
Create a new `strings-cobrowse.xml` resource file in your app project:

* in the `values` directory, in order to modify the text strings
* in the `values-XX` directory where `XX` represents a two-letter ISO 639-1 language code, in order to localize the text strings

Refer to [the Android documentation](https://developer.android.com/guide/topics/resources/localization) to see more details about this approach.

The content of `strings-cobrowse.xml` should be the following (replace XML values with your own text):

```markup
<resources>
    <string name="cobrowse_code_display_description">Provide this code to your support agent to begin screen sharing.</string>
    <string name="cobrowse_approve_session_description">A support agent would like to use this app with you. Do you accept?</string>
    <string name="cobrowse_approve_session_button_yes">Accept</string>
    <string name="cobrowse_approve_session_button_no">Cancel</string>
    <string name="cobrowse_manage_session_description">You\'re sharing screens from this app with a support agent.</string>
    <string name="cobrowse_screenshare_request_title">Screen Sharing Request</string>
    <string name="cobrowse_button_end_session">End Session</string>
    <string name="cobrowse_error_generic">Sorry, something went wrong. Check you\'re online and try again.</string>
    <string name="cobrowse_accessibility_service_description">Allows remote support</string>
    <string name="cobrowse_cobrowse_accessibility_setup_title">Accessibility Service Setup</string>
    <string name="cobrowse_foreground_service_title">Your screen is shared with a support agent</string>
    <string name="cobrowse_full_device_description">To allow full device control from remote support agents please enable the accessibility service for this app.</string>
    <string name="cobrowse_full_device_title">Action Required</string>
    <string name="cobrowse_open_accessibility_button">Open Settings</string>
    <string name="cobrowse_service_name">Remote Support</string>
    <string name="cobrowse_notification_channel_name">Support Notifications</string>
    <string name="cobrowse_remote_control_request_title">Remote Control Request</string>
    <string name="cobrowse_approve_remote_control_description">A support agent would like to control this app. Do you accept?</string>
    <string name="cobrowse_approve_remote_control_button_yes">Accept</string>
    <string name="cobrowse_approve_remote_control_button_no">Cancel</string>
</resources>
```
{% endtab %}
{% endtabs %}

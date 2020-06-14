# Salesforce

Learn more at [https://cobrowse.io/salesforce](https://cobrowse.io/salesforce)

## Install the app

Please install Cobrowse.io for Salesforce from the Salesforce App Exchange. You will also need to sign-up for an account via the Cobrowse.io website to use with your Salesforce.

Once Cobrowse.io for Salesforce is installed into your Salesforce instance, you will see Cobrowse.io Settings and Cobrowse.io Dashboard available via the App Launcher.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-app-launcher-1.38881e87.png)

## Insert the license key

The first step is to insert your license key into Cobrowse.io Settings, to associate your Cobrowse.io account with your Salesforce instance.

To generate a license key, please create an account via the Cobrowse.io website. Then you can enter it via the App Launcher by navigating to Cobrowse.io Settings. You can leave Filter Config as it is for now.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-license-key-1.fcc59a86.png)

If you have not entered your valid license key into your Salesforce settings, you will receive the following error when trying to load Cobrowse.io Dashboard via the App Launcher in Salesforce.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-license-key-0.8b36e593.png)

## Grant access to Users

Next, you need to define which Users will have access to Cobrowse.io within Salesforce. You will need to go to Setup -&gt; App Manager and then click to Manage the CobrowseIO app as shown in the screenshot.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-manage-1.4cc872d7.png)

We want users to automatically be granted access, as long as they belong to the valid Profiles or have the valid Permission Set assignments, and are already logged into Salesforce. Click Edit Policies as shown in the screenshot.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-manage-2.6bb31379.png)

Please change the option under OAuth Policies -&gt; Permitted Users to "Admin approved users are pre-authorized". Then click Save.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-manage-3.343b84f6.png)

Now, if you try to load Cobrowse.io Dashboard you will see the following error. This is because we have not yet defined the Profiles to have access to Cobrowse.io within Salesforce, or assigned any users the "Cobrowse.io Agent" or "Cobrowse.io Admin" Permission Sets.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-profiles-0.c03e02f1.png)

From the App Manager, go back to "Manage Profiles" for the CobrowseIO app.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-profiles-1.3240bb6e.png)

Now, select the System Administrator Profile you want to grant access. Once this is done, you can try to refresh the Cobrowse.io Dashboard via the App Launcher to see the page load.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-profiles-2.f91bd2dd.png)

This works for the System Administrator, but Cobrowse.io for Salesforce also comes with two Permission Sets, namely "Cobrowse.io Agent" or "Cobrowse.io Admin". You can add these Permission Sets to Profiles, or assign them to Users individually.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-permission-sets-1.559b2fcc.png)

Here's what it looks like to assign these Permission Sets. Admins will be able to edit the Cobrowse.io account-level settings, whereas Agents will only be able to use the tool.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-permission-sets-2.4f1086c9.png)

If you have any questions at all, we'd be happy to guide you through it. Please email us at [support@cobrowse.io](mailto:support@cobrowse.io)!

## Generate signing certificate

As long as your users are authenticated with Salesforce, they will automatically be authenticated with Cobrowse.io. We achieve this by using JSON Web Tokens \(JWTs\) which are signed on the Salesforce backend by a private signing certificate, and verified on the Cobrowse.io backend by the certificate's corresponding public key.

In Salesforce, please naviate to Setup -&gt; Certificate and Key Management and "Create Self-Signed Certificate" named "CobrowseIO".

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-cert-1.d88f474d.png)

Here's what it looks like when you create the new certificate.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-cert-2.c14cb86a.png)

Next, you will need to download the certificate so that you can extract the certifiate's corresponding public key. Make sure to delete it from your local computer afterwards, because it can be used to authenticate your Salesforce users to Cobrowse.io.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-cert-3.ce1b72b6.png)

Once the file is downloaded, use the command `openssl x509 -pubkey -noout -in CobrowseIO.crt` to extract the public key.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-cert-4.3347be53.png)

Finally, login to the Cobrowse.io website and paste it in under JWT SSO in your settings page.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-cert-5.b21dabb3.png)

## Visualforce widget placement

Now, when you load Cobrowse.io Dashboard via the App Launcher in Salesforce, you should see a list of any active devices. If you don't see any, then make sure you've added the Cobrowse.io SDK to your apps.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-dashboard-tab.b39d16d2.png)

You can also add the Cobrowse.io widget to your record pages for Cases, Contacts, Accounts, or any other record pages accessible via Sales Console, Service Console, or otherwise. To do this, navigate to that page and click the settings button in the top right to "Edit Page".

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-edit-page-1.f3ce1cea.png)

The Cobrowse.io widget is exposed as a Visualforce page which can be dragged and dropped into your page layout. Most clients will position the Cobrowse.io widget on the right sidebar with `Height: 900`.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-edit-page-2.9f80b842.png)

Once you click save, you'll have the ability to "Assign as Org Default" for all users in your Salesforce organization.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-edit-page-3.a509484e.png)

One more confirmation dialog to finish.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-edit-page-4.b5ba3d82.png)

Now you will see the Cobrowse.io Widget in the sidebar of this Record page. Here it is shown for a Contact Record. Notice how the user's devices are already pre-filtered for you by e-mail address from the context of the page.

![salesforce app for cobrowse.io documentation](https://cobrowse.io/static/media/salesforce-edit-page-5.e157d40c.png)

If you'd like to modify which fields are used to pre-filter the relevant devices for a particular Record, this is what the Filter Config setting is for under App Launcher -&gt; Cobrowse.io Settings. Please email us at [support@cobrowse.io](mailto:support@cobrowse.io) if you have any questions!


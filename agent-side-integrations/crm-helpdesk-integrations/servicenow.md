---
description: >-
  Cobrowse integrates seamlessly with ServiceNow workflows, enabling real-time
  visual collaboration between agents and customers. This integration is
  available through the ServiceNow Store.
---

# ServiceNow

## Overview

Cobrowse integrates seamlessly with ServiceNow workspaces such as _Service
Operations Workspace_ or _Customer Service Management_, enabling real-time
visual collaboration between agents and customers. This integration is available
through the [ServiceNow Store](https://store.servicenow.com).

## Installation guide

To install and configure the Cobrowse integration with ServiceNow:

1. Sign up for a [Cobrowse account](https://cobrowse.io/register)
2. As a Cobrowse Administrator:
   1. Generate a public key under Settings > Integrations > JWT SSO.
   2. Generate a key store for it under Settings > Integrations > ServiceNow
3. As a ServiceNow administrator:
   1. Install the app from the [ServiceNow Store](https://store.servicenow.com).
   2. Add the keystore under All > Multi-Provider SSO > Administration > x509
      Certificate.
      1. Name: Cobrowse.io
      2. Type: Java Key Store
      3. Attachments > Choose your generated keystore
      4. Submit
   3. Link the new keystore for the Cobrowse JWT key under All > System OAuth >
      JWT Keys > Cobrowse.io
      1. Signing Keystore: Click the magnifying glass and look for the added
         keystore
      2. Signing Key: The password that was given on the cobrowse dashboard. By
         default, this is `my-cobrowse-keystore`.
   4. Configure the system properties, All > `sys_properties.list` + enter >
      Search for `x_1781074`:
      1. Update the `x_billc_cobrowse_i.license` property and set it to your
         cobrowse account license which you find on the cobrowse dashboard>
         Settings > General
      2. _(Optional)_ If you are
         [self-hosting](../../enterprise-self-hosting/self-hosting-overview.md)
         your Cobrowse.io instance, update the `x_billc_cobrowse_i.base_url`
         system property and point it at your own installation.

### Enabling Cobrowse for Portable Virtual Agent chat widget

Don't forget to include the [Cobrowse SDK](../../sdk-installation/web.md) into
the webpage where the Portable Virtual Agent chat widget is running. No further
changes should be necessary on the ServiceNow side to enable cobrowsing. Your
license key can be found in your **Account Settings**, seen above.

## Controlling user access

The Cobrowse ServiceNow application includes two roles for managing user
permissions:

1.  `x_billc_cobrowse_i.Administrator`

    Grants access to the Cobrowse.io application via the All > Cobrowse.io >
    Dashboard. Users with this role can view active devices, sessions, and
    settings.

2.  `x_billc_cobrowse_i.Agent`

    Grants access to the Cobrowse sidebar, allowing agents to initiate cobrowse
    sessions during customer interactions.

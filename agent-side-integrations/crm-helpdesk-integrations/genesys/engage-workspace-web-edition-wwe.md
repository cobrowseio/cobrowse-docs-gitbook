---
description: Engage Cloud and Engage On-Premise WWE
---

# Engage Workspace Web Edition \(WWE\)

## Installation

### 1. Define the Interaction Tab

Define a new Interaction Tab in your WWE Cluster configuration using Genesys GAX or Genesys Administrator.

You may name the tab as you wish, for example CobrowseIOTab. The tab should have the following values:

* **label** – The label that should be displayed in the tab within the WWE UI, eg. COBROWSE
* **url** – The URL to the web content that will be displayed within the tab.  The URL can also contain variable names pertaining to agent and interaction data. This value will be provided by Cobrowse.io. The default value when using the Cobrowse.io hosted version is https://cobrowse.io/apps/genesys/index.html?env=wwe&license=&lt;your license key&gt;

More information about how to add a new Interaction Tab to Workspace Web Edition \(WWE\) available at [https://developer.genesys.com/customizing-genesys-workspace-web-edition-2/](https://developer.genesys.com/customizing-genesys-workspace-web-edition-2/).

### 2. Add the Tab to your Workspace

Navigate to the interaction-workspace/interaction.web-content and add the value CobrowseIOTab to this list to enable it. 

### 3. Start Workspace Web Edition

You should now see the COBROWSE tab within your Workspace Web Edition when interacting with an end-user, such as on a phone call, or via a live chat. 

Demo video - [https://www.youtube.com/watch?v=ZKx5bOsodtw](https://www.youtube.com/watch?v=ZKx5bOsodtw)




---
sidebar_position: 1
---

# Getting Started: Two Ignition Architecture
**This tutorial provides step-by-step instructions for installing and configuring a two-gateway Ignition architecture using MQTT.**

## Index
**Ignition**: an Industrial Application Platform that can be used to create SCADA and HMI solutions. A fully functional Ignition system can be downloaded and run in a trial mode running for two hours at a time with unlimited restarts. Using Ignition as a tool in this way allows us to install the Sparkplug MQTT Modules and observe everything working.

**Ignition Edge**: a leaner version of Ignition made specifically for use in on edge-of-network devices. Ignition comes with unlimited Tags, Clients, and database connections, while Ignition Edge comes with unlimited Tags, two Clients (one local and one remote) and no database connectivity.

**MQTT Distributor**: An MQTT Server that runs as an Ignition module.

**MQTT Engine**: An MQTT Client that implements the Sparkplug specification and automatically creates Ignition tag structures for Edge Node and Device metadata and process variables.

**MQTT Transmission**: An MQTT Client that implements the Sparkplug specification to bridge local Ignition tags (OPC-UA and Memory tags) and publish the resulting structure to an MQTT infrastructure.

[photo of architecture is here] 

## Prerequisites
- Two machines to run the two instances of **Ignition**, or **Ignition + Ignition Edge**
- **Ignition** can run on a laptop, in the cloud via an AWS EC2 instance, or some other development computer.
- **Ignition Edge** can run on one of many supported embedded edge of network gateways, a laptop or development computer, a Raspberry Pi (load ARMHF version), or also in a cloud service.

## Check Compatibility
Find the latest compatible Cirrus Link Solutions MQTT Modules for Ignition Version **8.1.xx**, as this may not correspond to the most recent version of Ignition. To do this, go to the Strategic Partner Modules tab on the [Ignition Downloads page](https://inductiveautomation.com/downloads/third-party-modules/8.3.3). Use the drop-down menu to find the stable **8.1.xx** version.

[photo of Ignition Strategic Partner Modules tab here]

* This is the version you will use to download Ignition *and* Cirrus Link MQTT Modules (pictured below the drop-down menu).

## Configure the Primary Machine
1. From the [Ignition Version Archive](https://inductiveautomation.com/downloads/archive), select and download the appropriate Ignition Installer version for either Windows, Linux or MacOS.
[photo of Ignition downloads tab here]
2. Follow Inductive Automation’s guide to [install and start Ignition](https://www.docs.inductiveautomation.com/docs/8.1/getting-started/installing-and-upgrading).
> *Note: You will use the username & password credentials again in the next steps, so keep them readily available.*
3. Once Ignition is successfully downloaded and started, go to the Strategic Partner Modules tab on the [Ignition Downloads page](https://inductiveautomation.com/downloads/third-party-modules/8.3.3). Again, use the drop-down menu to find the appropriate **8.1.xx** version(s).
4. Download the **MQTT Distributor Module** and **MQTT Engine Module** under 'Cirrus Link Solutions MQTT Modules for Ignition'.
> On your primary machine, remember to either turn off firewalls or, at a minimum, allow inbound connections to TCP/IP port #1883 and port #8883, as remote MQTT Clients will need to be able to establish a TCP/IP socket connection to these ports. --> *[how do I do this?](www.google.com)* 

## Configure the Secondary Machine
1. Repeat steps 1 through 3 from configuring the primary machine, on the secondary machine.
> *Note: You will use the previously created username & password credentials here, so have them readily available.*
2. Download the **MQTT Transmission Module** under ‘Cirrus Link Solutions MQTT Modules for Ignition’.

## Configure the Ignition Gateway on Both Machines
### Primary Machine

1. In the Ignition left hand side bar, navigate to **Config** → **SYSTEM** → **Modules**. Scroll to the bottom of the page and click on the link **Install or Upgrade a Module…**

[photo]

2. When prompted, select the MQTT Distributor module from the file browser and select Install. Accept the license agreement and certification and install the module. Repeat for the MQTT Engine module.
3. When complete, the Ignition Gateway will show the current state of the installed modules (pictured above).

- By default, MQTT Engine is already configured to point to an MQTT Server at tcp://localhost:1883, which means it will automatically connect to the MQTT Distributor installed with it. No additional configuration is required.

4. To verify the connection status, navigate to the **Config** tab → **MQTT ENGINE** → **Settings** → **Servers** tab. Confirm that the Chariot SCADA Status shows ‘Connected’.

### Secondary Machine
1. In the Ignition left hand side bar, navigate to **Config** → **SYSTEM** → **Modules**. Scroll to the bottom of the page and click on the link **Install or Upgrade a Module…**

2. When prompted, select the **MQTT Transmission** module from the file browser and select Install. Accept the license agreement and certification and install the module.

3. When complete, the Ignition Gateway will show the current state of the installed module:

[photo]

4. MQTT Transmission needs to be configured to point to the MQTT Distribution server in order to publish data into the MQTT Engine. To do this, navigate to: **Config** → **MQTT TRANSMISSION** → **Settings** → **Servers** tab. 

5. Edit the MQTT Server named Chariot SCADA to modify the URL to point to the Primary machine’s Ignition Gateway IP Address.

> *[(?) How can I find the IP Address on my machine?](www.google.com)*

- Example: If your IP Address is 10.1.10.97 on your Primary machine, then set the URL to **tcp://10.1.10.97:1883** and **Save Changes**.

- Once configured, MQTT Transmission will automatically connect, and you can confirm by checking that the ‘Connected’ status on the Servers tab shows ‘1 of 1’.

> Validation check: ie. how is the user doing at this point? How can we provide answers for pain points in advance?

## Launch Designer on both Machines
#### At this point, you are ready to edit the default tag created during the MQTT Transmission installation so that data can be published and observed. 

1. From the Ignition Gateway web interface, select **Get Designer** and follow the instructions to download, install, and launch **Designer Launcher.**

[photo]

2. Once Ignition Designer Launcher has launched, navigate to **Settings** in the top right hand corner. 
3. Select **Add Designer**.
4. Once a Designer is added (example pictured below: **Ignition-itsgivinggreen** is the Designer), log in using the username and password previously created in Ignition.

[photo]

> Note: if you enabled the Quick Start option when starting Ignition, a samplequickstart project will have been created and you will need to open that project. 

5. Under the Tag Browser section on the left hand side, find the drop down menu and select **default**.
6. Expand the Edge Nodes folder tree until you have exposed the **PLC 1** folder with the **Example Tag**.
7. Complete steps 1 - 6 in this section on both machines.

### Test Your Connection
1. On your Secondary machine, where you see the Value as 1, change it to 2.
2. On your Primary machine, verify in the same folder tree that the value has changed from 1 to 2.

> The Ignition Designer tool is not by default in Read/Write mode, so your first attempt to write a new value to this Example Tag will ask you to select either Cancel | Enable Read/Write Mode | Write Once.  Select Write Once or Read/Write to allow Tag value changes.

At this point you have a fully functional system that can be expanded or modified as required.  Below are some additional activities you may want to try on your own.

## Extra Activities
Allow outbound tag writes. Video 9: Allow Outbound Tag Writes
Disable MQTT Transmission to see the tags go stale in MQTT Engine
Set the 'Primary Host ID'.  This is a setting that is highly recommended and should be set on both MQTT Engine and any MQTT Transmission instances that are reporting in as well. Video 10: Primary Host ID Setting
Modify the tags folder to add additional memory tags Creating Tags in Ignition and force an update to the Ignition Gateway Using the MQTT Transmission Refresh Mechanism
TLS enable the MQTT Distributor module and disable port 1883. Video 11: How to Set Up Transport Layer Security 
Set up Store-and-Forward in MQTT Transmission to show data being saved when the connection goes down. Video 12: Set Up Store-and-Forward System

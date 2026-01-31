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
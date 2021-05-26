# vmware.vmware_rest.content_subscribedlibrary

**Creates a new subscribed library**

Version added: 1.0.0


* Synopsis


* Requirements


* Parameters


* Status

## Synopsis


* Creates a new subscribed library. <p> Once created, the subscribed
library will be empty. If the mailto:[{@link](mailto:{@link)
LibraryModel#subscriptionInfo} property is set, the Content Library
Service will attempt to synchronize to the remote source. This is
an asynchronous operation so the content of the published library
may not immediately appear.

## Requirements

The below requirements are needed on the host that executes this
module.


* python >= 3.6


* aiohttp

## Parameters

## Status

### Authors


* Ansible Cloud Team (@ansible-collections)

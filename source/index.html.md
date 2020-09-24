---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to Sidescale's implementation of the Apache CloudStack API. This page will contain the reference documentation for 253 API requests, and all the information you need to get started using the API. **The documentation is currently under construction.**

Complete documentation for the Apache CloudStack API can be found [here](https://cloudstack.apache.org/api/apidocs-4.11/index.html).

# Authentication

## Generating API keys

To access the Apache CloudStack API, you will need to generate an API key and a secret key. To do so, log in to [sidescale.com](sidescale.com), navigate to [sidescale.com/client](sidescale.com/client), and follow these steps:

1. Select the **Accounts** tab in the left sidebar.
2. Select **People**, then **View Users**.
3. Select your username, then the **Generate Keys** icon.

Your API key and secret key will be automatically generated, appearing midway down the page.

<aside class="warning">
Note that, due to the permission level Sidescale assigns to you, you will only be able to generate an API key for your account.
</aside>

## Installing CloudMonkey

To make API calls with the Apache CloudStack API, you will need to install a command line interface for doing so. We recommend [CloudMonkey](https://github.com/apache/cloudstack-cloudmonkey), and will explain how to use it here. Those skilled in Python might prefer using [Exoscale](https://github.com/exoscale/cs).

You will need to have [Go](https://golang.org/doc/install) installed before you can use CloudMonkey.

To install CloudMonkey, follow the instructions on the [**Getting Started**](https://github.com/apache/cloudstack-cloudmonkey/wiki/Getting-Started) page of the repository's wiki.

## Configuring your CloudMonkey server profile

> To authorize CloudMonkey for use with the Sidescale API, enter the following commands:

```shell
cmk set profile mycloud
cmk set url https://sidescale.com/client/api
cmk set apikey <apikey>
cmk set secretkey <secretkey>
```

The first time you use CloudMonkey, it will create a default server profile called `localcloud`. You will need to create a new server profile and configure it with the commands in the panel on the right.

<aside class="notice">
Alternatively, you can make API requests from within the CloudMonkey interactive shell. See the CloudMonkey <a href="https://github.com/apache/cloudstack-cloudmonkey/wiki">wiki</a> for more information.
</aside>

# Making API requests

To make API requests, you must include `cmk`, the command you are using, and any required or optional query parameters you wish to use. Query parameters are passed as `key=value` pairs. For example, to request all of the VM instances owned by the Admin user, enter:

`$ cmk listVirtualMachines domainId=1 account=admin`

# Event

## deleteEvents

This command deletes one or more events.

<aside class="notice">
Not asynchronous
</aside>

### Command

> The following command deletes one or more events:

```shell
$ cmk deleteEvents
```

`deleteEvents`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ids**|The IDs of the events|list|255|false|
|**type**|Delete by event type|string|255|false|
|**enddate**|End date range to delete events (including) this date (use format \"yyyy-MM-dd\" or the new format \"yyyy-MM-ddThh:mm:ss\")|date|255|false|
|**startdate**|Start date range to delete events (including) this date (use format \"yyyy-MM-dd\" or the new format \"yyyy-MM-ddThh:mm:ss\")|date|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|

# Hypervisor

## listHypervisors

This command lists hypervisors.

<aside class="notice">
Not asynchronous
</aside>

### Command

> The following command lists hypervisors:

```shell
$ cmk listHypervisors
```

`listHypervisors`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zone id**|The zone ID for listing hypervisors|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|string|The UUID of the latest async job acting on this object|
|**name**|string|Hypervisor name|
|**jobstatus**|integer|The current status of the latest async job acting on this object|

# Template

## listNuageVspDomainTemplates

This command lists Nuage VSP domain templates.

<aside class="notice">
Not asynchronous
</aside>

### Command

> The following command lists Nuage VSP domain templates:

```shell
$ cmk listNuageVspDomainTemplates
```

`listNuageVspDomainTemplates`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zoneid**|The zone ID|uuid|255|false|
|**keyword**|Filters the domain templates which contain the keyword|string|255|false|
|**physicalnetworkid**|The physical network ID|uuid|255|false|
|**domainid**|The domain ID|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|

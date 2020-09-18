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

Welcome to Sidescale's implementation of the Apache CloudStack API. This page contains the reference documentation for 253 API requests, and all the information you need to get started using the API.

Complete documentation for the Apache CloudStack API can be found [here](https://cloudstack.apache.org/api/apidocs-4.11/index.html).

# Authentication

## Generate API Keys

To access the Apache CloudStack API, you will need to generate an API key and a secret key. To do so, log in to [sidescale.com](sidescale.com), navigate to [sidescale.com/client](sidescale.com/client), and follow these steps:

1. Select the **Accounts** tab in the left sidebar.
2. Select **People**, then **View Users**.
3. Select your username, then the **Generate Keys** icon.

Your API key and secret key will be automatically generated, appearing midway down the page.

<aside class="warning">
Note that you will only be able to generate an API key for your account due to the permission level Sidescale assigns to you.
</aside>

## Install CloudMonkey

To make API calls with the Apache CloudStack API, you will need to install a command line interface for doing so. We recommend [CloudMonkey](https://github.com/apache/cloudstack-cloudmonkey), and will explain how to use it here. Those skilled in Python might prefer using [Exoscale](https://github.com/exoscale/cs).

You will need to have [Go](https://golang.org/doc/install) installed before you can use CloudMonkey.

To install CloudMonkey, follow the instructions on the [**Getting Started**](https://github.com/apache/cloudstack-cloudmonkey/wiki/Getting-Started) page of the repository's wiki.

## Configure your CloudMonkey Server Profile

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

# Make API Requests

To make API requests, you must include `cmk`, the command you are using, and any required or optional query parameters you wish to use. Query parameters are passed as `key=value` pairs. For example, to request all of the VM instances owned by the Admin user, enter:

`$ cmk listVirtualMachines domainId=1 account=admin`

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

### Query Parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zone id**|The zone ID for listing hypervisors|uuid|255|false|

### Response Schema

> The above command returns JSON structured like this:

```json
{
  "count": 8,
  "hypervisor": [
    {
      "name": "Hyperv"
    },
    {
      "name": "KVM"
    },
    {
      "name": "XenServer"
    },
    {
      "name": "VMware"
    },
    {
      "name": "BareMetal"
    },
    {
      "name": "Ovm"
    },
    {
      "name": "LXC"
    },
    {
      "name": "Ovm3"
    }
  ]
}
```

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|string|The UUID of the latest async job acting on this object|
|**name**|string|Hypervisor name|
|**jobstatus**|integer|The current status of the latest async job acting on this object|

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

## Configure your CloudMonkey Profile

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

# Autoscale

## deleteAutoscaleVmProfile

This command deletes an autoscale VM profile.

<aside class="notice">
Asynchronous
</aside>

<!--- aside classes are "notice," "warning," and "success" -->

### Command

> The following command deletes an autoscale VM profile:

```shell
$ cmk deleteAutoScaleVmProfile <id>
```

`deleteAutoscaleVmProfile`

### Query Parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the autoscale profile|uuid|255|true|

### Response Schema

> The above command returns JSON structured like this:

```json
{
  "json": "example"
}
```

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

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

# ISO

## copyIso

This command copies an ISO from one zone to another.

<aside class="notice">
Asynchronous
</aside>

### Command

> The following command lists hypervisors:

```shell
$ cmk copyIso <id>
```

`copyIso`

### Query Parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**destzoneid**|ID of the zone the template is being copied to|uuid|255|false|
|**id**|Template ID|uuid|255|true|
|**destzoneids**|A list of IDs of the zones that the template needs to be copied to. Specify this list if the template needs to be copied to multiple zones in one go. Do not specify destzoneid and destzoneids together. However, one of them is required.|list|255|false|
|**sourcezoneid**|ID of the zone the template is currently hosted on. If not specified and template is cross-zone, then we will sync this template to region wide image store.|uuid|255|false|

### Response Schema

> The above command returns JSON structured like this:

```json
{
  "json": "example"
}
```

|Element|Description|Type|
|-------|-----------|----|
|**domain**|The name of the domain to which the template belongs|string|
|**crosszones**|True if the template is managed across all Zones, false otherwise|boolean|
|**ostypename**|The name of the OS type for this template|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**displaytext**|The template display text|string|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**hypervisor**|The hypervisor on which the template runs|string|
|**accountid**|The account ID to which the template belongs|string|
|**passwordenabled**|True if the reset password feature is enabled; false otherwise|boolean|
|**name**|The template name|string|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domainid**|The ID of the domain to which the template belongs|string|
|**templatetag**|The tag of this template|string|
|**ostypeid**|The ID of the OS type for this template|string|
|**removed**|The date this template was removed|date|
|**hostname**|The name of the secondary storage host for the template|string|
|**checksum**|Checksum of the template|string|
|**templatetype**|The type of the template|string|
|**format**|The format of the template|imageformat|
|**physicalsize**|The physical size of the template|long|
|**zonename**|The name of the zone for this template|string|
|**tags**|The list of resource tags associated|set|
|**childtemplates**|If root disk template, then ids of the datas disk templates this template owns|set|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**zoneid**|The ID of the zone for this template|string|
|**status**|The status of the template|string|
|**size**|The size of the template|long|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**created**|The date this template was created|date|
|**details**|Additional key/value details tied with template|map|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM cpu/memory|boolean|
|**projectid**|The project id of the template|string|
|**id**|The template ID|string|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**account**|The account name to which the template belongs|string
|**bits**|The processor bit size|int|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**project**|The project name of the template|string|
|**hostid**|The ID of the secondary storage host for the template|string|

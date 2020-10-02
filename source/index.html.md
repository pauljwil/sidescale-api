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

# Account

## addAccountToProject

This command adds account to a project.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command adds account to a project:

```shell
$ cmk addAccountToProject
```

`addAccountToProject`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|Name of the account to be added to the project|string|255|false|
|**email**|Email to which invitation to the project is going to be sent|string|255|false|
|**projectid**|ID of the project to add the account to|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## deleteAccountFromProject

This command deletes account from the project.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes account from the project:

```shell
$ cmk deleteAccountFromProject
```

`deleteAccountFromProject`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**projectid**|ID of the project to remove the account from|uuid|255|true|
|**account**|Name of the account to be removed from the project|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**success**|True if operation is executed successfully|boolean|

## getSolidFireAccountId

This command gets SolidFire Account ID.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command gets SolidFire Account ID:

```shell
$ cmk getSolidFireAccountId
```

`getSolidFireAccountId`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**accountid**|CloudStack Account UUID|string|255|true|
|**storageid**|Storage Pool UUID|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**solidFireAccountId**|SolidFire Account ID|long|

# Address

## associateIpAddress

This command acquires and associates a public IP to an account.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command acquires and associates a public IP to an account:

```shell
$ cmk associateIpAddress
```

`associateIpAddress`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zoneid**|The ID of the availability zone you want to acquire an public IP address from|uuid|255|false|
|**account**|The account to associate with this IP address|string|255|false|
|**projectid**|Deploy VM for the project|uuid|255|false|
|**domainid**|The ID of the domain to associate with this IP address|uuid|255|false|
|**networkid**|The network this IP address should be associated to|uuid|255|false|
|**isportable**|Should be set to true if public IP is required to be transferable across zones, if not specified defaults to false|boolean|255|false|
|**fordisplay**|An optional field, whether to the display the IP to the end user or not|boolean|255|false|
|**regionid**|Region ID from where portable IP is to be associated|integer|255|false|
|**vpcid**|The VPC you want the IP address to be associated with|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**zoneid**|The ID of the zone the public IP address belongs to|string|
|**physicalnetworkid**|The physical network this belongs to|string|
|**fordisplay**|Is public IP for display to the regular user|boolean|
|**associatednetworkname**|The name of the Network associated with the IP address|string|
|**account**|The account the public IP address is associated with|string|
|**virtualmachinename**|Virtual machine name the ip address is assigned to (not null only for static nat IP)|string|
|**vlanname**|The VLAN associated with the IP address|string|
|**state**|State of the ip address. Can be: Allocating, Allocated and Releasing|string|
|**forvirtualnetwork**|The virtual network for the IP address|boolean|
|**virtualmachinedisplayname**|Virtual machine display name the IP address is assigned to (not null only for static nat IP)|string|
|**associatednetworkid**|The ID of the Network associated with the IP address|string|
|**vmipaddress**|Virtual machine (DNAT) IP address (not null only for static nat IP)|string|
|**ipaddress**|Public IP address|string|
|**project**|The project name of the address|string|
|**allocated**|Date the public IP address was acquired|date|
|**domainid**|The domain ID the public IP address is associated with|string|
|**issourcenat**|True if the IP address is a source nat address, false otherwise|boolean|
|**isstaticnat**|True if this IP is for static nat, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**vlanid**|The ID of the VLAN associated with the IP address. This parameter is visible to ROOT admins only.|string|
|**zonename**|The name of the zone the public IP address belongs to|string|
|**tags**|The list of resource tags associated with IP address|list|
|**id**|Public IP address id|string|
|**networkid**|The ID of the Network where IP belongs to|string|
|**domain**|The domain the public IP address is associated with|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**purpose**|Purpose of the IP address. In Acton this value is not null for IPs with isSystem=true, and can have either StaticNat or LB value.|string|
|**projectid**|The project ID of the IP address|string|
|**virtualmachineid**|Virtual machine ID the IP address is assigned to (not null only for static nat IP)|string|
|**issystem**|True if this IP is system IP (was allocated as a part of deployVm or createLbRule)|boolean|
|**vpcid**|VPC the IP belongs to|string|
|**isportable**|Is public IP portable across the zones|boolean|

## listPublicIpAddresses

This command lists all public IP addresses.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all public IP addresses:

```shell
$ cmk listPublicIpAddresses
```

`listPublicIpAddresses`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**vpcid**|List IPs belonging to the VPC|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**id**|Lists IP address by ID|uuid|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**vlanid**|Lists all public IP addresses by VLAN ID|uuid|255|false|
|**zoneid**|Lists all public IP addresses by zone ID|uuid|255|false|
|**forloadbalancing**|List only IPs used for load balancing|boolean|255|false|
|**state**|Lists all public IP addresses by state|string|255|false|
|**issourcenat**|List only source NAT IP addresses|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**isstaticnat**|List only static NAT IP addresses|boolean|255|false|
|**page**|Page|integer|255|false|
|**keyword**|List by keyword|string|255|false|
|**ipaddress**|Lists the specified IP address|string|255|false|
|**forvirtualnetwork**|The virtual network for the IP address|boolean|255|false|
|**allocatedonly**|Limits search results to allocated public IP addresses|boolean|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**physicalnetworkid**|Lists all public IP addresses by physical network ID|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**associatednetworkid**|Lists all public IP addresses associated to the network specified|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**vlanname**|The VLAN associated with the IP address|string|
|**associatednetworkname**|The name of the Network associated with the IP address|string|
|**zonename**|The name of the zone the public IP address belongs to|string|
|**state**|State of the IP address. Can be: Allocatin, Allocated and Releasing.|string|
|**forvirtualnetwork**|The virtual network for the IP address|boolean|
|**associatednetworkid**|The ID of the Network associated with the IP address|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**ipaddress**|Public IP address|string|
|**projectid**|The project ID of the IP address|string|
|**virtualmachineid**|Virtual machine ID the IP address is assigned to (not null only for static nat IP)|string|
|**issystem**|True if this IP is system IP (was allocated as a part of deployVm or createLbRule)|boolean|
|**issourcenat**|True if the IP address is a source nat address, false otherwise|boolean|
|**vmipaddress**|Virtual machine (DNAT) IP address (not null only for static nat IP)|string|
|**id**|Public IP address ID|string|
|**virtualmachinedisplayname**|Virtual machine display name the IP address is assigned to (not null only for static nat IP)|string|
|**physicalnetworkid**|The physical network this belongs to|string|
|**account**|The account the public IP address is associated with|string|
|**virtualmachinename**|Virtual machine name the IP address is assigned to (not null only for static nat IP)|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**networkid**|The ID of the Network where IP belongs to|string|
|**project**|The project name of the address|string|
|**purpose**|Purpose of the IP address. In Acton this value is not null for IPs with isSystem=true, and can have either StaticNat or LB value|string|
|**isstaticnat**|True if this ip is for static NAT, false otherwise|boolean|
|**domainid**|The domain ID the public IP address is associated with|string|
|**zoneid**|The ID of the zone the public IP address belongs to|string|
|**vpcid**|VPC the IP belongs to|string|
|**tags**|The list of resource tags associated with IP address|list|
|**fordisplay**|Is public IP for display to the regular user|boolean|
|**vlanid**|The ID of the VLAN associated with the IP address. This parameter is visible to ROOT admins only.|string|
|**domain**|The domain the public IP address is associated with|string|
|**allocated**|Date the public IP address was acquired|date|
|**isportable**|Is public IP portable across the zones|boolean|

# Affinity group

## createAffinityGroup

This command creates an affinity/anti-affinity group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an affinity/anti-affinity group:

```shell
$ cmk createAffinityGroup
```

`createAffinityGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**type**|Type of the affinity group from the available affinity/anti-affinity group types|string|255|true|
|**projectid**|Create affinity group for project|uuid|255|false|
|**description**|Optional description of the affinity group|string|255|false|
|**name**|Name of the affinity group|string|255|true|
|**domainid**|DomainId of the account owning the affinity group|uuid|255|false|
|**account**|An account for the affinity group. Must be used with domain ID.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectid**|The project ID of the affinity group|string|
|**project**|The project name of the affinity group|string|
|**domain**|The domain name of the affinity group|string|
|**type**|The type of the affinity group|string|
|**id**|The ID of the affinity group|string|
|**account**|The account owning the affinity group|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**name**|The name of the affinity group|string|
|**description**|The description of the affinity group|string|
|**virtualmachineIds**|Virtual machine IDs associated with this affinity group|list|
|**domainid**|The domain ID of the affinity group|string|

## deleteAffinityGroup

This command deletes affinity group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes affinity group:

```shell
$ cmk deleteAffinityGroup
```

`deleteAffinityGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|The account of the affinity group. Must be specified with domain ID.|string|255|false|
|**domainid**|The domain ID of account owning the affinity group|uuid|255|false|
|**id**|The ID of the affinity group. Mutually exclusive with name parameter.|uuid|255|false|
|**projectid**|The project of the affinity group|uuid|255|false|
|**name**|The name of the affinity group. Mutually exclusive with ID parameter.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listAffinityGroups

This command lists affinity groups.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists affinity groups:

```shell
$ cmk listAffinityGroups
```

`listAffinityGroups`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**pagesize**|Page size|integer|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domainId till leaves|boolean|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**id**|List the affinity group by the ID provided|uuid|255|false|
|**name**|Lists affinity groups by name|string|255|false|
|**keyword**|List by keyword|string|255|false|
|**type**|Lists affinity groups by type|string|255|false|
|**virtualmachineid**|Lists affinity groups by virtual machine ID|uuid|255|false|
|**page**|Page|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**name**|The name of the affinity group|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectid**|The project ID of the affinity group|string|
|**account**|The account owning the affinity group|string|
|**domainid**|The domain ID of the affinity group|string|
|**type**|The type of the affinity group|string|
|**domain**|The domain name of the affinity group|string|
|**virtualmachineIds**|Virtual machine IDs associated with this affinity group|list|
|**project**|The project name of the affinity group|string|
|**id**|The ID of the affinity group|string|
|**description**|The description of the affinity group|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

# Async job

## listAsyncJobs

This command lists all pending asynchronous jobs for the account.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all pending asynchronous jobs for the account:

```shell
$ cmk listAsyncJobs
```

`listAsyncJobs`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**startdate**|The start date of the async job|tzdate|255|false|
|**keyword**|List by keyword|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**page**|Page|integer|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**accountid**|The account that executed the async command|string|
|**jobstatus**|The current job status-should be 0 for PENDING|integer|
|**jobresultcode**|The result code for the job|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobprocstatus**|The progress information of the PENDING job|integer|
|**cmd**|The async command executed|string|
|**created**|The created date of the job|date|
|**jobinstanceid**|The unique ID of the instance/entity object related to the job|string|
|**userid**|The user that executed the async command|string|
|**jobinstancetype**|The instance/entity object related to the job|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobresult**|The result reason|responseobject|
|**jobresulttype**|The result type|string|

# AutoScale

## createAutoScaleVmGroup

This command creates and automatically starts a virtual machine based on a service offering, disk offering, and template.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates and automatically starts a virtual machine based on a service offering, disk offering, and template:

```shell
$ cmk createAutoScaleVmGroup
```

`createAutoScaleVmGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**lbruleid**|The ID of the load balancer rule|uuid|255|true|
|**minmembers**|The minimum number of members in the VM group, the number of instances in the VM group will be equal to or more than this number|integer|255|true|
|**scaledownpolicyids**|List of scaledown autoscale policies|list|255|true|
|**fordisplay**|An optional field, whether to the display the group to the end user or not|boolean|255|false|
|**scaleuppolicyids**|List of scaleup autoscale policies|list|255|true|
|**maxmembers**|The maximum number of members in the VM group, The number of instances in the VM group will be equal to or less than this number|integer|255|true|
|**vmprofileid**|The autoscale profile that contains information about the VMs in the VM group.|uuid|255|true|
|**interval**|The frequency at which the conditions have to be evaluated|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**lbruleid**|The load balancer rule ID|string|
|**domain**|The domain name of the VM profile|string|
|**domainid**|The domain ID of the VM profile|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**maxmembers**|The maximum number of members in the VM group. The number of instances in the VM group will be equal to or less than this number.|int|
|**scaledownpolicies**|List of scaledown autoscale policies|list|
|**projectid**|The project ID VM profile|string|
|**id**|The autoscale VM group ID|string|
|**account**|The account owning the instance group|string|
|**project**|The project name of the VM profile|string|
|**vmprofileid**|The autoscale profile that contains information about the VMs in the VM group.|string|
|**interval**|The frequency at which the conditions have to be evaluated|int|
|**scaleuppolicies**|List of scaleup autoscale policies|list|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**fordisplay**|Is group for display to the regular user|boolean|
|**state**|The current state of the AutoScale VM group|string|
|**minmembers**|The minimum number of members in the VM group. The number of instances in the VM group will be equal to or more than this number.|int|

## createAutoScalePolicy

This command creates an autoscale policy for a provision or deprovision action, the action is taken when the all the conditions evaluates to true for the specified duration. The policy is in effect once it is attached to an autscale VM group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an autoscale policy for a provision or deprovision action:

```shell
$ cmk createAutoScalePolicy
```

`createAutoScalePolicy`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**duration**|The duration for which the conditions have to be true before action is taken|integer|255|true|
|**quiettime**|The cool down period for which the policy should not be evaluated after the action has been taken|integer|255|false|
|**conditionids**|The list of IDs of the conditions that are being evaluated on every interval|list|255|true|
|**action**|The action to be executed if all the conditions evaluate to true for the specified duration|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The project name of the autoscale policy|string|
|**action**|The action to be executed if all the conditions evaluate to true for the specified duration.|string|
|**projectid**|The project ID autoscale policy|string|
|**conditions**|The list of IDs of the conditions that are being evaluated on every interval|list|
|**domain**|The domain name of the autoscale policy|string|
|**domainid**|The domain ID of the autoscale policy|string|
|**id**|The autoscale policy ID|string|
|**quiettime**|The cool down period for which the policy should not be evaluated after the action has been taken|integer|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**account**|The account owning the autoscale policy|string|
|**duration**|The duration for which the conditions have to be true before action is taken|integer|

## deleteAutoScalePolicy

This command deletes an autoscale policy.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes an autoscale policy:

```shell
$ cmk deleteAutoScalePolicy
```

`deleteAutoScalePolicy`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the autoscale policy|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**success**|True if operation is executed successfully|boolean|

## deleteAutoScaleVmProfile

This command deletes an autoscale VM profile.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes an autoscale VM profile:

```shell
$ cmk deleteAutoScaleVmProfile
```

`deleteAutoScaleVmProfile`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the autoscale profile|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## deleteCondition

This command removes a condition.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command removes a condition:

```shell
$ cmk deleteCondition
```

`deleteCondition`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the condition|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## disableAutoScaleVmGroup

This command disables an AutoScale VM Group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command disables an AutoScale VM Group:

```shell
$ cmk disableAutoScaleVmGroup
```

`disableAutoScaleVmGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the autoscale group|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The autoscale VM group ID|string|
|**domain**|The domain name of the VM profile|string|
|**interval**|The frequency at which the conditions have to be evaluated|int|
|**scaleuppolicies**|List of scaleup autoscale policies|list|
|**scaledownpolicies**|List of scaledown autoscale policies|list|
|**lbruleid**|The load balancer rule ID|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**maxmembers**|The maximum number of members in the VM group. The number of instances in the VM group will be equal to or less than this number.|int|
|**domainid**|The domain ID of the vm profile|string|
|**state**|The current state of the AutoScale VM Group|string|
|**minmembers**|The minimum number of members in the VM group. The number of instances in the VM group will be equal to or more than this number.|int|
|**account**|The account owning the instance group|string|
|**fordisplay**|Is group for display to the regular user|boolean|
|**project**|The project name of the VM profile|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectid**|The project ID VM profile|string|
|**vmprofileid**|The autoscale profile that contains information about the VMs in the VM group|string|

## enableAutoScaleVmGroup

This command enables an AutoScale VM Group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command enables an AutoScale VM Group:

```shell
$ cmk enableAutoScaleVmGroup
```

`enableAutoScaleVmGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the autoscale group|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**projectid**|The project ID VM profile|string|
|**scaleuppolicies**|List of scaleup autoscale policies|list|
|**account**|The account owning the instance group|string|
|**domainid**|The domain ID of the VM profile|string|
|**maxmembers**|The maximum number of members in the VM group. The number of instances in the vm group will be equal to or less than this number.|int|
|**fordisplay**|Is group for display to the regular user|boolean|
|**domain**|The domain name of the VM profile|string|
|**interval**|The frequency at which the conditions have to be evaluated|int|
|**id**|The autoscale VM group ID|string|
|**state**|The current state of the AutoScale VM Group|string|
|**project**|The project name of the VM profile|string|
|**minmembers**|The minimum number of members in the VM group. The number of instances in the VM group will be equal to or more than this number.|int|
|**lbruleid**|The load balancer rule ID|string|
|**scaledownpolicies**|List of scaledown autoscale policies|list|
|**vmprofileid**|The autoscale profile that contains information about the VMs in the VM group|string|

## listConditions

This command lists Conditions for the specific user.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists Conditions for the specific user:

```shell
$ cmk listConditions
```

`listConditions`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**pagesize**|Page size|integer|255|false|
|**policyid**|The ID of the policy|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false|boolean|255|false|
|**counterid**|Counter-ID of the condition.|uuid|255|false|
|**page**|Page|integer|255|false|
|**account**|List resources by account. Must be used with the domainId parameter.|string|255|false|
|**id**|ID of the Condition|uuid|255|false|
|**keyword**|List by keyword|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**projectid**|The project ID of the Condition|string|
|**account**|The owner of the Condition|string|
|**zoneid**|Zone ID of counter|string|
|**domainid**|The domain ID of the Condition owner|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**counter**|Details of the Counter|list|
|**domain**|The domain name of the owner|string|
|**threshold**|Threshold Value for the counter|long|
|**id**|The ID of the Condition|string|
|**project**|The project name of the Condition|string|
|**relationaloperator**|Relational Operator to be used with threshold|string|

## updateAutoScaleVmGroup

This command updates an existing autoscale VM group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates an existing autoscale VM group:

```shell
$ cmk updateAutoScaleVmGroup
```

`updateAutoScaleVmGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**scaledownpolicyids**|List of scaledown autoscale policies|list|255|false|
|**interval**|The frequency at which the conditions have to be evaluated|integer|255|false|
|**maxmembers**|The maximum number of members in the VM group. The number of instances in the VM group will be equal to or less than this number.|integer|255|false|
|**minmembers**|The minimum number of members in the VM group, the number of instances in the VM group will be equal to or more than this number.|integer|255|false|
|**scaleuppolicyids**|List of scaleup autoscale policies|list|255|false|
|**fordisplay**|An optional field, whether to the display the group to the end user or not|boolean|255|false|
|**id**|The ID of the autoscale group|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**scaleuppolicies**|List of scaleup autoscale policies|list|
|**account**|The account owning the instance group|string|
|**minmembers**|The minimum number of members in the VM group. The number of instances in the VM group will be equal to or more than this number.|int|
|**maxmembers**|The maximum number of members in the VM group. The number of instances in the VM group will be equal to or less than this number.|int|
|**project**|The project name of the VM profile|string|
|**vmprofileid**|The autoscale profile that contains information about the VMs in the VM group|string|
|**lbruleid**|The load balancer rule ID|string|
|**id**|The autoscale VM group ID|string|
|**scaledownpolicies**|List of scaledown autoscale policies|list|
|**interval**|The frequency at which the conditions have to be evaluated|int|
|**fordisplay**|Is group for display to the regular user|boolean|
|**projectid**|The project ID VM profile|string|
|**domain**|The domain name of the VM profile|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domainid**|The domain ID of the VM profile|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**state**|The current state of the AutoScale VM Group|string|

## updateAutoScaleVmProfile

This command updates an existing autoscale VM profile.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates an existing autoscale VM profile:

```shell
$ cmk updateAutoScaleVmProfile
```

`updateAutoScaleVmProfile`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**templateid**|The template of the auto deployed virtual machine|uuid|255|false|
|**autoscaleuserid**|The ID of the user used to launch and destroy the VMs|uuid|255|false|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**counterparam**|Counterparam list. Example: counterparam[0].name=snmpcommunity&counterparam[0].value=public&counterparam[1].name=snmpport&counterparam[1].value=161|map|255|false|
|**fordisplay**|An optional field, whether to the display the profile to the end user or not|boolean|255|false|
|**id**|The ID of the autoscale VM profile|uuid|255|true|
|**destroyvmgraceperiod**|The time allowed for existing connections to get closed before a VM is destroyed|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**projectid**|The project ID VM profile|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the VM profile|string|
|**domainid**|The domain ID of the VM profile|string|
|**id**|The autoscale VM profile ID|string|
|**templateid**|The template to be used while deploying a virtual machine|string|
|**account**|The account owning the instance group|string|
|**domain**|The domain name of the VM profile|string|
|**fordisplay**|Is profile for display to the regular user|boolean|
|**destroyvmgraceperiod**|The time allowed for existing connections to get closed before a VM is destroyed|integer|
|**autoscaleuserid**|The ID of the user used to launch and destroy the VMs|string|
|**otherdeployparams**|Parameters other than zoneId/serviceOfferringId/templateId to be used while deploying a virtual machine|string|
|**zoneid**|The availability zone to be used while deploying a virtual machine|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**serviceofferingid**|The service offering to be used while deploying a virtual machine|string|

# Baremetal

## notifyBaremetalProvisionDone

This command notifies provision has been done on a host. This API is for baremetal virtual router service, not for end user.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command notifies provision has been done on a host:

```shell
$ cmk notifyBaremetalProvisionDone
```

`notifyBaremetalProvisionDone`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**mac**|Mac of the nic used for provision|object|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

# Configuration

## listCapabilities

This command lists capabilities.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists capabilities:

```shell
$ cmk listCapabilities
```

`listCapabilities`

### Query parameters

There are no query parameters available for this command.

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**dynamicrolesenabled**|True if dynamic role-based API checker is enabled, false otherwise|boolean|
|**customdiskofferingminsize**|Minimum size that can be specified when create disk from disk offering with custom size|long|
|**regionsecondaryenabled**|True if region wide secondary is enabled, false otherwise|boolean|
|**apilimitmax**|Max allowed number of API requests within the specified interval|integer|
|**securitygroupsenabled**|True if security groups support is enabled, false otherwise|boolean|
|**kvmsnapshotenabled**|True if snapshot is supported for KVM host, false otherwise|boolean|
|**apilimitinterval**|Time interval (in seconds) to reset API count|integer|
|**projectinviterequired**|If invitation confirmation is required when add account to project|boolean|
|**cloudstackversion**|Version of the cloud stack|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**userpublictemplateenabled**|True if user and domain admins can set templates to be shared, false otherwise|boolean|
|**supportELB**|True if region supports elastic load balancer on basic zones|string|
|**allowusercreateprojects**|True if regular user is allowed to create projects|boolean|
|**customdiskofferingmaxsize**|Maximum size that can be specified when create disk from disk offering with custom size|long|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**allowuserexpungerecovervm**|True if the user can recover and expunge virtual machines, false otherwise|boolean|
|**allowuserviewdestroyedvm**|True if the user is allowed to view destroyed virtual machines, false otherwise|boolean|

## listConfigurations

This command lists all configurations.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all configurations:

```shell
$ cmk listConfigurations
```

`listConfigurations`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**imagestoreuuid**|The ID of the Image Store to update the parameter value for corresponding image store|uuid|255|false|
|**clusterid**|The ID of the Cluster to update the parameter value for corresponding cluster|uuid|255|false|
|**storageid**|The ID of the Storage pool to update the parameter value for corresponding storage pool|uuid|255|false|
|**zoneid**|The ID of the Zone to update the parameter value for corresponding zone|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**category**|Lists configurations by category|string|255|false|
|**name**|Lists configuration by name|string|255|false|
|**keyword**|List by keyword|string|255|false|
|**accountid**|The ID of the Account to update the parameter value for corresponding account|uuid|255|false|
|**page**|Page|integer|255|false|
|**domainid**|The ID of the Domain to update the parameter value for corresponding domain|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**scope**|Scope(zone/cluster/pool/account) of the parameter that needs to be updated|string|
|**id**|The value of the configuration|long|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**value**|The value of the configuration|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**name**|The name of the configuration|string|
|**description**|The description of the configuration|string|
|**category**|The category of the configuration|string|

# Event

## archiveEvents

This command archives one or more events.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command archives one or more events:

```shell
$ cmk archiveEvents
```

`archiveEvents`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ids**|The IDs of the events|list|255|false|
|**enddate**|End date range to archive events (including) this date (use format "yyyy-MM-dd" or the new format "yyyy-MM-ddThh:mm:ss")|date|255|false|
|**startdate**|Start date range to archive events (including) this date (use format "yyyy-MM-dd" or the new format "yyyy-MM-ddThh:mm:ss")|date|255|false|
|**type**|Archive by event type|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## deleteEvents

This command deletes one or more events.

<aside class='notice'>
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
|**enddate**|End date range to delete events (including) this date (use format "yyyy-MM-dd" or the new format "yyyy-MM-ddThh:mm:ss")|date|255|false|
|**startdate**|Start date range to delete events (including) this date (use format "yyyy-MM-dd" or the new format "yyyy-MM-ddThh:mm:ss")|date|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|

# Firewall

## createEgressFirewallRule

This command creates an egress firewall rule for a given network.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an egress firewall rule for a given network:

```shell
$ cmk createEgressFirewallRule
```

`createEgressFirewallRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**startport**|The starting port of firewall rule|integer|255|false|
|**icmptype**|Type of the ICMP message being sent|integer|255|false|
|**endport**|The ending port of firewall rule|integer|255|false|
|**icmpcode**|Error code for this ICMP message|integer|255|false|
|**cidrlist**|The CIDR list to forward traffic from|list|255|false|
|**protocol**|The protocol for the firewall rule. Valid values are TCP/UDP/ICMP.|string|255|true|
|**type**|Type of firewall rule: system/user|string|255|false|
|**destcidrlist**|The CIDR list to forward traffic to|list|255|false|
|**networkid**|The network ID of the port forwarding rule|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**ipaddressid**|The public IP address ID for the firewall rule|string|
|**destcidrlist**|The CIDR list to forward traffic to|string|
|**tags**|The list of resource tags associated with the rule|list|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**ipaddress**|The public IP address for the firewall rule|string|
|**endport**|The ending port of firewall rule's port range|integer|
|**networkid**|The network ID of the firewall rule|string|
|**startport**|The starting port of firewall rule's port range|integer|
|**icmptype**|Type of the ICMP message being sent|integer|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**id**|The ID of the firewall rule|string|
|**icmpcode**|Error code for this ICMP message|integer|
|**state**|The state of the rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**protocol**|The protocol of the firewall rule|string|
|**fordisplay**|Is rule for display to the regular user|boolean|

## createPortForwardingRule

This command creates a port forwarding rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a port forwarding rule:

```shell
$ cmk createPortForwardingRule
```

`createPortForwardingRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**cidrlist**|The CIDR list to forward traffic from|list|255|false|
|**openfirewall**|If true, firewall rule for source/end public port is automatically created; if false - firewall rule has to be created explicitly. If not specified 1) defaulted to false when PF rule is being created for VPC guest network 2) in all other cases defaulted to true.|boolean|255|false|
|**privateendport**|The ending port of port forwarding rule's private port range|integer|255|false|
|**publicport**|The starting port of port forwarding rule's public port range|integer|255|true|
|**ipaddressid**|The IP address ID of the port forwarding rule|uuid|255|true|
|**privateport**|The starting port of port forwarding rule's private port range|integer|255|true|
|**publicendport**|The ending port of port forwarding rule's private port range|integer|255|false|
|**virtualmachineid**|The ID of the virtual machine for the port forwarding rule|uuid|255|true|
|**protocol**|The protocol for the port forwarding rule. Valid values are TCP or UDP.|string|255|true|
|**networkid**|The network of the virtual machine the port forwarding rule will be created for. Required when public IP address is not associated with any guest network yet (VPC case).|uuid|255|false|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**vmguestip**|VM guest nic secondary IP address for the port forwarding rule|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**networkid**|The ID of the guest network the port forwarding rule belongs to|string|
|**privateendport**|The ending port of port forwarding rule's private port range|string|
|**virtualmachinedisplayname**|The VM display name for the port forwarding rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**protocol**|The protocol of the port forwarding rule|string|
|**publicport**|The starting port of port forwarding rule's public port range|string|
|**publicendport**|The ending port of port forwarding rule's private port range|string|
|**ipaddress**|The public IP address for the port forwarding rule|string|
|**id**|The ID of the port forwarding rule|string|
|**virtualmachineid**|The VM ID for the port forwarding rule|string|
|**privateport**|The starting port of port forwarding rule's private port range|string|
|**fordisplay**|Is firewall for display to the regular user|boolean|
|**state**|The state of the rule|string|
|**vmguestip**|The VM IP address for the port forwarding rule|string|
|**ipaddressid**|The public IP address ID for the port forwarding rule|string|
|**virtualmachinename**|The VM name for the port forwarding rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**tags**|The list of resource tags associated with the rule|list|
|**cidrlist**|The cidr list to forward traffic from|string|

## deleteEgressFirewallRule

This command deletes an egress firewall rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes an egress firewall rule:

```shell
$ cmk deleteEgressFirewallRule
```

`deleteEgressFirewallRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the firewall rule|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listEgressFirewallRules

This command lists all egress firewall rules for network ID.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all egress firewall rules for network ID:

```shell
$ cmk listEgressFirewallRules
```

`listEgressFirewallRules`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**pagesize**|Page size|integer|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**id**|Lists rule with the specified ID|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**keyword**|List by keyword|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**page**|Page|integer|255|false|
|**networkid**|The network ID for the egress firewall services|uuid|255|false|
|**ipaddressid**|The ID of IP address of the firewall services|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**icmptype**|Type of the ICMP message being sent|integer|
|**state**|The state of the rule|string|
|**destcidrlist**|The CIDR list to forward traffic to|string|
|**id**|The ID of the firewall rule|string|
|**endport**|The ending port of firewall rule's port range|integer|
|**networkid**|The network ID of the firewall rule|string|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**protocol**|The protocol of the firewall rule|string|
|**ipaddress**|The public IP address for the firewall rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**startport**|The starting port of firewall rule's port range|integer|
|**tags**|The list of resource tags associated with the rule|list|
|**ipaddressid**|The public IP address ID for the firewall rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**icmpcode**|Error code for this ICMP message|integer|

## listFirewallRules

This command lists all firewall rules for an IP address.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all firewall rules for an IP address:

```shell
$ cmk listFirewallRules
```

`listFirewallRules`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**ipaddressid**|The ID of IP address of the firewall services|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**networkid**|List firewall rules for certain network|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**id**|Lists rule with the specified ID.|uuid|255|false|
|**page**|Page|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**ipaddressid**|The public IP address ID for the firewall rule|string|
|**endport**|The ending port of firewall rule's port range|integer|
|**startport**|The starting port of firewall rule's port range|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**networkid**|The network ID of the firewall rule|string|
|**protocol**|The protocol of the firewall rule|string|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**state**|The state of the rule|string|
|**destcidrlist**|The CIDR list to forward traffic to|string|
|**icmpcode**|Error code for this ICMP message|integer|
|**icmptype**|Type of the ICMP message being sent|integer|
|**ipaddress**|The public IP address for the firewall rule|string|
|**tags**|The list of resource tags associated with the rule|list|
|**id**|The ID of the firewall rule|string|

## listPortForwardingRules

This command lists all port forwarding rules for an IP address.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all port forwarding rules for an IP address:

```shell
$ cmk listPortForwardingRules
```

`listPortForwardingRules`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**networkid**|List port forwarding rules for certain network|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**id**|Lists rule with the specified ID|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**page**|Page|integer|255|false|
|**ipaddressid**|The ID of IP address of the port forwarding services|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**virtualmachineid**|The VM ID for the port forwarding rule|string|
|**publicport**|The starting port of port forwarding rule's public port range|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**protocol**|The protocol of the port forwarding rule|string|
|**id**|The ID of the port forwarding rule|string|
|**privateendport**|The ending port of port forwarding rule's private port range|string|
|**vmguestip**|The VM IP address for the port forwarding rule|string|
|**tags**|The list of resource tags associated with the rule|list|
|**networkid**|The ID of the guest network the port forwarding rule belongs to|string|
|**virtualmachinedisplayname**|The VM display name for the port forwarding rule|string|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**publicendport**|The ending port of port forwarding rule's private port range|string|
|**virtualmachinename**|The VM name for the port forwarding rule|string|
|**ipaddress**|The public IP address for the port forwarding rule|string|
|**state**|The state of the rule|string|
|**ipaddressid**|The public IP address ID for the port forwarding rule|string|
|**fordisplay**|Is firewall for display to the regular user|boolean|
|**privateport**|The starting port of port forwarding rule's private port range|string|

## updateFirewallRule

This command updates firewall rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates firewall rule:

```shell
$ cmk updateFirewallRule
```

`updateFirewallRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**id**|The ID of the firewall rule|uuid|255|true|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The ID of the firewall rule|string|
|**networkid**|The network ID of the firewall rule|string|
|**ipaddress**|The public IP address for the firewall rule|string|
|**protocol**|The protocol of the firewall rule|string|
|**icmpcode**|Error code for this ICMP message|integer|
|**tags**|The list of resource tags associated with the rule|list|
|**endport**|The ending port of firewall rule's port range|integer|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**icmptype**|Type of the ICMP message being sent|integer|
|**ipaddressid**|The public IP address ID for the firewall rule|string|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**destcidrlist**|The CIDR list to forward traffic to|string|
|**state**|The state of the rule|string|
|**startport**|The starting port of firewall rule's port range|integer|

## updatePortForwardingRule

This command updates a port forwarding rule. Only the private port and the virtual machine can be updated.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates a port forwarding rule:

```shell
$ cmk updatePortForwardingRule
```

`updatePortForwardingRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**id**|The ID of the port forwarding rule|uuid|255|true|
|**privateendport**|The private end port of the port forwarding rule|integer|255|false|
|**privateport**|The private start port of the port forwarding rule|integer|255|false|
|**virtualmachineid**|The ID of the virtual machine for the port forwarding rule|uuid|255|false|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**vmguestip**|VM guest nic Secondary IP address for the port forwarding rule|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**privateport**|The starting port of port forwarding rule's private port range|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**fordisplay**|Is firewall for display to the regular user|boolean|
|**protocol**|The protocol of the port forwarding rule|string|
|**publicport**|The starting port of port forwarding rule's public port range|string|
|**state**|The state of the rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**virtualmachineid**|The VM ID for the port forwarding rule|string|
|**tags**|The list of resource tags associated with the rule|list|
|**publicendport**|The ending port of port forwarding rule's private port range|string|
|**networkid**|The ID of the guest network the port forwarding rule belongs to|string|
|**id**|The ID of the port forwarding rule|string|
|**privateendport**|The ending port of port forwarding rule's private port range|string|
|**ipaddress**|The public IP address for the port forwarding rule|string|
|**vmguestip**|The VM IP address for the port forwarding rule|string|
|**ipaddressid**|The public IP address ID for the port forwarding rule|string|
|**virtualmachinedisplayname**|The VM display name for the port forwarding rule|string|
|**virtualmachinename**|The VM name for the port forwarding rule|string|
|**cidrlist**|The CIDR list to forward traffic from|string|

# Guest OS

## listOsTypes

This command lists all supported OS types for this cloud.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all supported OS types for this cloud:

```shell
$ cmk listOsTypes
```

`listOsTypes`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|List by OS type ID|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**oscategoryid**|List by OS Category ID|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**description**|List OS by description|string|255|false|
|**page**|Page|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**isuserdefined**|Is the guest OS user defined|boolean|
|**oscategoryid**|The ID of the OS category|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**id**|The ID of the OS type|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**description**|The name/description of the OS type|string|

# Hypervisor

## listHypervisors

This command lists hypervisors.

<aside class='notice'>
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
|**zoneid**|The zone ID for listing hypervisors|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**name**|Hypervisor name|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

# ISO

## copyIso

This command copies an ISO from one zone to another.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command copies an ISO from one zone to another:

```shell
$ cmk copyIso
```

`copyIso`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**destzoneid**|ID of the zone the template is being copied to|uuid|255|false|
|**id**|Template ID|uuid|255|true|
|**destzoneids**|A list of IDs of the zones that the template needs to be copied to. Specify this list if the template needs to copied to multiple zones in one go. Do not specify destzoneid and destzoneids together, however one of them is required.|list|255|false|
|**sourcezoneid**|ID of the zone the template is currently hosted on. If not specified and template is cross-zone, then we will sync this template to region wide image store.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**domain**|The name of the domain to which the template belongs|string|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**ostypename**|The name of the OS type for this template|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**displaytext**|The template display text|string|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**hypervisor**|The hypervisor on which the template runs|string|
|**accountid**|The account ID to which the template belongs|string|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|
|**name**|The template name|string|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domainid**|The ID of the domain to which the template belongs|string|
|**templatetag**|The tag of this template|string|
|**ostypeid**|The ID of the OS type for this template.|string|
|**removed**|The date this template was removed|date|
|**hostname**|The name of the secondary storage host for the template|string|
|**checksum**|Checksum of the template|string|
|**templatetype**|The type of the template|string|
|**format**|The format of the template|imageformat|
|**physicalsize**|The physical size of the template|long|
|**zonename**|The name of the zone for this template|string|
|**tags**|The list of resource tags associated|set|
|**childtemplates**|If root disk template, then IDs of the datas disk templates this template owns|set|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**zoneid**|The ID of the zone for this template|string|
|**status**|The status of the template|string|
|**size**|The size of the template|long|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**created**|The date this template was created|date|
|**details**|Additional key/value details tied with template|map|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**projectid**|The project ID of the template|string|
|**id**|The template ID|string|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**account**|The account name to which the template belongs|string|
|**bits**|The processor bit size|int|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**project**|The project name of the template|string|
|**hostid**|The ID of the secondary storage host for the template|string|

## deleteIso

This command deletes an ISO file.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes an ISO file:

```shell
$ cmk deleteIso
```

`deleteIso`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the ISO file|uuid|255|true|
|**zoneid**|The ID of the zone of the ISO file. If not specified, the ISO will be deleted from all the zones.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## detachIso

This command detaches any ISO file (if any) currently attached to a virtual machine.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command detaches any ISO file (if any) currently attached to a virtual machine:

```shell
$ cmk detachIso
```

`detachIso`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**virtualmachineid**|The ID of the virtual machine|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**rootdeviceid**|Device ID of the root volume|long|
|**name**|The name of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**memorykbs**|The memory used by the VM|long|
|**hostid**|The ID of the host for the virtual machine|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**hostname**|The name of the host for the virtual machine|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**cpuspeed**|The speed of each CPU|integer|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**guestosid**|OS type ID of the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**keypair**|SSH key-pair|string|
|**id**|The ID of the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**username**|The user's name who deployed the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**password**|The password (if exists) of the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**ostypeid**|OS type ID of the VM|string|
|**details**|VM details in key/value pairs|map|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**nic**|The list of nics associated with VM|set|
|**projectid**|The project ID of the VM|string|
|**tags**|The list of resource tags associated|set|
|**diskioread**|The read (io) of disk on the VM|long|
|**created**|The date when this virtual machine was created|date|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the VM|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**templatedisplaytext**| an alternate display text of the template for the virtual machine|string|
|**memorytargetkbs**|The target memory in VM|long|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**state**|The state of the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**templatename**|The name of the template for the virtual machine|string|
|**group**|The group name of the virtual machine|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**groupid**|The group ID of the virtual machine|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|

## extractIso

This command extracts an ISO.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command extracts an ISO:

```shell
$ cmk extractIso
```

`extractIso`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zoneid**|The ID of the zone where the ISO is originally located|uuid|255|false|
|**url**|The URL to which the ISO would be extracted|string|2048|false|
|**id**|The ID of the ISO file|uuid|255|true|
|**mode**|The mode of extraction - HTTP_DOWNLOAD or FTP_UPLOAD|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**extractId**|The upload ID of extracted object|string|
|**id**|The ID of extracted object|string|
|**storagetype**|Type of the storage|string|
|**zoneid**|Zone ID the object was extracted from|string|
|**status**|The status of the extraction|string|
|**state**|The state of the extracted object|string|
|**accountid**|The account ID to which the extracted object belongs|string|
|**extractMode**|The mode of extraction - upload or download|string|
|**name**|The name of the extracted object|string|
|**resultstring**|Result string|string|
|**uploadpercentage**|The percentage of the entity uploaded to the specified location|integer|
|**zonename**|Zone name the object was extracted from|string|
|**created**|The time and date the object was created|date|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**url**|If mode = upload then url of the uploaded entity. if mode = download the URL from which the entity can be downloaded.|string|

## updateIso

This command updates an ISO file.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command updates an ISO file:

```shell
$ cmk updateIso
```

`updateIso`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**displaytext**|The display text of the image|string|4096|false|
|**bootable**|True if image is bootable, false otherwise; available only for updateIso API|boolean|255|false|
|**isrouting**|True if the template type is routing i.e., if template is used to deploy router|boolean|255|false|
|**passwordenabled**|True if the image supports the password reset feature; default is false|boolean|255|false|
|**sortkey**|Sort key of the template, integer|integer|255|false|
|**requireshvm**|True if the template requires HVM, false otherwise; available only for updateTemplate API|boolean|255|false|
|**ostypeid**|The ID of the OS type that best represents the OS of this image|uuid|255|false|
|**cleanupdetails**|Optional boolean field, which indicates if details should be cleaned up or not (if set to true, details removed for this resource, details field ignored; if false or not set, no action)|boolean|255|false|
|**name**|The name of the image file|string|255|false|
|**id**|The ID of the image file|uuid|255|true|
|**isdynamicallyscalable**|True if template/ISO contains XS/VMWare tools inorder to support dynamic scaling of VM CPU/memory|boolean|255|false|
|**format**|The format for the image|string|255|false|
|**details**|Details in key/value pairs using format details[i].keyname=keyvalue. Example: details[0].hypervisortoolsversion=xenserver61|map|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**tags**|The list of resource tags associated|set|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**zonename**|The name of the zone for this template|string|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**displaytext**|The template display text|string|
|**domainid**|The ID of the domain to which the template belongs|string|
|**created**|The date this template was created|date|
|**status**|The status of the template|string|
|**templatetag**|The tag of this template|string|
|**hostid**|The ID of the secondary storage host for the template|string|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**format**|The format of the template|imageformat|
|**removed**|The date this template was removed|date|
|**details**|Additional key/value details tied with template|map|
|**ostypeid**|The ID of the OS type for this template|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**account**|The account name to which the template belongs|string|
|**hostname**|The name of the secondary storage host for the template|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**accountid**|The account ID to which the template belongs|string|
|**name**|The template name|string|
|**bits**|The processor bit size|int|
|**childtemplates**|If root disk template, then IDs of the datas disk templates this template owns|set|
|**project**|The project name of the template|string|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|
|**projectid**|The project ID of the template|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**ostypename**|The name of the OS type for this template.|string|
|**templatetype**|The type of the template|string|
|**id**|The template ID|string|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**zoneid**|The ID of the zone for this template|string|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**checksum**|Checksum of the template|string|
|**size**|The size of the template|long|
|**physicalsize**|The physical size of the template|long|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**domain**|The name of the domain to which the template belongs|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|

# LDAP

## listLdapConfigurations

This command lists all LDAP configurations.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all LDAP configurations:

```shell
$ cmk listLdapConfigurations
```

`listLdapConfigurations`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**keyword**|List by keyword|string|255|false|
|**port**|Port|integer|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|Linked domain|uuid|255|false|
|**hostname**|Hostname|string|255|false|
|**page**|Page|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**hostname**|Name of the host running the ldap server|string|
|**port**|Port the LDAP server is running on|int|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**domainid**|Linked domain|string|

# Load balancer

## assignCertToLoadBalancer

This command assigns a certificate to a load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command assigns a certificate to a load balancer rule:

```shell
$ cmk assignCertToLoadBalancer
```

`assignCertToLoadBalancer`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**lbruleid**|The ID of the load balancer rule|uuid|255|true|
|**certid**|The ID of the certificate|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## assignToLoadBalancerRule

This command assigns virtual machine or a list of virtual machines to a load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command assigns virtual machine or a list of virtual machines to a load balancer rule:

```shell
$ cmk assignToLoadBalancerRule
```

`assignToLoadBalancerRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the load balancer rule|uuid|255|true|
|**vmidipmap**|VM ID and IP map, vmidipmap[0].vmid=1 vmidipmap[0].ip=10.1.1.75|map|255|false|
|**virtualmachineids**|The list of IDs of the virtual machine that are being assigned to the load balancer rule(i.e. virtualMachineIds=1,2,3)|list|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

## createGlobalLoadBalancerRule

This command creates a global load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a global load balancer rule:

```shell
$ cmk createGlobalLoadBalancerRule
```

`createGlobalLoadBalancerRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|The domain ID associated with the load balancer|uuid|255|false|
|**gslbstickysessionmethodname**|Session sticky method (sourceip) if not specified defaults to sourceip|string|255|false|
|**description**|The description of the load balancer rule|string|4096|false|
|**regionid**|Region where the global load balancer is going to be created|integer|255|true|
|**gslbdomainname**|Domain name for the GSLB service|string|255|true|
|**gslblbmethod**|Load balancer algorithm (roundrobin, leastconn, proximity) that method is used to distribute traffic across the zones participating in global server load balancing, if not specified defaults to 'round robin'|string|255|false|
|**name**|Name of the load balancer rule|string|255|true|
|**account**|The account associated with the global load balancer. Must be used with the domain ID parameter.|string|255|false|
|**gslbservicetype**|GSLB service type (tcp, udp, http)|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**gslbservicetype**|GSLB service type|string|
|**gslbstickysessionmethodname**|Session persistence method used for the global load balancer|string|
|**name**|Name of the global load balancer rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**projectid**|The project ID of the load balancer|string|
|**gslblbmethod**|Load balancing method used for the global load balancer|string|
|**gslbdomainname**|DNS domain name given for the global load balancer|string|
|**id**|Global load balancer rule ID|string|
|**regionid**|Region Id in which global load balancer is created|integer|
|**account**|The account of the load balancer rule|string|
|**domain**|The domain of the load balancer rule|string|
|**loadbalancerrule**|List of load balancer rules that are part of GSLB rule|list|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The project name of the load balancer|string|
|**domainid**|The domain ID of the load balancer rule|string|
|**description**|The description of the global load balancer rule|string|

## createLoadBalancerRule

This command creates a load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a load balancer rule:

```shell
$ cmk createLoadBalancerRule
```

`createLoadBalancerRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**cidrlist**|The CIDR list to forward traffic from|list|255|false|
|**protocol**|The protocol for the LB|string|255|false|
|**algorithm**|Load balancer algorithm (source, roundrobin, leastconn)|string|255|true|
|**name**|Name of the load balancer rule|string|255|true|
|**openfirewall**|If true, firewall rule for source/end public port is automatically created; if false - firewall rule has to be created explicitely. If not specified 1) defaulted to false when LB rule is being created for VPC guest network 2) in all other cases defaulted to true.|boolean|255|false|
|**publicipid**|Public IP address ID from where the network traffic will be load balanced from|uuid|255|false|
|**networkid**|The guest network this rule will be created for. Required when public IP address is not associated with any Guest network yet (VPC case)|uuid|255|false|
|**description**|The description of the load balancer rule|string|4096|false|
|**privateport**|The private port of the private IP address/virtual machine where the network traffic will be load balanced to|integer|255|true|
|**zoneid**|Zone where the load balancer is going to be created. This parameter is required when LB service provider is ElasticLoadBalancerVm|uuid|255|false|
|**account**|The account associated with the load balancer. Must be used with the domain ID parameter.|string|255|false|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**domainid**|The domain ID associated with the load balancer|uuid|255|false|
|**publicport**|The public port from where the network traffic will be load balanced from|integer|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**publicport**|The public port|string|
|**publicip**|The public IP address|string|
|**name**|The name of the load balancer|string|
|**privateport**|The private port|string|
|**zoneid**|The ID of the zone the rule belongs to|string|
|**description**|The description of the load balancer|string|
|**domainid**|The domain ID of the load balancer rule|string|
|**networkid**|The ID of the guest network the LB rule belongs to|string|
|**account**|The account of the load balancer rule|string|
|**project**|The project name of the load balancer|string|
|**tags**|The list of resource tags associated with load balancer|list|
|**state**|The state of the rule|string|
|**domain**|The domain of the load balancer rule|string|
|**id**|The load balancer rule ID|string|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**projectid**|The project ID of the load balancer|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**algorithm**|The load balancer algorithm (source, roundrobin, leastconn)|string|
|**zonename**|The name of the zone the load balancer rule belongs to|string|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**protocol**|The protocol of the loadbalanacer rule|string|
|**publicipid**|The public IP address ID|string|

## deleteLoadBalancerRule

This command deletes a load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a load balancer rule:

```shell
$ cmk deleteLoadBalancerRule
```

`deleteLoadBalancerRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the load balancer rule|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## listGlobalLoadBalancerRules

This command lists load balancer rules.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists load balancer rules:

```shell
$ cmk listGlobalLoadBalancerRules
```

`listGlobalLoadBalancerRules`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**page**|Page|integer|255|false|
|**keyword**|List by keyword|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**id**|The ID of the global load balancer rule|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**regionid**|Region ID|integer|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**gslblbmethod**|Load balancing method used for the global load balancer|string|
|**gslbdomainname**|DNS domain name given for the global load balancer|string|
|**projectid**|The project ID of the load balancer|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**loadbalancerrule**|List of load balancer rules that are part of GSLB rule|list|
|**account**|The account of the load balancer rule|string|
|**gslbservicetype**|GSLB service type|string|
|**regionid**|Region ID in which global load balancer is created|integer|
|**id**|Global load balancer rule ID|string|
|**name**|Name of the global load balancer rule|string|
|**gslbstickysessionmethodname**|Session persistence method used for the global load balancer|string|
|**domainid**|The domain ID of the load balancer rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**description**|The description of the global load balancer rule|string|
|**domain**|The domain of the load balancer rule|string|
|**project**|The project name of the load balancer|string|

## listLoadBalancerRuleInstances

This command lists all virtual machine instances that are assigned to a load balancer rule.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all virtual machine instances that are assigned to a load balancer rule:

```shell
$ cmk listLoadBalancerRuleInstances
```

`listLoadBalancerRuleInstances`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**pagesize**|Page size|integer|255|false|
|**page**|Page|integer|255|false|
|**keyword**|List by keyword|string|255|false|
|**id**|The ID of the load balancer rule|uuid|255|true|
|**applied**|True if listing all virtual machines currently applied to the load balancer rule; default is true|boolean|255|false|
|**lbvmips**|True if load balancer rule VM IP information to be included; default is false|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**lbvmipaddresses**|IP addresses of the VM set of LB rule|list|
|**loadbalancerruleinstance**|The user VM set for LB rule|uservmresponse|

## listSslCerts

This command lists SSL certificates.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists SSL certificates:

```shell
$ cmk listSslCerts
```

`listSslCerts`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**accountid**|Account ID|uuid|255|false|
|**certid**|ID of SSL certificate|uuid|255|false|
|**projectid**|Project that owns the SSL certificate|uuid|255|false|
|**lbruleid**|Load balancer rule ID|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**account**|Account for the certificate|string|
|**domain**|The domain name of the network owner|string|
|**projectid**|The project ID of the certificate|string|
|**certificate**|Certificate|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The project name of the certificate|string|
|**domainid**|The domain ID of the network owner|string|
|**loadbalancerrulelist**|List of load balancers this certificate is bound to|list|
|**name**|Name|string|
|**certchain**|Certificate chain|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**id**|SSL certificate ID|string|
|**fingerprint**|Certificate fingerprint|string|

## removeFromLoadBalancerRule

This command removes a virtual machine or a list of virtual machines from a load balancer rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command removes a virtual machine or a list of virtual machines from a load balancer rule:

```shell
$ cmk removeFromLoadBalancerRule
```

`removeFromLoadBalancerRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the load balancer rule|uuid|255|true|
|**vmidipmap**|VM ID and IP map, vmidipmap[0].vmid=1 vmidipmap[0].ip=10.1.1.75|map|255|false|
|**virtualmachineids**|The list of IDs of the virtual machines that are being removed from the load balancer rule (i.e. virtualMachineIds=1,2,3)|list|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## updateLBHealthCheckPolicy

This command updates load balancer health check policy.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates load balancer health check policy:

```shell
$ cmk updateLBHealthCheckPolicy
```

`updateLBHealthCheckPolicy`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**fordisplay**|An optional field, whether to the display the policy to the end user or not|boolean|255|false|
|**id**|ID of load balancer health check policy|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**domainid**|The domain ID of the HealthCheck policy|string|
|**lbruleid**|The LB rule ID|string|
|**account**|The account of the HealthCheck policy|string|
|**healthcheckpolicy**|The list of HealthCheck policies|list|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domain**|The domain of the HealthCheck policy|string|
|**zoneid**|The ID of the zone the HealthCheck policy belongs to|string|

## updateLBStickinessPolicy

This command updates load balancer stickiness policy.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates load balancer stickiness policy:

```shell
$ cmk updateLBStickinessPolicy
```

`updateLBStickinessPolicy`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|Id of LB stickiness policy|uuid|255|true|
|**fordisplay**|An optional field, whether to the display the policy to the end user or not|boolean|255|false|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**name**|The name of the Stickiness policy|string|
|**stickinesspolicy**|The list of Stickiness policies|list|
|**domain**|The domain of the Stickiness policy|string|
|**account**|The account of the Stickiness policy|string|
|**domainid**|The domain ID of the Stickiness policy|string|
|**description**|The description of the Stickiness policy|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**lbruleid**|The LB rule ID|string|
|**zoneid**|The ID of the zone the Stickiness policy belongs to|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**state**|The state of the policy|string|

## updateLoadBalancer

This command updates a load balancer.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates a load balancer:

```shell
$ cmk updateLoadBalancer
```

`updateLoadBalancer`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**id**|The ID of the load balancer|uuid|255|true|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**name**|The name of the Load Balancer|string|
|**account**|The account of the Load Balancer|string|
|**loadbalancerrule**|The list of rules associated with the Load Balancer|list|
|**networkid**|Load Balancer network ID|string|
|**description**|The description of the Load Balancer|string|
|**projectid**|The project ID of the Load Balancer|string|
|**sourceipaddressnetworkid**|Load Balancer source IP network id|string|
|**sourceipaddress**|Load Balancer source IP|string|
|**algorithm**|The load balancer algorithm (source, roundrobin, leastconn)|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the Load Balancer|string|
|**tags**|The list of resource tags associated with the Load Balancer|list|
|**domainid**|The domain ID of the Load Balancer|string|
|**domain**|The domain of the Load Balancer|string|
|**loadbalancerinstance**|The list of instances associated with the Load Balancer|list|
|**id**|The Load Balancer ID|string|

# Metrics

## listVolumesMetrics

This command lists volume metrics.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists volume metrics:

```shell
$ cmk listVolumesMetrics
```

`listVolumesMetrics`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**name**|The name of the disk volume|string|255|false|
|**hostid**|List volumes on specified host|uuid|255|false|
|**storageid**|The ID of the storage pool, available to ROOT admin only|string|255|false|
|**type**|The type of disk volume|string|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**keyword**|List by keyword|string|255|false|
|**zoneid**|The ID of the availability zone|uuid|255|false|
|**page**|Page|integer|255|false|
|**diskofferingid**|List volumes by disk offering|uuid|255|false|
|**displayvolume**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**virtualmachineid**|The ID of the virtual machine|uuid|255|false|
|**clusterid**|The cluster ID the disk volume belongs to|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**podid**|The pod ID the disk volume belongs to|uuid|255|false|
|**ids**|The IDs of the volumes, mutually exclusive with id|list|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**id**|The ID of the disk volume|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**podname**|Pod name of the volume|string|
|**size**|Size of the disk volume|long|
|**provisioningtype**|Provisioning type used to create volumes.|string|
|**serviceofferingid**|ID of the service offering for root disk|string|
|**sizegb**|Disk size in GiB|string|
|**created**|The date the disk volume was created|date|
|**storagetype**|Shared or local storage|string|
|**diskofferingname**|Name of the disk offering|string|
|**storage**|Name of the primary storage hosting the disk volume|string|
|**virtualsize**|The bytes actually consumed on disk|long|
|**serviceofferingname**|Name of the service offering for root disk|string|
|**attached**|The date the volume was attached to a VM instance|date|
|**id**|ID of the disk volume|string|
|**virtualmachineid**|ID of the virtual machine|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**account**|The account associated with the disk volume|string|
|**type**|Type of the disk volume (ROOT or DATADISK)|string|
|**path**|The path of the volume|string|
|**zoneid**|ID of the availability zone|string|
|**utilization**|The disk utilization|string|
|**domain**|The domain associated with the disk volume|string|
|**displayvolume**|An optional field whether to the display the volume to the end user or not|boolean|
|**vmdisplayname**|Display name of the virtual machine|string|
|**destroyed**|The boolean state of whether the volume is destroyed or not|boolean|
|**diskofferingdisplaytext**|The display text of the disk offering|string|
|**name**|Name of the disk volume|string|
|**domainid**|The ID of the domain associated with the disk volume|string|
|**physicalsize**|The bytes allocated|long|
|**chaininfo**|The chain info of the volume|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**diskIopsWriteRate**|IO requests write rate of the disk volume|long|
|**clusterid**|Cluster ID of the volume|string|
|**serviceofferingdisplaytext**|The display text of the service offering for root disk|string|
|**diskBytesWriteRate**|Bytes write rate of the disk volume|long|
|**state**|The state of the disk volume|string|
|**vmstate**|State of the virtual machine|string|
|**snapshotid**|ID of the snapshot from which this volume was created|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**diskBytesReadRate**|Bytes read rate of the disk volume|long|
|**clustername**|Cluster name where the volume is allocated|string|
|**tags**|The list of resource tags associated|set|
|**status**|The status of the volume|string|
|**diskIopsReadRate**|IO requests read rate of the disk volume|long|
|**hypervisor**|Hypervisor the volume belongs to|string|
|**storageid**|ID of the primary storage hosting the disk volume; returned to admin user only|string|
|**isextractable**|True if the volume is extractable, false otherwise|boolean|
|**deviceid**|The ID of the device on user VM the volume is attahed to. This tag is not returned when the volume is detached.|long|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**quiescevm**|Need quiesce VM or not when taking snapshot|boolean|
|**templatename**|The name of the template for the virtual machine|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**projectid**|The project ID of the VPN|string|
|**vmname**|Name of the virtual machine|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**podid**|Pod ID of the volume|string|
|**project**|The project name of the VPN|string|
|**diskofferingid**|ID of the disk offering|string|
|**miniops**|Min IOPS of the disk volume|long|
|**zonename**|Name of the availability zone|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**maxiops**|Max IOPS of the disk volume|long|

# NAT

## createIpForwardingRule

This command creates an IP forwarding rule.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an IP forwarding rule:

```shell
$ cmk createIpForwardingRule
```

`createIpForwardingRule`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**endport**|The end port for the rule|integer|255|false|
|**protocol**|The protocol for the rule. Valid values are TCP or UDP.|string|255|true|
|**startport**|The start port for the rule|integer|255|true|
|**openfirewall**|If true, firewall rule for source/end public port is automatically created; if false - firewall rule has to be created explicitly. Has value true by default|boolean|255|false|
|**cidrlist**|The CIDR list to forward traffic from|list|255|false|
|**ipaddressid**|The public IP address ID of the forwarding rule, already associated via associateIp|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**vmguestip**|The VM IP address for the port forwarding rule|string|
|**fordisplay**|Is firewall for display to the regular user|boolean|
|**protocol**|The protocol of the port forwarding rule|string|
|**id**|The ID of the port forwarding rule|string|
|**state**|The state of the rule|string|
|**virtualmachineid**|The VM ID for the port forwarding rule|string|
|**ipaddressid**|The public IP address ID for the port forwarding rule|string|
|**networkid**|The ID of the guest network the port forwarding rule belongs to|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**ipaddress**|The public IP address for the port forwarding rule|string|
|**virtualmachinename**|The VM name for the port forwarding rule|string|
|**tags**|The list of resource tags associated with the rule|list|
|**virtualmachinedisplayname**|The VM display name for the port forwarding rule|string|
|**privateport**|The starting port of port forwarding rule's private port range|string|
|**privateendport**|The ending port of port forwarding rule's private port range|string|
|**publicendport**|The ending port of port forwarding rule's private port range|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**publicport**|The starting port of port forwarding rule's public port range|string|
|**cidrlist**|The CIDR list to forward traffic from|string|

## disableStaticNat

This command disables static rule for given IP address.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command disables static rule for given IP address:

```shell
$ cmk disableStaticNat
```

`disableStaticNat`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ipaddressid**|The public IP address ID for which static NAT feature is being disabled|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## listIpForwardingRules

This command lists the IP forwarding rules.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists the IP forwarding rules:

```shell
$ cmk listIpForwardingRules
```

`listIpForwardingRules`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ipaddressid**|List the rule belonging to this public IP address|uuid|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**keyword**|List by keyword|string|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**page**|Page|integer|255|false|
|**pagesize**|Page size|integer|255|false|
|**virtualmachineid**|Lists all rules applied to the specified VM|uuid|255|false|
|**id**|Lists rule with the specified ID|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**publicendport**|The ending port of port forwarding rule's private port range|string|
|**publicport**|The starting port of port forwarding rule's public port range|string|
|**state**|The state of the rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**id**|The ID of the port forwarding rule|string|
|**vmguestip**|The VM IP address for the port forwarding rule|string|
|**virtualmachineid**|The VM ID for the port forwarding rule|string|
|**privateport**|The starting port of port forwarding rule's private port range|string|
|**virtualmachinename**|The VM name for the port forwarding rule|string|
|**privateendport**|The ending port of port forwarding rule's private port range|string|
|**tags**|The list of resource tags associated with the rule|list|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**virtualmachinedisplayname**|The VM display name for the port forwarding rule|string|
|**ipaddressid**|The public IP address ID for the port forwarding rule|string|
|**protocol**|The protocol of the port forwarding rule|string|
|**ipaddress**|The public IP address for the port forwarding rule|string|
|**networkid**|The ID of the guest network the port forwarding rule belongs to|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**fordisplay**|Is firewall for display to the regular user|boolean|

# Network

## createNetworkACL

This command creates an ACL rule in the given network (the network has to belong to VPC).

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an ACL rule in the given network:

```shell
$ cmk createNetworkACL
```

`createNetworkACL`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**number**|The network of the VM the ACL will be created for|integer|255|false|
|**fordisplay**|An optional field, whether to the display the rule to the end user or not|boolean|255|false|
|**protocol**|The protocol for the ACL rule. Valid values are TCP/UDP/ICMP/ALL or valid protocol number.|string|255|true|
|**cidrlist**|The CIDR list to allow traffic from/to|list|255|false|
|**aclid**|The network of the VM the ACL will be created for|uuid|255|false|
|**icmptype**|Type of the ICMP message being sent|integer|255|false|
|**networkid**|The network of the VM the ACL will be created for|uuid|255|false|
|**endport**|The ending port of ACL|integer|255|false|
|**traffictype**|The traffic type for the ACL, can be ingress or egress, defaulted to ingress if not specified|string|255|false|
|**icmpcode**|Error code for this ICMP message|integer|255|false|
|**startport**|The starting port of ACL|integer|255|false|
|**action**|SCL entry action, allow or deny|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**icmpcode**|Error code for this ICMP message|integer|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**startport**|The starting port of ACL's port range|string|
|**number**|Number of the ACL Item|integer|
|**aclid**|The ID of the ACL this item belongs to|string|
|**tags**|The list of resource tags associated with the network ACLs|list|
|**traffictype**|The traffic type for the ACL|string|
|**protocol**|The protocol of the ACL|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**state**|The state of the rule|string|
|**action**|Action of ACL Item. Allow/Deny.|string|
|**endport**|The ending port of ACL's port range|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**icmptype**|Type of the ICMP message being sent|integer|
|**id**|The ID of the ACL Item|string|
|**fordisplay**|Is rule for display to the regular user|boolean|

## listNetworkACLLists

This command lists all network ACLs.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all network ACLs:

```shell
$ cmk listNetworkACLLists
```

`listNetworkACLLists`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|List resources by account. Must be used with the domainId parameter.|string|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**vpcid**|List network ACLs by VPC ID|uuid|255|false|
|**id**|Lists network ACL with the specified ID|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**page**|Page|integer|255|false|
|**networkid**|List network ACLs by network ID|uuid|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**name**|List network ACLs by specified name|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**keyword**|List by keyword|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**fordisplay**|Is ACL for display to the regular user|boolean|
|**vpcid**|ID of the VPC this ACL is associated with|string|
|**id**|The ID of the ACL|string|
|**description**|Description of the ACL|string|
|**name**|The Name of the ACL|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listNetworkACLs

This command lists all network ACL items.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all network ACL items:

```shell
$ cmk listNetworkACLs
```

`listNetworkACLs`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**keyword**|List by keyword|string|255|false|
|**page**|Page|integer|255|false|
|**networkid**|List network ACL items by network ID|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**protocol**|List network ACL items by protocol|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**action**|List network ACL items by action|string|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**aclid**|List network ACL items by ACL ID|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**id**|Lists network ACL Item with the specified ID|uuid|255|false|
|**traffictype**|List network ACL items by traffic type - ingress or egress|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**number**|Number of the ACL Item|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**icmpcode**|Error code for this ICMP message|integer|
|**icmptype**|Type of the ICMP message being sent|integer|
|**aclid**|The ID of the ACL this item belongs to|string|
|**tags**|The list of resource tags associated with the network ACLs|list|
|**endport**|The ending port of ACL's port range|string|
|**startport**|The starting port of ACL's port range|string|
|**state**|The state of the rule|string|
|**cidrlist**|The CIDR list to forward traffic from|string|
|**fordisplay**|Is rule for display to the regular user|boolean|
|**traffictype**|The traffic type for the ACL|string|
|**protocol**|The protocol of the ACL|string|
|**id**|The ID of the ACL Item|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**action**|Action of ACL Item. Allow/Deny.|string|

## listNetworks

This command lists all available networks.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all available networks:

```shell
$ cmk listNetworks
```

`listNetworks`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**zoneid**|The zone ID of the network|uuid|255|false|
|**traffictype**|Type of the traffic|string|255|false|
|**restartrequired**|List networks by restartRequired|boolean|255|false|
|**keyword**|List by keyword|string|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**acltype**|List networks by ACL (access control list) type. Supported values are account and domain.|string|255|false|
|**issystem**|True if network is system, false otherwise|boolean|255|false|
|**forvpc**|The network belongs to VPC|boolean|255|false|
|**displaynetwork**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**id**|List networks by ID|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**page**|Page|integer|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**physicalnetworkid**|List networks by physical network ID|uuid|255|false|
|**specifyipranges**|True if need to list only networks which support specifying IP ranges|boolean|255|false|
|**vpcid**|List networks by VPC|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**supportedservices**|List networks supporting certain services|list|255|false|
|**canusefordeploy**|List networks available for VM deployment|boolean|255|false|
|**type**|The type of the network. Supported values are: isolated and shared.|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**networkcidr**|The network CIDR of the guest network configured with IP reservation. It is the summation of CIDR and RESERVED_IP_RANGE.|string|
|**specifyipranges**|True if network supports specifying IP ranges, false otherwise|boolean|
|**isdefault**|True if network is default, false otherwise|boolean|
|**dns1**|The first DNS for the network|string|
|**cidr**|CloudStack managed address space, all CloudStack managed VMs get IP address from CIDR|string|
|**aclid**|ACL ID associated with the VPC network|string|
|**redundantrouter**|True if the network has redundant routers enabled, false otherwise|boolean|
|**state**|State of the network|string|
|**tags**|The list of resource tags associated with network|list|
|**domain**|The domain name of the network owner|string|
|**networkofferingavailability**|Availability of the network offering the network is created from|string|
|**domainid**|The domain ID of the network owner|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**zonename**|The name of the zone the network belongs to|string|
|**networkofferingname**|Name of the network offering the network is created from|string|
|**reservediprange**|The network's IP range not to be used by CloudStack guest VMs and can be used for non CloudStack purposes|string|
|**physicalnetworkid**|The physical network ID|string|
|**ip6gateway**|The gateway of IPv6 network|string|
|**issystem**|True if network is system, false otherwise|boolean|
|**canusefordeploy**|List networks available for VM deployment|boolean|
|**subdomainaccess**|True if users from subdomains can access the domain level network|boolean|
|**networkofferingid**|Network offering ID the network is created from|string|
|**account**|The owner of the network|string|
|**displaytext**|The displaytext of the network|string|
|**ip6cidr**|The CIDR of IPv6 network|string|
|**restartrequired**|True network requires restart|boolean|
|**name**|The name of the network|string|
|**projectid**|The project ID of the IP address|string|
|**service**|The list of services|list|
|**zonesnetworkspans**|If a network is enabled for 'streched l2 subnet' then represents zones on which network currently spans|set|
|**acltype**|ACL type - access type to the network|string|
|**type**|The type of the network|string|
|**networkofferingconservemode**|True if network offering is IP conserve mode enabled|boolean|
|**networkdomain**|The network domain|string|
|**traffictype**|The traffic type of the network|string|
|**zoneid**|Zone ID of the network|string|
|**externalid**|The external ID of the network|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**vlan**|The VLAN of the network. This parameter is visible to ROOT admins only.|string|
|**strechedl2subnet**|True if network can span multiple zones|boolean|
|**gateway**|The network's gateway|string|
|**netmask**|The network's netmask|string|
|**dns2**|The second DNS for the network|string|
|**broadcasturi**|Broadcast URI of the network. This parameter is visible to ROOT admins only.|string|
|**project**|The project name of the address|string|
|**related**|Related to what other network configuration|string|
|**id**|The ID of the network|string|
|**displaynetwork**|An optional field, whether to the display the network to the end user or not|boolean|
|**ispersistent**|List networks that are persistent|boolean|
|**networkofferingdisplaytext**|Display text of the network offering the network is created from|string|
|**broadcastdomaintype**|Broadcast domain type of the network|string|
|**vpcid**|VPC the network belongs to|string|

## replaceNetworkACLList

This command replaces ACL associated with a network or private gateway.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command replaces ACL associated with a network or private gateway:

```shell
$ cmk replaceNetworkACLList
```

`replaceNetworkACLList`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**networkid**|The ID of the network|uuid|255|false|
|**aclid**|The ID of the network ACL|uuid|255|true|
|**gatewayid**|The ID of the private gateway|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## restartNetwork

This command restarts the network; includes 1) restarting network elements - virtual routers, DHCP servers 2) reapplying all public IPs 3) reapplying loadBalancing/portForwarding rules.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command restarts the network:

```shell
$ cmk restartNetwork
```

`restartNetwork`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**makeredundant**|Turn the network into a network with redundant routers|boolean|255|false|
|**cleanup**|If cleanup old network elements|boolean|255|false|
|**id**|The ID of the network to restart|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**project**|The project name of the address|string|
|**issystem**|True if this IP is system IP (was allocated as a part of deployVm or createLbRule)|boolean|
|**tags**|The list of resource tags associated with IP address|list|
|**zoneid**|The ID of the zone the public IP address belongs to|string|
|**physicalnetworkid**|The physical network this belongs to|string|
|**id**|Public IP address ID|string|
|**associatednetworkname**|The name of the Network associated with the IP address|string|
|**issourcenat**|True if the IP address is a source nat address, false otherwise|boolean|
|**vlanid**|The ID of the VLAN associated with the IP address. This parameter is visible to ROOT admins only.|string|
|**isstaticnat**|True if this IP is for static nat, false otherwise|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectid**|The project ID of the IP address|string|
|**zonename**|The name of the zone the public IP address belongs to|string|
|**vlanname**|The VLAN associated with the IP address|string|
|**vmipaddress**|Virtual machine (DNAT) IP address (not null only for static nat IP)|string|
|**account**|The account the public IP address is associated with|string|
|**purpose**|Purpose of the IP address. In Acton this value is not null for IPs with isSystem=true, and can have either StaticNat or LB value|string|
|**forvirtualnetwork**|The virtual network for the IP address|boolean|
|**state**|State of the ip address. Can be: Allocatin, Allocated and Releasing.|string|
|**virtualmachinename**|Virtual machine name the IP address is assigned to (not null only for static nat IP)|string|
|**domain**|The domain the public IP address is associated with|string|
|**vpcid**|VPC the IP belongs to|string|
|**virtualmachinedisplayname**|Virtual machine display name the IP address is assigned to (not null only for static nat IP)|string|
|**allocated**|Date the public IP address was acquired|date|
|**networkid**|The ID of the Network where IP belongs to|string|
|**ipaddress**|Public IP address|string|
|**isportable**|Is public IP portable across the zones|boolean|
|**associatednetworkid**|The ID of the Network associated with the IP address|string|
|**fordisplay**|Is public IP for display to the regular user|boolean|
|**virtualmachineid**|Virtual machine ID the IP address is assigned to (not null only for static nat IP)|string|
|**domainid**|The domain ID the public IP address is associated with|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## updateNetwork

This command updates a network.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates a network:

```shell
$ cmk updateNetwork
```

`updateNetwork`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**networkofferingid**|Network offering ID|uuid|255|false|
|**displaynetwork**|An optional field, whether to the display the network to the end user or not|boolean|255|false|
|**updateinsequence**|If true, we will update the routers one after the other. Applicable only for redundant router based networks using virtual router as provider.|boolean|255|false|
|**networkdomain**|Network domain|string|255|false|
|**displaytext**|The new display text for the network|string|255|false|
|**guestvmcidr**|CIDR for guest VMs, CloudStack allocates IPs to guest VMs only from this CIDR|string|255|false|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**forced**|Setting this to true will cause a forced network update|boolean|255|false|
|**name**|The new name for the network|string|255|false|
|**id**|The ID of the network|uuid|255|true|
|**changecidr**|Force update even if CIDR type is different|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**related**|Related to what other network configuration|string|
|**dns1**|The first DNS for the network|string|
|**zonename**|The name of the zone the network belongs to|string|
|**issystem**|True if network is system, false otherwise|boolean|
|**project**|The project name of the address|string|
|**externalid**|The external ID of the network|string|
|**netmask**|The network's netmask|string|
|**physicalnetworkid**|The physical network ID|string|
|**broadcasturi**|Broadcast URI of the network. This parameter is visible to ROOT admins only.|string|
|**projectid**|The project ID of the IP address|string|
|**subdomainaccess**|True if users from subdomains can access the domain level network|boolean|
|**tags**|The list of resource tags associated with network|list|
|**strechedl2subnet**|True if network can span multiple zones|boolean|
|**displaytext**|The display text of the network|string|
|**name**|The name of the network|string|
|**id**|The ID of the network|string|
|**networkofferingname**|Name of the network offering the network is created from|string|
|**redundantrouter**|If the network has redundant routers enabled|boolean|
|**vpcid**|VPC the network belongs to|string|
|**vlan**|The VLAN of the network. This parameter is visible to ROOT admins only.|string|
|**networkcidr**|The network CIDR of the guest network configured with IP reservation. It is the summation of CIDR and RESERVED_IP_RANGE.|string|
|**restartrequired**|True network requires restart|boolean|
|**zonesnetworkspans**|If a network is enabled for 'streched l2 subnet' then represents zones on which network currently spans|set|
|**ip6gateway**|The gateway of IPv6 network|string|
|**networkofferingavailability**|Availability of the network offering the network is created from|string|
|**broadcastdomaintype**|Broadcast domain type of the network|string|
|**dns2**|The second DNS for the network|string|
|**acltype**|ACL type - access type to the network|string|
|**zoneid**|Zone ID of the network|string|
|**networkofferingdisplaytext**|Display text of the network offering the network is created from|string|
|**isdefault**|True if network is default, false otherwise|boolean|
|**cidr**|CloudStack managed address space, all CloudStack managed VMs get IP address from CIDR|string|
|**traffictype**|The traffic type of the network|string|
|**state**|State of the network|string|
|**specifyipranges**|True if network supports specifying IP ranges, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**networkofferingid**|Network offering ID the network is created from|string|
|**gateway**|The network's gateway|string|
|**networkofferingconservemode**|True if network offering is IP conserve mode enabled|boolean|
|**ip6cidr**|The CIDR of IPv6 network|string|
|**networkdomain**|The network domain|string|
|**canusefordeploy**|List networks available for VM deployment|boolean|
|**domainid**|The domain ID of the network owner|string|
|**aclid**|ACL ID associated with the VPC network|string|
|**ispersistent**|List networks that are persistent|boolean|
|**displaynetwork**|An optional field, whether to the display the network to the end user or not|boolean|
|**domain**|The domain name of the network owner|string|
|**service**|The list of services|list|
|**type**|The type of the network|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**account**|The owner of the network|string|
|**reservediprange**|The network's IP range not to be used by CloudStack guest VMs and can be used for non CloudStack purposes|string|

## updateNetworkACLList

This command updates network ACL list.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates network ACL list:

```shell
$ cmk updateNetworkACLList
```

`updateNetworkACLList`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**fordisplay**|An optional field, whether to the display the list to the end user or not|boolean|255|false|
|**id**|The ID of the network ACL|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

# Nic

## addIpToNic

This command assigns secondary IP to NIC.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command assigns secondary IP to NIC:

```shell
$ cmk addIpToNic
```

`addIpToNic`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ipaddress**|Secondary IP Address|string|255|false|
|**nicid**|The ID of the nic to which you want to assign private IP|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The ID of the secondary private IP addr|string|
|**ipaddress**|Secondary IP address|string|
|**secondaryip**|The list of Secondary ipv4 addr of nic|list|
|**nicid**|The ID of the nic|string|
|**virtualmachineid**|The ID of the VM|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**networkid**|The ID of the network|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listNics

This command lists the VM nics IP to NIC.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists the VM nics IP to NIC:

```shell
$ cmk listNics
```

`listNics`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**virtualmachineid**|The ID of the VM|uuid|255|true|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**nicid**|The ID of the nic to to list IPs|uuid|255|false|
|**page**|Page|integer|255|false|
|**keyword**|List by keyword|string|255|false|
|**networkid**|List nic of the specific VM's network|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**nsxlogicalswitch**|ID of the NSX Logical Switch (if NSX based), null otherwise|string|
|**macaddress**|True if nic is default, false otherwise|string|
|**extradhcpoption**|The extra DHCP options on the nic|list|
|**virtualmachineid**|ID of the VM to which the nic belongs|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**networkid**|The ID of the corresponding network|string|
|**ip6cidr**|The CIDR of IPv6 network|string|
|**broadcasturi**|The broadcast URI of the nic|string|
|**nsxlogicalswitchport**|ID of the NSX Logical Switch Port (if NSX based), null otherwise|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**isdefault**|True if nic is default, false otherwise|boolean|
|**ip6address**|The IPv6 address of network|string|
|**secondaryip**|The Secondary IPv4 addr of nic|list|
|**ip6gateway**|The gateway of IPv6 network|string|
|**gateway**|The gateway of the nic|string|
|**ipaddress**|The IP address of the nic|string|
|**traffictype**|The traffic type of the nic|string|
|**netmask**|The netmask of the nic|string|
|**networkname**|The name of the corresponding network|string|
|**type**|The type of the nic|string|
|**isolationuri**|The isolation URI of the nic|string|
|**deviceid**|Device ID for the network when plugged into the virtual machine|string|
|**id**|The ID of the nic|string|

## updateVmNicIp

This command update the default IP of a VM Nic.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command update the default Ip of a VM Nic:

```shell
$ cmk updateVmNicIp
```

`updateVmNicIp`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ipaddress**|Secondary IP Address|string|255|false|
|**nicid**|The ID of the nic to which you want to assign private IP|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**memorytargetkbs**|The target memory in VM|long|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**cpuspeed**|The speed of each cpu|integer|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**displayname**|User generated name. The name of the virtual machine is returned if no displayname exists.|string|
|**created**|The date when this virtual machine was created|date|
|**name**|The name of the virtual machine|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**password**|The password (if exists) of the virtual machine|string|
|**username**|The user's name who deployed the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**account**|The account associated with the virtual machine|string|
|**keypair**|SSH key-pair|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**groupid**|The group ID of the virtual machine|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**nic**|The list of nics associated with VM|set|
|**ostypeid**|OS type ID of the VM|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**group**|The group name of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**hostname**|The name of the host for the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**guestosid**|OS type ID of the virtual machine|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**memorykbs**|The memory used by the VM|long|
|**memory**|The memory allocated for the virtual machine|integer|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**state**|The state of the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**publicip**|Public IP address ID associated with vm via Static nat rule|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**projectid**|The project ID of the VM|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**tags**|The list of resource tags associated|set|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**id**|The ID of the virtual machine|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**publicipid**|Public IP address ID associated with vm via Static nat rule|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**rootdeviceid**|Device ID of the root volume|long|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**templatename**|The name of the template for the virtual machine|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**project**|The project name of the VM|string|
|**details**|VM details in key/value pairs|map|

# Project

## createProject

This command creates a project.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a project:

```shell
$ cmk createProject
```

`createProject`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**name**|Name of the project|string|255|true|
|**domainid**|Domain ID of the account owning a project|uuid|255|false|
|**account**|Account who will be Admin for the project|string|255|false|
|**displaytext**|Display text of the project|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**cpulimit**|The total number of CPU cores the project can own|string|
|**templatetotal**|The total number of templates which have been created by this project|long|
|**snapshotavailable**|The total number of snapshots available for this project|string|
|**secondarystorageavailable**|The total secondary storage space (in GiB) available to be used for this project|string|
|**primarystorageavailable**|The total primary storage space (in GiB) available to be used for this project|string|
|**secondarystoragelimit**|The total secondary storage space (in GiB) the project can own|string|
|**id**|The ID of the project|string|
|**vmrunning**|The total number of virtual machines running for this project|integer|
|**snapshottotal**|The total number of snapshots stored by this project|long|
|**vmtotal**|The total number of virtual machines deployed by this project|long|
|**volumeavailable**|The total volume available for this project|string|
|**volumetotal**|The total volume being used by this project|long|
|**snapshotlimit**|The total number of snapshots which can be stored by this project|string|
|**templatelimit**|The total number of templates which can be created by this project|string|
|**state**|The state of the project|string|
|**vmavailable**|The total number of virtual machines available for this project to acquire|string|
|**memorylimit**|The total memory (in MB) the project can own|string|
|**ipavailable**|The total number of public IP addresses available for this project to acquire|string|
|**cpuavailable**|The total number of CPU cores available to be created for this project|string|
|**networktotal**|The total number of networks owned by project|long|
|**networkavailable**|The total number of networks available to be created for this project|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**vmstopped**|The total number of virtual machines stopped for this project|integer|
|**vpctotal**|The total number of VPCs owned by project|long|
|**volumelimit**|The total volume which can be used by this project|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**memoryavailable**|The total memory (in MB) available to be created for this project|string|
|**vpcavailable**|The total number of VPCs available to be created for this project|string|
|**domain**|The domain name where the project belongs to|string|
|**name**|The name of the project|string|
|**account**|The account name of the project's owner|string|
|**memorytotal**|The total memory (in MB) owned by project|long|
|**primarystoragelimit**|The total primary storage space (in GiB) the project can own|string|
|**cputotal**|The total number of CPU cores owned by project|long|
|**iptotal**|The total number of public ip addresses allocated for this project|long|
|**networklimit**|The total number of networks the project can own|string|
|**templateavailable**|The total number of templates available to be created by this project|string|
|**domainid**|The domain ID the project belongs to|string|
|**vmlimit**|The total number of virtual machines that can be deployed by this project|string|
|**iplimit**|The total number of public IP addresses this project can acquire|string|
|**secondarystoragetotal**|The total secondary storage space (in GiB) owned by project|float|
|**primarystoragetotal**|The total primary storage space (in GiB) owned by project|long|
|**projectaccountname**|The project account name of the project|string|
|**displaytext**|The display text of the project|string|
|**vpclimit**|The total number of VPCs the project can own|string|
|**tags**|The list of resource tags associated with vm|list|

## deleteProjectInvitation

This command deletes project invitation.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes project invitation:

```shell
$ cmk deleteProjectInvitation
```

`deleteProjectInvitation`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|ID of the invitation|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## listProjectInvitations

This command lists project invitations and provides detailed information for listed invitations.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists project invitations and provides detailed information for listed invitations:

```shell
$ cmk listProjectInvitations
```

`listProjectInvitations`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|List invitations by ID|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**keyword**|List by keyword|string|255|false|
|**activeonly**|If true, list only active invitations - having Pending state and ones that are not timed out yet|boolean|255|false|
|**page**|Page|integer|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**projectid**|List by project ID|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**state**|List invitations by state|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**state**|The invitation state|string|
|**domainid**|The domain ID the project belongs to|string|
|**email**|The email the invitation was sent to|string|
|**id**|The ID of the invitation|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The name of the project|string|
|**domain**|The domain name where the project belongs to|string|
|**projectid**|The ID of the project|string|
|**account**|The account name of the project's owner|string|

# Quota

## quotaIsEnabled

This command returns true if the plugin is enabled.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command return true if the plugin is enabled:

```shell
$ cmk quotaIsEnabled
```

`quotaIsEnabled`

### Query parameters

There are no query parameters available for this command.

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**isenabled**|Is quota service enabled|boolean|

# Resource tags

## deleteTags

This command deletes resource tag(s).

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes resource tag(s):

```shell
$ cmk deleteTags
```

`deleteTags`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**tags**|Delete tags matching key/value pairs|map|255|false|
|**resourcetype**|Delete tag by resource type|string|255|true|
|**resourceids**|Delete tags for resource ID(s)|list|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|

## listTags

This command lists resource tag(s).

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists resource tag(s):

```shell
$ cmk listTags
```

`listTags`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**key**|List by key|string|255|false|
|**customer**|List by customer name|string|255|false|
|**value**|List by value|string|255|false|
|**keyword**|List by keyword|string|255|false|
|**page**|Page|integer|255|false|
|**resourcetype**|List by resource type|string|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**resourceid**|List by resource ID|string|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**customer**|Customer associated with the tag|string|
|**value**|Tag value|string|
|**domain**|The domain associated with the tag|string|
|**resourcetype**|Resource type|string|
|**resourceid**|ID of the resource|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**domainid**|The ID of the domain associated with the tag|string|
|**projectid**|The project id the tag belongs to|string|
|**project**|The project name where tag belongs to|string|
|**key**|Tag key name|string|
|**account**|The account associated with the tag|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

# Resource metadata

## listResourceDetails

This command lists resource detail(s).

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command list resource detail(s):

```shell
$ cmk listResourceDetails
```

`listResourceDetails`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**key**|List by key|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**fordisplay**|If set to true, only details marked with display=true, are returned. False by default.|boolean|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**page**|Page|integer|255|false|
|**value**|List by key, value. Needs to be passed only along with key.|string|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**keyword**|List by keyword|string|255|false|
|**resourcetype**|List by resource type|string|255|true|
|**resourceid**|List by resource ID|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**resourceid**|ID of the resource|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**account**|The account associated with the tag|string|
|**customer**|Customer associated with the tag|string|
|**domain**|The domain associated with the tag|string|
|**domainid**|The ID of the domain associated with the tag|string|
|**projectid**|The project ID the tag belongs to|string|
|**resourcetype**|Resource type|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The project name where tag belongs to|string|
|**key**|Tag key name|string|
|**value**|Tag value|string|

# Security group

## authorizeSecurityGroupEgress

This command authorizes a particular egress rule for this security group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command authorizes a particular egress rule for this security group:

```shell
$ cmk authorizeSecurityGroupEgress
```

`authorizeSecurityGroupEgress`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|An optional domain ID for the security group. If the account parameter is used, domain ID must also be used.|uuid|255|false|
|**startport**|Start port for this egress rule|integer|255|false|
|**account**|An optional account for the security group. Must be used with domain ID.|string|255|false|
|**protocol**|TCP is default. UDP is the other supported protocol.|string|255|false|
|**projectid**|An optional project of the security group|uuid|255|false|
|**securitygroupname**|The name of the security group. Mutually exclusive with securityGroupId parameter|string|255|false|
|**icmptype**|Type of the ICMP message being sent|integer|255|false|
|**icmpcode**|Error code for this ICMP message|integer|255|false|
|**cidrlist**|The CIDR list associated|list|255|false|
|**usersecuritygrouplist**|User to security group mapping|map|255|false|
|**securitygroupid**|The ID of the security group. Mutually exclusive with securityGroupName parameter.|uuid|255|false|
|**endport**|End port for this egress rule|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**protocol**|The protocol of the security group rule|string|
|**cidr**|The CIDR notation for the base IP address of the security group rule|string|
|**icmpcode**|The code for the ICMP message response|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**endport**|The ending IP of the security group rule|integer|
|**account**|Account owning the security group rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**icmptype**|The type of the ICMP message response|integer|
|**startport**|The starting IP of the security group rule|integer|
|**tags**|The list of resource tags associated with the rule|set|
|**securitygroupname**|Security group name|string|
|**ruleid**|The ID of the security group rule|string|

## deleteSecurityGroup

This command deletes security group.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command deletes security group:

```shell
$ cmk deleteSecurityGroup
```

`deleteSecurityGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|The domain ID of account owning the security group|uuid|255|false|
|**projectid**|The project of the security group|uuid|255|false|
|**name**|The ID of the security group. Mutually exclusive with id parameter.|string|255|false|
|**id**|The ID of the security group. Mutually exclusive with name parameter.|uuid|255|false|
|**account**|The account of the security group. Must be specified with domain ID.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## listSecurityGroups

This command lists security groups.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists security groups:

```shell
$ cmk listSecurityGroups
```

`listSecurityGroups`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**virtualmachineid**|Lists security groups by virtual machine ID|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**id**|List the security group by the ID provided|uuid|255|false|
|**page**|Page|integer|255|false|
|**securitygroupname**|Lists security groups by name|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**name**|The name of the security group|string|
|**virtualmachineids**|The list of virtualmachine ids associated with this securitygroup|set|
|**domain**|The domain name of the security group|string|
|**account**|The account owning the security group|string|
|**project**|The project name of the group|string|
|**ingressrule**|The list of ingress rules associated with the security group|set|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**description**|The description of the security group|string|
|**egressrule**|The list of egress rules associated with the security group|set|
|**projectid**|The project id of the group|string|
|**domainid**|The domain ID of the security group|string|
|**id**|The ID of the security group|string|
|**tags**|The list of resource tags associated with the rule|set|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**virtualmachinecount**|The number of virtualmachines associated with this securitygroup|integer|

## revokeSecurityGroupEgress

This command deletes a particular egress rule from this security group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a particular egress rule from this security group:

```shell
$ cmk revokeSecurityGroupEgress
```

`revokeSecurityGroupEgress`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the egress rule|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|

## revokeSecurityGroupIngress

This command deletes a particular ingress rule from this security group.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a particular ingress rule from this security group:

```shell
$ cmk revokeSecurityGroupIngress
```

`revokeSecurityGroupIngress`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the ingress rule|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

# Snapshot

## createSnapshot

This command creates an instant snapshot of a volume.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates an instant snapshot of a volume:

```shell
$ cmk createSnapshot
```

`createSnapshot`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|The account of the snapshot. The account parameter must be used with the domain ID parameter.|string|255|false|
|**name**|The name of the snapshot|string|255|false|
|**policyid**|Policy ID of the snapshot, if this is null, then use MANUAL_POLICY|uuid|255|false|
|**volumeid**|The ID of the disk volume|uuid|255|true|
|**locationtype**|Currently applicable only for managed storage. Valid location types: 'primary', 'secondary'. Default = 'primary'.|string|255|false|
|**quiescevm**|Quiesce VM if true|boolean|255|false|
|**asyncbackup**|Asynchronous backup if true|boolean|255|false|
|**domainid**|The domain ID of the snapshot. If used with the account parameter, specifies a domain for the account associated with the disk volume.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|ID of the snapshot|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**domain**|The domain name of the snapshot's account|string|
|**volumename**|Name of the disk volume|string|
|**osdisplayname**|Display name of the OS on volume|string|
|**ostypeid**|ID of the OS on volume|string|
|**physicalsize**|Physical size of backed up snapshot on image store|long|
|**virtualsize**|Virtual size of backed up snapshot on image store|long|
|**intervaltype**|Valid types are hourly, daily, weekly, monthy, template, and none|string|
|**created**|The date the snapshot was created|date|
|**zoneid**|ID of the availability zone|string|
|**locationtype**|Valid location types are primary and secondary|string|
|**name**|Name of the snapshot|string|
|**state**|The state of the snapshot. BackedUp means that snapshot is ready to be used; Creating - the snapshot is being allocated on the primary storage; BackingUp - the snapshot is being backed up on secondary storage|state|
|**volumetype**|Type of the disk volume|string|
|**domainid**|The domain ID of the snapshot's account|string|
|**tags**|The list of resource tags associated with snapshot|list|
|**volumeid**|ID of the disk volume|string|
|**snapshottype**|The type of the snapshot|string|
|**account**|The account associated with the snapshot|string|
|**revertable**|Indicates whether the underlying storage supports reverting the volume to this snapshot|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectid**|The project ID of the snapshot|string|
|**project**|The project name of the snapshot|string|

## createSnapshotPolicy

This command creates a snapshot policy for the account.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command creates a snapshot policy for the account:

```shell
$ cmk createSnapshotPolicy
```

`createSnapshotPolicy`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**maxsnaps**|Maximum number of snapshots to retain|integer|255|true|
|**intervaltype**|Valid values are HOURLY, DAILY, WEEKLY, and MONTHLY|string|255|true|
|**timezone**|Specifies a timezone for this command. For more information on the timezone parameter, see Time Zone Format.|string|255|true|
|**fordisplay**|An optional field, whether to the display the policy to the end user or not|boolean|255|false|
|**schedule**|Time the snapshot is scheduled to be taken. Format is:* if HOURLY, MM* if DAILY, MM:HH* if WEEKLY, MM:HH:DD (1-7)* if MONTHLY, MM:HH:DD (1-28)|string|255|true|
|**volumeid**|The ID of the disk volume|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The ID of the snapshot policy|string|
|**intervaltype**|The interval type of the snapshot policy|short|
|**fordisplay**|Is this policy for display to the regular user|boolean|
|**maxsnaps**|Maximum number of snapshots retained|int|
|**timezone**|The time zone of the snapshot policy|string|
|**volumeid**|The ID of the disk volume|string|
|**schedule**|Time the snapshot is scheduled to be taken|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|

## deleteSnapshot

This command deletes a snapshot of a disk volume.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a snapshot of a disk volume:

```shell
$ cmk deleteSnapshot
```

`deleteSnapshot`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the snapshot|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|

## deleteVMSnapshot

This command deletes a vmsnapshot.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a vmsnapshot:

```shell
$ cmk deleteVMSnapshot
```

`deleteVMSnapshot`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**vmsnapshotid**|The ID of the VM snapshot|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## listSnapshotPolicies

This command lists snapshot policies.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists snapshot policies:

```shell
$ cmk listSnapshotPolicies
```

`listSnapshotPolicies`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**volumeid**|The ID of the disk volume|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**page**|Page|integer|255|false|
|**id**|The ID of the snapshot policy|uuid|255|false|
|**fordisplay**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**keyword**|List by keyword|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**intervaltype**|The interval type of the snapshot policy|short|
|**maxsnaps**|Maximum number of snapshots retained|int|
|**id**|The ID of the snapshot policy|string|
|**schedule**|Time the snapshot is scheduled to be taken|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**fordisplay**|Is this policy for display to the regular user|boolean|
|**timezone**|The time zone of the snapshot policy|string|
|**volumeid**|The ID of the disk volume|string|

## listSnapshots

This command lists all available snapshots for the account.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all available snapshots for the account.:

```shell
$ cmk listSnapshots
```

`listSnapshots`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**zoneid**|List snapshots by zone ID|uuid|255|false|
|**volumeid**|The ID of the disk volume|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**keyword**|List by keyword|string|255|false|
|**id**|Lists snapshot by snapshot ID|uuid|255|false|
|**page**|Page|integer|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**account**|List resources by account. Must be used with the domainId parameter.|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**ids**|The IDs of the snapshots, mutually exclusive with ID|list|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**intervaltype**|Valid values are HOURLY, DAILY, WEEKLY, and MONTHLY|string|255|false|
|**snapshottype**|Valid values are MANUAL or RECURRING|string|255|false|
|**name**|Lists snapshot by snapshot name|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**zoneid**|ID of the availability zone|string|
|**volumeid**|ID of the disk volume|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**intervaltype**|Valid types are hourly, daily, weekly, monthly, template, and none|string|
|**locationtype**|Valid location types are primary and secondary|string|
|**state**|The state of the snapshot. BackedUp means that snapshot is ready to be used; Creating - the snapshot is being allocated on the primary storage; BackingUp - the snapshot is being backed up on secondary storage.|state|
|**volumetype**|Type of the disk volume|string|
|**name**|Name of the snapshot|string|
|**project**|The project name of the snapshot|string|
|**snapshottype**|The type of the snapshot|string|
|**created**|The date the snapshot was created|date|
|**virtualsize**|Virtual size of backed up snapshot on image store|long|
|**osdisplayname**|Display name of the OS on volume|string|
|**account**|The account associated with the snapshot|string|
|**revertable**|Indicates whether the underlying storage supports reverting the volume to this snapshot|boolean|
|**volumename**|Name of the disk volume|string|
|**physicalsize**|Physical size of backed up snapshot on image store|long|
|**projectid**|The project ID of the snapshot|string|
|**domain**|The domain name of the snapshot's account|string|
|**domainid**|The domain ID of the snapshot's account|string|
|**tags**|The list of resource tags associated with snapshot|list|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**id**|ID of the snapshot|string|
|**ostypeid**|ID of the os on volume|string|

# SSH

## createSSHKeyPair

This command creates a new keypair and returns the private key.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command creates a new keypair and returns the private key:

```shell
$ cmk createSSHKeyPair
```

`createSSHKeyPair`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|An optional domain ID for the SSH key. If the account parameter is used, domain ID must also be used.|uuid|255|false|
|**account**|An optional account for the SSH key. Must be used with domain ID.|string|255|false|
|**name**|Name of the keypair|string|255|true|
|**projectid**|An optional project for the SSH key|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**privatekey**|Private key|string|
|**domain**|The domain name of the keypair owner|string|
|**account**|The owner of the keypair|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**fingerprint**|Fingerprint of the public key|string|
|**domainid**|The domain ID of the keypair owner|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**name**|Name of the keypair|string|

## deleteSSHKeyPair

This command deletes a keypair by name.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command deletes a keypair by name:

```shell
$ cmk deleteSSHKeyPair
```

`deleteSSHKeyPair`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**name**|Name of the keypair|string|255|true|
|**projectid**|The project associated with keypair|uuid|255|false|
|**account**|The account associated with the keypair. Must be used with the domain ID parameter.|string|255|false|
|**domainid**|The domain ID associated with the keypair|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|

## registerSSHKeyPair

This command registers a public key in a keypair under a certain name.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command registers a public key in a keypair under a certain name:

```shell
$ cmk registerSSHKeyPair
```

`registerSSHKeyPair`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**name**|Name of the keypair|string|255|true|
|**account**|An optional account for the ssh key. Must be used with domain ID.|string|255|false|
|**publickey**|Public key material of the keypair|string|5120|true|
|**projectid**|An optional project for the SSH key|uuid|255|false|
|**domainid**|An optional domain ID for the SSH key. If the account parameter is used, domain ID must also be used.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**name**|Name of the keypair|string|
|**domainid**|The domain ID of the keypair owner|string|
|**fingerprint**|Fingerprint of the public key|string|
|**domain**|The domain name of the keypair owner|string|
|**account**|The owner of the keypair|string|

# Template

## copyTemplate

This command copies a template from one zone to another.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command copies a template from one zone to another:

```shell
$ cmk copyTemplate
```

`copyTemplate`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**destzoneid**|ID of the zone the template is being copied to|uuid|255|false|
|**destzoneids**|A list of IDs of the zones that the template needs to be copied to. Specify this list if the template needs to copied to multiple zones in one go. Do not specify destzoneid and destzoneids together, however one of them is required.|list|255|false|
|**sourcezoneid**|ID of the zone the template is currently hosted on. If not specified and template is cross-zone, then we will sync this template to region wide image store.|uuid|255|false|
|**id**|Template ID|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**created**|The date this template was created|date|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**displaytext**|The template display text|string|
|**size**|The size of the template|long|
|**childtemplates**|If root disk template, then IDs of the datas disk templates this template owns|set|
|**checksum**|Checksum of the template|string|
|**templatetag**|The tag of this template|string|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**domainid**|The ID of the domain to which the template belongs|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**removed**|The date this template was removed|date|
|**domain**|The name of the domain to which the template belongs|string|
|**zoneid**|The ID of the zone for this template|string|
|**ostypename**|The name of the OS type for this template|string|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**id**|The template ID|string|
|**format**|The format of the template|imageformat|
|**name**|The template name|string|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**project**|The project name of the template|string|
|**account**|The account name to which the template belongs|string|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**details**|Additional key/value details tied with template|map|
|**hostname**|The name of the secondary storage host for the template|string|
|**zonename**|The name of the zone for this template|string|
|**projectid**|The project ID of the template|string|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**templatetype**|The type of the template|string|
|**tags**|The list of resource tags associated|set|
|**physicalsize**|The physical size of the template|long|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**ostypeid**|The ID of the OS type for this template|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**accountid**|The account ID to which the template belongs|string|
|**hostid**|The ID of the secondary storage host for the template|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**status**|The status of the template|string|
|**bits**|The processor bit size|int|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|

## createTemplate

This command creates a template of a virtual machine. The virtual machine must be in a STOPPED state. A template created from this command is automatically designated as a private template visible to the account that created it.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a template of a virtual machine:

```shell
$ cmk createTemplate
```

`createTemplate`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**snapshotid**|The ID of the snapshot the template is being created from. Either this parameter, or volumeId has to be passed in.|uuid|255|false|
|**projectid**|Create template for the project|uuid|255|false|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|255|false|
|**ostypeid**|The ID of the OS Type that best represents the OS of this template|uuid|255|true|
|**requireshvm**|True if the template requires HVM, false otherwise|boolean|255|false|
|**url**|Optional, only for baremetal hypervisor. The directory name where template stored on CIFS server|string|2048|false|
|**volumeid**|The ID of the disk volume the template is being created from. Either this parameter, or snapshotId has to be passed in|uuid|255|false|
|**bits**|32 or 64 bit|integer|255|false|
|**details**|Template details in key/value pairs using format details[i].keyname=keyvalue. Example: details[0].hypervisortoolsversion=xenserver61|map|255|false|
|**templatetag**|The tag for this template|string|255|false|
|**ispublic**|True if this template is a public template, false otherwise|boolean|255|false|
|**name**|The name of the template|string|255|true|
|**displaytext**|The display text of the template. This is usually used for display purposes.|string|4096|true|
|**virtualmachineid**|Optional, VM ID. If this presents, it is going to create a baremetal template for VM this ID refers to. This is only for VM whose hypervisor type is BareMetal.|uuid|255|false|
|**passwordenabled**|True if the template supports the password reset feature; default is false|boolean|255|false|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**id**|The template ID|string|
|**hostid**|The ID of the secondary storage host for the template|string|
|**format**|The format of the template|imageformat|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**zonename**|The name of the zone for this template|string|
|**templatetag**|The tag of this template|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**ostypename**|The name of the OS type for this template|string|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**removed**|The date this template was removed|date|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**hypervisor**|The hypervisor on which the template runs|string|
|**size**|The size of the template|long|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**childtemplates**|If root disk template, then IDs of the datas disk templates this template owns|set|
|**zoneid**|The ID of the zone for this template|string|
|**project**|The project name of the template|string|
|**physicalsize**|The physical size of the template|long|
|**details**|Additional key/value details tied with template|map|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**domain**|The name of the domain to which the template belongs|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**templatetype**|The type of the template|string|
|**bits**|The processor bit size|int|
|**status**|The status of the template|string|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**ostypeid**|The ID of the OS type for this template|string|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**accountid**|The account ID to which the template belongs|string|
|**account**|The account name to which the template belongs|string|
|**created**|The date this template was created|date|
|**displaytext**|The template display text|string|
|**checksum**|Checksum of the template|string|
|**projectid**|The project ID of the template|string|
|**tags**|The list of resource tags associated|set|
|**hostname**|The name of the secondary storage host for the template|string|
|**name**|The template name|string|
|**domainid**|The ID of the domain to which the template belongs|string|

## deleteTemplate

This command deletes a template from the system. All virtual machines using the deleted template will not be affected.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a template from the system:

```shell
$ cmk deleteTemplate
```

`deleteTemplate`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**forced**|Force delete a template|boolean|255|false|
|**zoneid**|The ID of zone of the template|uuid|255|false|
|**id**|The ID of the template|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## listNuageVspDomainTemplates

This command lists Nuage VSP domain templates.

<aside class='notice'>
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

## listTemplatePermissions

This command lists template visibility and all accounts that have permissions to view this template.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command list template visibility and all accounts that have permissions to view this template:

```shell
$ cmk listTemplatePermissions
```

`listTemplatePermissions`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The template ID|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**account**|The list of accounts the template is available for|list|
|**id**|The template ID|string|
|**domainid**|The ID of the domain to which the template belongs|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**projectids**|The list of projects the template is available for|list|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## listTemplates

This command lists all public, private, and privileged templates.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all public, private, and privileged templates:

```shell
$ cmk listTemplates
```

`listTemplates`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**showremoved**|Show removed templates as well|boolean|255|false|
|**zoneid**|List templates by zoneId|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**page**|Page|integer|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**templatefilter**|Possible values are "featured", "self", "selfexecutable","sharedexecutable","executable", and "community". * featured : templates that have been marked as featured and public. * self : templates that have been registered or created by the calling user. * selfexecutable : same as self, but only returns templates that can be used to deploy a new VM. * sharedexecutable : templates ready to be deployed that have been granted to the calling user by another user. * executable : templates that are owned by the calling user, or public templates, that can be used to deploy a VM. * community : templates that have been marked as public but not featured. * all : all templates (only usable by admins).|string|255|true|
|**id**|The template ID|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**ids**|The IDs of the templates, mutually exclusive with ID|list|255|false|
|**keyword**|List by keyword|string|255|false|
|**name**|The template name|string|255|false|
|**hypervisor**|The hypervisor for which to restrict the search|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**parenttemplateid**|List datadisk templates by parent template id|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**checksum**|Checksum of the template|string|
|**physicalsize**|The physical size of the template|long|
|**details**|Additional key/value details tied with template|map|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**size**|The size of the template|long|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**ostypeid**|The ID of the OS type for this template|string|
|**created**|The date this template was created|date|
|**hostid**|The ID of the secondary storage host for the template|string|
|**tags**|The list of resource tags associated|set|
|**domainid**|The ID of the domain to which the template belongs|string|
|**zonename**|The name of the zone for this template|string|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**hostname**|The name of the secondary storage host for the template|string|
|**id**|The template ID|string|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**removed**|The date this template was removed|date|
|**domain**|The name of the domain to which the template belongs|string|
|**format**|The format of the template|imageformat|
|**hypervisor**|The hypervisor on which the template runs|string|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**name**|The template name|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**accountid**|The account ID to which the template belongs|string|
|**displaytext**|The template display text|string|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**ostypename**|The name of the OS type for this template|string|
|**projectid**|The project ID of the template|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the template|string|
|**bits**|The processor bit size|int|
|**templatetype**|The type of the template|string|
|**zoneid**|The ID of the zone for this template|string|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|
|**templatetag**|The tag of this template|string|
|**status**|The status of the template|string|
|**account**|The account name to which the template belongs|string|
|**childtemplates**|If root disk template, then ids of the datas disk templates this template owns|set|

## updateTemplate

This command updates attributes of a template.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command updates attributes of a template:

```shell
$ cmk updateTemplate
```

`updateTemplate`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**format**|The format for the image|string|255|false|
|**id**|The ID of the image file|uuid|255|true|
|**ostypeid**|The ID of the OS type that best represents the OS of this image|uuid|255|false|
|**name**|The name of the image file|string|255|false|
|**sortkey**|Sort key of the template, integer|integer|255|false|
|**displaytext**|The display text of the image|string|4096|false|
|**isrouting**|True if the template type is routing i.e., if template is used to deploy router|boolean|255|false|
|**isdynamicallyscalable**|True if template/ISO contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|255|false|
|**cleanupdetails**|Optional boolean field, which indicates if details should be cleaned up or not (if set to true, details removed for this resource, details field ignored; if false or not set, no action)|boolean|255|false|
|**bootable**|True if image is bootable, false otherwise; available only for updateIso API|boolean|255|false|
|**requireshvm**|True if the template requires HVM, false otherwise; available only for updateTemplate API|boolean|255|false|
|**details**|Details in key/value pairs using format details[i].keyname=keyvalue. Example: details[0].hypervisortoolsversion=xenserver61|map|255|false|
|**passwordenabled**|True if the image supports the password reset feature; default is false|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**details**|Additional key/value details tied with template|map|
|**isextractable**|True if the template is extractable, false otherwise|boolean|
|**parenttemplateid**|If Datadisk template, then ID of the root disk template this template belongs to|string|
|**projectid**|The project ID of the template|string|
|**checksum**|Checksum of the template|string|
|**templatetag**|The tag of this template|string|
|**sshkeyenabled**|True if template is sshkey enabled, false otherwise|boolean|
|**directdownload**|KVM Only: true if template is directly downloaded to Primary Storage bypassing Secondary Storage|boolean|
|**size**|The size of the template|long|
|**templatetype**|The type of the template|string|
|**accountid**|The account ID to which the template belongs|string|
|**ostypeid**|The ID of the OS type for this template|string|
|**isready**|True if the template is ready to be deployed from, false otherwise|boolean|
|**isfeatured**|True if this template is a featured template, false otherwise|boolean|
|**ispublic**|True if this template is a public template, false otherwise|boolean|
|**bits**|The processor bit size|int|
|**zoneid**|The ID of the zone for this template|string|
|**sourcetemplateid**|The template ID of the parent template if present|string|
|**crossZones**|True if the template is managed across all Zones, false otherwise|boolean|
|**physicalsize**|The physical size of the template|long|
|**id**|The template ID|string|
|**name**|The template name|string|
|**bootable**|True if the ISO is bootable, false otherwise|boolean|
|**ostypename**|The name of the OS type for this template|string|
|**project**|The project name of the template|string|
|**domain**|The name of the domain to which the template belongs|string|
|**created**|The date this template was created|date|
|**childtemplates**|If root disk template, then IDs of the datas disk templates this template owns|set|
|**removed**|The date this template was removed|date|
|**passwordenabled**|True if the reset password feature is enabled, false otherwise|boolean|
|**format**|The format of the template|imageformat|
|**domainid**|The ID of the domain to which the template belongs|string|
|**status**|The status of the template|string|
|**hostid**|The ID of the secondary storage host for the template|string|
|**tags**|The list of resource tags associated|set|
|**displaytext**|The template display text|string|
|**isdynamicallyscalable**|True if template contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**hostname**|The name of the secondary storage host for the template|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**zonename**|The name of the zone for this template|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**account**|The account name to which the template belongs|string|

## updateTemplatePermissions

This command updates a template visibility permissions. A public template is visible to all accounts within the same domain. A private template is visible only to the owner of the template. A priviledged template is a private template with account permissions added. Only accounts specified under the template permissions are visible to them.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command updates a template visibility permissions:

```shell
$ cmk updateTemplatePermissions
```

`updateTemplatePermissions`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**ispublic**|True for public template/ISO, false for private templates/ISOs|boolean|255|false|
|**id**|The template ID|uuid|255|true|
|**isfeatured**|True for featured template/ISO, false otherwise|boolean|255|false|
|**isextractable**|True if the template/ISO is extractable, false otherwise. Can be set only by root admin.|boolean|255|false|
|**accounts**|A comma delimited list of accounts. If specified, "op" parameter has to be passed in.|list|255|false|
|**projectids**|A comma delimited list of projects. If specified, "op" parameter has to be passed in.|list|255|false|
|**op**|Permission operator (add, remove, reset)|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|

# User

## getVirtualMachineUserData

This command returns user data associated with the VM.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command returns user data associated with the VM:

```shell
$ cmk getVirtualMachineUserData
```

`getVirtualMachineUserData`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**virtualmachineid**|The ID of the virtual machine|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**userdata**|Base 64 encoded VM user data|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**virtualmachineid**|The ID of the virtual machine|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

# Virtual machine

## addNicToVirtualMachine

This command adds VM to specified network by creating a NIC.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command adds VM to specified network by creating a NIC:

```shell
$ cmk addNicToVirtualMachine
```

`addNicToVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**macaddress**|Mac Address for the new network|string|255|false|
|**dhcpoptions**|DHCP options which are passed to the nic Example: dhcpoptions[0].dhcp:114=url&dhcpoptions[0].dhcp:66=www.test.com|map|255|false|
|**virtualmachineid**|Virtual Machine ID|uuid|255|true|
|**ipaddress**|IP Address for the new network|string|255|false|
|**networkid**|Network ID|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**cpuspeed**|The speed of each CPU|integer|
|**created**|The date when this virtual machine was created|date|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**memorykbs**|The memory used by the VM|long|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**ostypeid**|OS type ID of the VM|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**id**|The ID of the virtual machine|string|
|**nic**|The list of nics associated with VM|set|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**guestosid**|OS type ID of the virtual machine|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**project**|The project name of the VM|string|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**memorytargetkbs**|The target memory in VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**projectid**|The project ID of the VM|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**password**|The password (if exists) of the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**templatedisplaytext**| an alternate display text of the template for the virtual machine|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**hostid**|The ID of the host for the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**username**|The user's name who deployed the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**name**|The name of the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**hostname**|The name of the host for the virtual machine|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**details**|VM details in key/value pairs|map|
|**state**|The state of the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**tags**|The list of resource tags associated|set|
|**groupid**|The group ID of the virtual machine|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**keypair**|SSH key-pair|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**group**|The group name of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|

## changeServiceForVirtualMachine

This command changes the service offering for a virtual machine. The virtual machine must be in a "Stopped" state for this command to take effect.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command changes the service offering for a virtual machine:

```shell
$ cmk changeServiceForVirtualMachine
```

`changeServiceForVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**serviceofferingid**|The service offering ID to apply to the virtual machine|uuid|255|true|
|**id**|The ID of the virtual machine|uuid|255|true|
|**details**|Name value pairs of custom parameters for CPU, memory and CPU number. example details[i].name=value.|map|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**hostname**|The name of the host for the virtual machine|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**guestosid**|OS type ID of the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**nic**|The list of nics associated with VM|set|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**displayvm**|An optional field whether to the display the vm to the end user or not|boolean|
|**memorykbs**|The memory used by the VM|long|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**state**|The state of the virtual machine|string|
|**keypair**|SSH key-pair|string|
|**username**|The user's name who deployed the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**projectid**|The project ID of the VM|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the VM|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**hypervisor**|The hypervisor on which the template runs|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**cpuspeed**|The speed of each CPU|integer|
|**id**|The ID of the virtual machine|string|
|**created**|The date when this virtual machine was created|date|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**details**|VM details in key/value pairs|map|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**ostypeid**|OS type ID of the VM|string|
|**publicip**|Public IP address id associated with VM via Static nat rule|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**tags**|The list of resource tags associated|set|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**memorytargetkbs**|The target memory in VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**password**|The password (if exists) of the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**groupid**|The group ID of the virtual machine|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**account**|The account associated with the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**group**|The group name of the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**diskioread**|The read (io) of disk on the VM|long|
|**cpunumber**|The number of cpu this virtual machine is running with|integer|
|**name**|The name of the virtual machine|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|

## listVirtualMachines

This command lists the virtual machines owned by the account.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists the virtual machines owned by the account:

```shell
$ cmk listVirtualMachines
```

`listVirtualMachines`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**podid**|The pod ID|uuid|255|false|
|**name**|Name of the virtual machine (a substring match is made against the parameter value, data for all matching VMs will be returned)|string|255|false|
|**hostid**|The host ID|uuid|255|false|
|**keypair**|List VMs by SSH keypair name|string|255|false|
|**templateid**|List VMs by template|uuid|255|false|
|**storageid**|The storage ID where VM's volumes belong to|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**isoid**|List VMs by ISO|uuid|255|false|
|**zoneid**|The availability zone ID|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**ids**|The IDs of the virtual machines, mutually exclusive with ID|list|255|false|
|**pagesize**|Page size|integer|255|false|
|**displayvm**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**state**|State of the virtual machine. Possible values are: Running, Stopped, Present, Destroyed, Expunged. Present is used for the state equal not destroyed.|string|255|false|
|**details**|Comma separated list of host details requested, value can be a list of [all, group, nics, stats, secgrp, tmpl, servoff, diskoff, iso, volume, min, affgrp]. If no parameter is passed in, the details will be defaulted to all.|list|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**affinitygroupid**|List VMs by affinity group|uuid|255|false|
|**networkid**|List by network ID|uuid|255|false|
|**forvirtualnetwork**|List by network type; true if need to list VMs using Virtual Network, false otherwise|boolean|255|false|
|**serviceofferingid**|List by the service offering|uuid|255|false|
|**groupid**|The group ID|uuid|255|false|
|**userid**|The user ID that created the VM and is under the account that owns the VM|uuid|255|false|
|**page**|Page|integer|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**id**|The ID of the virtual machine|uuid|255|false|
|**hypervisor**|The target hypervisor for the template|string|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**vpcid**|List VMs by VPC|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**rootdevicetype**|Device type of the root volume|string|
|**memorykbs**|The memory used by the VM|long|
|**publicipid**|Public IP address id associated with VM via Static nat rule|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**created**|The date when this virtual machine was created|date|
|**state**|The state of the virtual machine|string|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**password**|The password (if exists) of the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**hostname**|The name of the host for the virtual machine|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**project**|The project name of the VM|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**keypair**|SSH key-pair|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**servicestate**|State of the Service from LB rule|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**hostid**|The ID of the host for the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**id**|The ID of the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**nic**|The list of nics associated with VM|set|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**cpuspeed**|The speed of each CPU|integer|
|**group**|The group name of the virtual machine|string|
|**ostypeid**|OS type ID of the VM|string|
|**memorytargetkbs**|The target memory in VM|long|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**details**|VM details in key/value pairs|map|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**groupid**|The group ID of the virtual machine|string|
|**username**|The user's name who deployed the virtual machine|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**name**|The name of the virtual machine|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**templatename**|The name of the template for the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**tags**|The list of resource tags associated|set|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**guestosid**|OS type ID of the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**projectid**|The project ID of the VM|string|

## listVirtualMachinesMetrics

This command lists VM metrics.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists VM metrics:

```shell
$ cmk listVirtualMachinesMetrics
```

`listVirtualMachinesMetrics`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**isoid**|List VMs by ISO|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**hypervisor**|The target hypervisor for the template|string|255|false|
|**state**|State of the virtual machine. Possible values are: Running, Stopped, Present, Destroyed, Expunged. Present is used for the state equal not destroyed.|string|255|false|
|**affinitygroupid**|List VMs by affinity group|uuid|255|false|
|**page**|Page|integer|255|false|
|**zoneid**|The availability zone ID|uuid|255|false|
|**name**|Name of the virtual machine (a substring match is made against the parameter value, data for all matching VMs will be returned)|string|255|false|
|**forvirtualnetwork**|List by network type; true if need to list VMs using Virtual Network, false otherwise|boolean|255|false|
|**keypair**|List VMs by SSH keypair name|string|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**displayvm**|List resources by display flag; only ROOT admin is eligible to pass this parameter|boolean|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**networkid**|List by network ID|uuid|255|false|
|**details**|Comma separated list of host details requested, value can be a list of [all, group, nics, stats, secgrp, tmpl, servoff, diskoff, iso, volume, min, affgrp]. If no parameter is passed in, the details will be defaulted to all.|list|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**storageid**|The storage ID where VM's volumes belong to|uuid|255|false|
|**hostid**|The host ID|uuid|255|false|
|**userid**|The user ID that created the VM and is under the account that owns the VM|uuid|255|false|
|**hostid**|The host ID|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**podid**|The pod ID|uuid|255|false|
|**podid**|The pod ID|uuid|255|false|
|**storageid**|The storage ID where VM's volumes belong to|uuid|255|false|
|**id**|The ID of the virtual machine|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**groupid**|The group ID|uuid|255|false|
|**account**|List resources by account. Must be used with the domainId parameter.|string|255|false|
|**serviceofferingid**|List by the service offering|uuid|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**vpcid**|List VMs by VPC|uuid|255|false|
|**templateid**|List VMs by template|uuid|255|false|
|**ids**|The IDs of the virtual machines, mutually exclusive with ID|list|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**cpuspeed**|The speed of each CPU|integer|
|**password**|The password (if exists) of the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**networkwrite**|Network write in MiB|string|
|**cputotal**|The total CPU capacity in Ghz|string|
|**id**|The ID of the virtual machine|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**publicipid**|Public IP address id associated with VM via Static nat rule|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**state**|The state of the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**diskwrite**|Disk write in MiB|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**keypair**|SSH key-pair|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**diskiopstotal**|The total disk IOPS|long|
|**guestosid**|OS type ID of the virtual machine|string|
|**group**|The group name of the virtual machine|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**servicestate**|State of the Service from LB rule|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**memorytotal**|The total memory capacity in GiB|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**created**|The date when this virtual machine was created|date|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**memorykbs**|The memory used by the VM|long|
|**memorytargetkbs**|The target memory in VM|long|
|**projectid**|The project ID of the VM|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**diskioread**|The read (io) of disk on the VM|long|
|**diskread**|Disk read in MiB|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**memory**|The memory allocated for the virtual machine|integer|
|**details**|VM details in key/value pairs|map|
|**tags**|The list of resource tags associated|set|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**ostypeid**|OS type ID of the vm|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**ipaddress**|The VM's primary IP address|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**nic**|The list of nics associated with VM|set|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**networkread**|Network read in MiB|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**username**|The user's name who deployed the virtual machine|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**groupid**|The group ID of the virtual machine|string|
|**project**|The project name of the VM|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**hostname**|The name of the host for the virtual machine|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**account**|The account associated with the virtual machine|string|
|**name**|The name of the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**userid**|The user's ID who deployed the virtual machine|string|

## rebootVirtualMachine

This command reboots a virtual machine.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command reboots a virtual machine:

```shell
$ cmk rebootVirtualMachine
```

`rebootVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the virtual machine|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**groupid**|The group ID of the virtual machine|string|
|**memorykbs**|The memory used by the VM|long|
|**tags**|The list of resource tags associated|set|
|**guestosid**|OS type ID of the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**hostname**|The name of the host for the virtual machine|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**hypervisor**|The hypervisor on which the template runs|string|
|**state**|The state of the virtual machine|string|
|**servicestate**|State of the Service from LB rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**displayname**|User generated name. The name of the virtual machine is returned if no displayname exists.|string|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**group**|The group name of the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**username**|The user's name who deployed the virtual machine|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**cpuspeed**|The speed of each CPU|integer|
|**zoneid**|The ID of the availablility zone for the virtual machine|string|
|**ostypeid**|OS type ID of the VM|string|
|**project**|The project name of the VM|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**memorytargetkbs**|The target memory in VM|long|
|**rootdeviceid**|Device ID of the root volume|long|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**created**|The date when this virtual machine was created|date|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**rootdevicetype**|Device type of the root volume|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**memory**|The memory allocated for the virtual machine|integer|
|**details**|VM details in key/value pairs.|map|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**name**|The name of the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**projectid**|The project ID of the VM|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**nic**|The list of nics associated with vm|set|
|**id**|The ID of the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**password**|The password (if exists) of the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**keypair**|SSH key-pair|string|

## resetPasswordForVirtualMachine

This command resets the password for virtual machine. The virtual machine must be in a "Stopped" state and the template must already support this feature for this command to take effect.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command resets the password for virtual machine:

```shell
$ cmk resetPasswordForVirtualMachine
```

`resetPasswordForVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the virtual machine|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**groupid**|The group ID of the virtual machine|string|
|**name**|The name of the virtual machine|string|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**hypervisor**|The hypervisor on which the template runs|string|
|**created**|The date when this virtual machine was created|date|
|**memorykbs**|The memory used by the VM|long|
|**rootdevicetype**|Device type of the root volume|string|
|**ostypeid**|OS type ID of the VM|string|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**state**|The state of the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**keypair**|SSH key-pair|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**servicestate**|State of the Service from LB rule|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**memorytargetkbs**|The target memory in VM|long|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**projectid**|The project ID of the VM|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**guestosid**|OS type ID of the virtual machine|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**tags**|The list of resource tags associated|set|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**id**|The ID of the virtual machine|string|
|**hostname**|The name of the host for the virtual machine|string|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**templatename**|The name of the template for the virtual machine|string|
|**username**|The user's name who deployed the virtual machine|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**project**|The project name of the VM|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**nic**|The list of nics associated with VM|set|
|**password**|The password (if exists) of the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**details**|VM details in key/value pairs|map|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**group**|The group name of the virtual machine|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**cpuspeed**|The speed of each CPU|integer|
|**vgpu**|The VGPU type used by the virtual machine|string|

## resetSSHKeyForVirtualMachine

This command resets the SSH Key for virtual machine.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command resets the SSH Key for virtual machine:

```shell
$ cmk resetSSHKeyForVirtualMachine
```

`resetSSHKeyForVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|An optional account for the ssh key. Must be used with domain ID.|string|255|false|
|**domainid**|An optional domain ID for the virtual machine. If the account parameter is used, domain ID must also be used.|uuid|255|false|
|**projectid**|An optional project for the SSH key|uuid|255|false|
|**keypair**|Name of the SSH key pair used to login to the virtual machine|string|255|true|
|**id**|The ID of the virtual machine|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**memory**|The memory allocated for the virtual machine|integer|
|**nic**|The list of nics associated with VM|set|
|**username**|The user's name who deployed the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**password**|The password (if exists) of the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**rootdevicetype**|Device type of the root volume|string|
|**guestosid**|OS type ID of the virtual machine|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**details**|VM details in key/value pairs|map|
|**keypair**|SSH key-pair|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**ostypeid**|OS type ID of the VM|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**state**|The state of the virtual machine|string|
|**tags**|The list of resource tags associated|set|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**groupid**|The group ID of the virtual machine|string|
|**hypervisor**|The hypervisor on which the template runs|string|
|**created**|The date when this virtual machine was created|date|
|**cpuspeed**|The speed of each CPU|integer|
|**project**|The project name of the VM|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**publicipid**|Public IP address ID associated with vm via Static nat rule|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|
|**group**|The group name of the virtual machine|string|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**memorykbs**|The memory used by the VM|long|
|**account**|The account associated with the virtual machine|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**hostname**|The name of the host for the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**memorytargetkbs**|The target memory in VM|long|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**id**|The ID of the virtual machine|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**name**|The name of the virtual machine|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**templatename**|The name of the template for the virtual machine|string|
|**projectid**|The project ID of the VM|string|

## restoreVirtualMachine

This command restores a VM to original template/ISO or new template/ISO.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command restores a VM to original template/ISO or new template/ISO:

```shell
$ cmk restoreVirtualMachine
```

`restoreVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**templateid**|An optional template ID to restore VM from the new template. This can be an ISO ID in case of restore VM deployed using ISO.|uuid|255|false|
|**virtualmachineid**|Virtual Machine ID|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**nic**|The list of nics associated with VM|set|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**name**|The name of the virtual machine|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**projectid**|The project ID of the VM|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**cpuspeed**|The speed of each CPU|integer|
|**hypervisor**|The hypervisor on which the template runs|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**ostypeid**|OS type ID of the VM|string|
|**servicestate**|State of the Service from LB rule|string|
|**guestosid**|OS type ID of the virtual machine|string|
|**group**|The group name of the virtual machine|string|
|**memorykbs**|The memory used by the VM|long|
|**username**|The user's name who deployed the virtual machine|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**tags**|The list of resource tags associated|set|
|**rootdevicetype**|Device type of the root volume|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**id**|The ID of the virtual machine|string|
|**created**|The date when this virtual machine was created|date|
|**hostname**|The name of the host for the virtual machine|string|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**memorytargetkbs**|The target memory in VM|long|
|**project**|The project name of the VM|string|
|**state**|The state of the virtual machine|string|
|**details**|VM details in key/value pairs|map|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**keypair**|SSH key-pair|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**groupid**|The group ID of the virtual machine|string|
|**publicipid**|Public IP address id associated with VM via Static nat rule|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**password**|The password (if exists) of the virtual machine|string|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**account**|The account associated with the virtual machine|string|

## updateDefaultNicForVirtualMachine

This command changes the default NIC on a VM.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command changes the default NIC on a VM:

```shell
$ cmk updateDefaultNicForVirtualMachine
```

`updateDefaultNicForVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**nicid**|NIC ID|uuid|255|true|
|**virtualmachineid**|Virtual Machine ID|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**state**|The state of the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**guestosid**|Os type ID of the virtual machine|string|
|**details**|VM details in key/value pairs|map|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**servicestate**|State of the Service from LB rule|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**ostypeid**|OS type ID of the VM|string|
|**rootdevicetype**|Device type of the root volume|string|
|**group**|The group name of the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**hostname**|The name of the host for the virtual machine|string|
|**account**|The account associated with the virtual machine|string|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**cpuspeed**|The speed of each CPU|integer|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**name**|The name of the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**groupid**|The group ID of the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**cpunumber**|The number of CPU this virtual machine is running with|integer|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**project**|The project name of the VM|string|
|**username**|The user's name who deployed the virtual machine|string|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**nic**|The list of nics associated with VM|set|
|**diskkbswrite**|The write (bytes) of disk on the VM|long|
|**memorykbs**|The memory used by the VM|long|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**memorytargetkbs**|The target memory in VM|long|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**tags**|The list of resource tags associated|set|
|**displayname**|User generated name. The name of the virtual machine is returned if no displayname exists.|string|
|**hostid**|The ID of the host for the virtual machine|string|
|**created**|The date when this virtual machine was created|date|
|**projectid**|The project ID of the VM|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**id**|The ID of the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**password**|The password (if exists) of the virtual machine|string|
|**keypair**|SSH key-pair|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**diskioread**|The read (io) of disk on the VM|long|
|**hypervisor**|The hypervisor on which the template runs|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|

## updateVirtualMachine

This command updates properties of a virtual machine. The VM has to be stopped and restarted for the new properties to take effect. UpdateVirtualMachine does not first check whether the VM is stopped. Therefore, stop the VM manually before issuing this call.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command updates properties of a virtual machine:

```shell
$ cmk updateVirtualMachine
```

`updateVirtualMachine`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**securitygroupids**|List of security group IDs to be applied on the virtual machine|list|255|false|
|**ostypeid**|The ID of the OS type that best represents this VM.|uuid|255|false|
|**group**|Group of the virtual machine|string|255|false|
|**dhcpoptionsnetworklist**|DHCP options which are passed to the VM on start up Example: dhcpoptionsnetworklist[0].dhcp:114=url&dhcpoptionsetworklist[0].networkid=networkid&dhcpoptionsetworklist[0].dhcp:66=www.test.com|map|255|false|
|**displayname**|User generated name|string|255|false|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory|boolean|255|false|
|**cleanupdetails**|Optional boolean field, which indicates if details should be cleaned up or not (if set to true, details removed for this resource, details field ignored; if false or not set, no action)|boolean|255|false|
|**name**|New host name of the VM. The VM has to be stopped/started for this update to take affect.|string|255|false|
|**instancename**|Instance name of the user VM|string|255|false|
|**details**|Details in key/value pairs|map|255|false|
|**customid**|An optional field, in case you want to set a custom id to the resource. Allowed to Root Admins only.|string|255|false|
|**userdata**|An optional binary data that can be sent to the virtual machine upon a successful deployment. This binary data must be base64 encoded before adding it to the request. Using HTTP GET (via querystring), you can send up to 2KB of data after base64 encoding. Using HTTP POST(via POST body), you can send up to 32K of data after base64 encoding.|string|32768|false|
|**securitygroupnames**|Comma separated list of security groups names that going to be applied to the virtual machine. Should be passed only when VM is created from a zone with Basic Network support. Mutually exclusive with securitygroupids parameter.|list|255|false|
|**displayvm**|An optional field, whether to the display the VM to the end user or not|boolean|255|false|
|**id**|The ID of the virtual machine|uuid|255|true|
|**haenable**|True if high-availability is enabled for the virtual machine, false otherwise|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**securitygroup**|List of security groups associated with the virtual machine|set|
|**cpuused**|The amount of the VM's CPU currently used|string|
|**created**|The date when this virtual machine was created|date|
|**hostname**|The name of the host for the virtual machine|string|
|**rootdevicetype**|Device type of the root volume|string|
|**diskkbsread**|The read (bytes) of disk on the VM|long|
|**domain**|The name of the domain in which the virtual machine exists|string|
|**diskkbswrite**|The write (bytes) of disk on the vm|long|
|**cpuspeed**|The speed of each CPU|integer|
|**account**|The account associated with the virtual machine|string|
|**memoryintfreekbs**|The internal memory thats free in VM|long|
|**groupid**|The group ID of the virtual machine|string|
|**diskioread**|The read (io) of disk on the VM|long|
|**diskofferingname**|The name of the disk offering of the virtual machine|string|
|**projectid**|The project ID of the VM|string|
|**serviceofferingid**|The ID of the service offering of the virtual machine|string|
|**haenable**|True if high-availability is enabled, false otherwise|boolean|
|**project**|The project name of the VM|string|
|**userid**|The user's ID who deployed the virtual machine|string|
|**diskiowrite**|The write (io) of disk on the VM|long|
|**name**|The name of the virtual machine|string|
|**displayvm**|An optional field whether to the display the VM to the end user or not|boolean|
|**hostid**|The ID of the host for the virtual machine|string|
|**publicip**|Public IP address ID associated with VM via Static nat rule|string|
|**ostypeid**|OS type ID of the VM|string|
|**guestosid**|Os type ID of the virtual machine|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**templatename**|The name of the template for the virtual machine|string|
|**cpunumber**|The number of cpu this virtual machine is running with|integer|
|**zoneid**|The ID of the availability zone for the virtual machine|string|
|**zonename**|The name of the availability zone for the virtual machine|string|
|**memory**|The memory allocated for the virtual machine|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**nic**|The list of nics associated with VM|set|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**instancename**|Instance name of the user VM; this parameter is returned to the ROOT admin only|string|
|**password**|The password (if exists) of the virtual machine|string|
|**vgpu**|The VGPU type used by the virtual machine|string|
|**rootdeviceid**|Device ID of the root volume|long|
|**details**|VM details in key/value pairs|map|
|**state**|The state of the virtual machine|string|
|**networkkbsread**|The incoming network traffic on the VM|long|
|**publicipid**|Public IP address ID associated with VM via Static nat rule|string|
|**forvirtualnetwork**|The virtual network for the service offering|boolean|
|**hypervisor**|The hypervisor on which the template runs|string|
|**passwordenabled**|True if the password rest feature is enabled, false otherwise|boolean|
|**displayname**|User generated name. The name of the virtual machine is returned if no display name exists.|string|
|**servicestate**|State of the Service from LB rule|string|
|**group**|The group name of the virtual machine|string|
|**isdynamicallyscalable**|True if VM contains XS/VMWare tools in order to support dynamic scaling of VM CPU/memory.|boolean|
|**serviceofferingname**|The name of the service offering of the virtual machine|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**tags**|The list of resource tags associated|set|
|**username**|The user's name who deployed the virtual machine|string|
|**affinitygroup**|List of affinity groups associated with the virtual machine|set|
|**diskofferingid**|The ID of the disk offering of the virtual machine|string|
|**memorykbs**|The memory used by the VM|long|
|**memorytargetkbs**|The target memory in VM|long|
|**id**|The ID of the virtual machine|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**networkkbswrite**|The outgoing network traffic on the host|long|
|**keypair**|SSH key-pair|string|
|**domainid**|The ID of the domain in which the virtual machine exists|string|

# VM Group

## deleteInstanceGroup

This command deletes a VM group.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command deletes a VM group:

```shell
$ cmk deleteInstanceGroup
```

`deleteInstanceGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the instance group|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**displaytext**|Any text associated with the success or failure|string|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listInstanceGroups

This command lists VM groups.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists VM groups:

```shell
$ cmk listInstanceGroups
```

`listInstanceGroups`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**name**|List instance groups by name|string|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves.|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**id**|List instance groups by ID|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**page**|Page|integer|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**created**|Time and date the instance group was created|date|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**account**|The account owning the instance group|string|
|**project**|The project name of the instance group|string|
|**domain**|The domain name of the instance group|string|
|**id**|The ID of the instance group|string|
|**domainid**|The domain ID of the instance group|string|
|**projectid**|The project ID of the instance group|string|
|**name**|The name of the instance group|string|

## updateInstanceGroup

This command updates a VM group.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command updates a VM group:

```shell
$ cmk updateInstanceGroup
```

`updateInstanceGroup`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|Instance group ID|uuid|255|true|
|**name**|New instance group name|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The ID of the instance group|string|
|**project**|The project name of the instance group|string|
|**name**|The name of the instance group|string|
|**account**|The account owning the instance group|string|
|**domain**|The domain name of the instance group|string|
|**projectid**|The project ID of the instance group|string|
|**domainid**|The domain ID of the instance group|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**created**|Time and date the instance group was created|date|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

# Volume

## detachVolume

This command detaches a disk volume from a virtual machine.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command detaches a disk volume from a virtual machine:

```shell
$ cmk detachVolume
```

`detachVolume`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the disk volume|uuid|255|false|
|**virtualmachineid**|The ID of the virtual machine where the volume is detached from|uuid|255|false|
|**deviceid**|The device ID on the virtual machine where volume is detached from|long|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**vmname**|Name of the virtual machine|string|
|**clustername**|Cluster name where the volume is allocated|string|
|**diskBytesReadRate**|Bytes read rate of the disk volume|long|
|**maxiops**|Max IOPS of the disk volume|long|
|**storageid**|ID of the primary storage hosting the disk volume; returned to admin user only|string|
|**vmdisplayname**|Display name of the virtual machine|string|
|**attached**|The date the volume was attached to a VM instance|date|
|**vmstate**|State of the virtual machine|string|
|**projectid**|The project ID of the VPN|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**diskofferingid**|ID of the disk offering|string|
|**project**|The project name of the VPN|string|
|**provisioningtype**|Provisioning type used to create volumes|string|
|**diskofferingname**|Name of the disk offering|string|
|**clusterid**|Cluster ID of the volume|string|
|**snapshotid**|ID of the snapshot from which this volume was created|string|
|**virtualsize**|The bytes actually consumed on disk|long|
|**state**|The state of the disk volume|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**storage**|Name of the primary storage hosting the disk volume|string|
|**serviceofferingdisplaytext**|The display text of the service offering for root disk|string|
|**domain**|The domain associated with the disk volume|string|
|**miniops**|Min IOPS of the disk volume|long|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**diskIopsWriteRate**|IO requests write rate of the disk volume|long|
|**virtualmachineid**|ID of the virtual machine|string|
|**quiescevm**|Need quiesce VM or not when taking snapshot|boolean|
|**path**|The path of the volume|string|
|**domainid**|The ID of the domain associated with the disk volume|string|
|**serviceofferingname**|Name of the service offering for root disk|string|
|**diskBytesWriteRate**|Bytes write rate of the disk volume|long|
|**destroyed**|The boolean state of whether the volume is destroyed or not|boolean|
|**utilization**|The disk utilization|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**account**|The account associated with the disk volume|string|
|**zoneid**|ID of the availability zone|string|
|**diskofferingdisplaytext**|The display text of the disk offering|string|
|**type**|Type of the disk volume (ROOT or DATADISK)|string|
|**created**|The date the disk volume was created|date|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**displayvolume**|An optional field whether to the display the volume to the end user or not|boolean|
|**chaininfo**|The chain info of the volume|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**serviceofferingid**|ID of the service offering for root disk|string|
|**zonename**|Name of the availability zone|string|
|**physicalsize**|The bytes allocated|long|
|**status**|The status of the volume|string|
|**name**|Name of the disk volume|string|
|**deviceid**|The ID of the device on user VM the volume is attahed to. This tag is not returned when the volume is detached.|long|
|**id**|ID of the disk volume|string|
|**isextractable**|True if the volume is extractable, false otherwise|boolean|
|**size**|Size of the disk volume|long|
|**tags**|The list of resource tags associated|set|
|**podname**|Pod name of the volume|string|
|**hypervisor**|Hypervisor the volume belongs to|string|
|**templatename**|The name of the template for the virtual machine|string|
|**storagetype**|Shared or local storage|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**diskIopsReadRate**|IO requests read rate of the disk volume|long|
|**podid**|Pod ID of the volume|string|

## getSolidFireVolumeAccessGroupId

This command gets the SF Volume Access Group ID.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command gets the SF Volume Access Group ID:

```shell
$ cmk getSolidFireVolumeAccessGroupId
```

`getSolidFireVolumeAccessGroupId`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**storageid**|Storage Pool UUID|string|255|true|
|**clusterid**|Cluster UUID|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**solidFireVolumeAccessGroupId**|SolidFire Volume Access Group ID|long|
|**jobstatus**|The current status of the latest async job acting on this object|integer|

## getSolidFireVolumeSize

This command gets the SF volume size including Hypervisor Snapshot Reserve.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command gets the SF volume size including Hypervisor Snapshot Reserve:

```shell
$ cmk getSolidFireVolumeSize
```

`getSolidFireVolumeSize`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**volumeid**|Volume UUID|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**solidFireVolumeSize**|SolidFire Volume Size Including Hypervisor Snapshot Reserve|long|

## getUploadParamsForVolume

This command uploads a data disk to the CloudStack cloud.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command uploads a data disk to the CloudStack cloud:

```shell
$ cmk getUploadParamsForVolume
```

`getUploadParamsForVolume`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|An optional domain ID. If the account parameter is used, domain ID must also be used.|uuid|255|false|
|**zoneid**|The ID of the zone the volume/template is to be hosted on|uuid|255|true|
|**diskofferingid**|The ID of the disk offering. This must be a custom sized offering since during upload of volume/template size is unknown.|uuid|255|false|
|**name**|The name of the volume/template|string|255|true|
|**projectid**|Upload volume/template for the project|uuid|255|false|
|**format**|The format for the volume/template. Possible values include QCOW2, OVA, and VHD.|string|255|true|
|**imagestoreuuid**|Image store uuid|string|255|false|
|**account**|An optional accountName. Must be used with domain ID.|string|255|false|
|**checksum**|The checksum value of this volume/template The parameter containing the checksum will be considered a MD5sum if it is not prefixed and just a plain ascii/utf8 representation of a hexadecimal string. If it is required to use another algorithm the hexadecimal string is to be prefixed with a string of the form, "{<algorithm>}", not including the double quotes. In this <algorithm> is the exact string representing the java supported algorithm, i.e. MD5 or SHA-256. Note that java does not contain an algorithm called SHA256 or one called sha-256, only SHA-256.|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**signature**|Signature to be sent in the POST request|string|
|**expires**|The timestamp after which the signature expires|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**postURL**|POST URL to upload the file to|url|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**metadata**|Encrypted data to be sent in the POST request|string|
|**id**|The template/volume ID|uuid|

## resizeVolume

This command resizes a volume.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command resizes a volume:

```shell
$ cmk resizeVolume
```

`resizeVolume`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**diskofferingid**|New disk offering ID|uuid|255|false|
|**maxiops**|New maximum number of IOPS|long|255|false|
|**miniops**|New minimum number of IOPS|long|255|false|
|**size**|New volume size in GB|long|255|false|
|**shrinkok**|Verify OK to Shrink|boolean|255|false|
|**id**|The ID of the disk volume|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**maxiops**|Max IOPS of the disk volume|long|
|**name**|Name of the disk volume|string|
|**templatename**|The name of the template for the virtual machine|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**account**|The account associated with the disk volume|string|
|**virtualmachineid**|ID of the virtual machine|string|
|**diskBytesReadRate**|Bytes read rate of the disk volume|long|
|**zoneid**|ID of the availability zone|string|
|**utilization**|The disk utilization|string|
|**domainid**|The ID of the domain associated with the disk volume|string|
|**attached**|The date the volume was attached to a VM instance|date|
|**storage**|Name of the primary storage hosting the disk volume|string|
|**type**|Type of the disk volume (ROOT or DATADISK)|string|
|**diskofferingname**|Name of the disk offering|string|
|**diskIopsWriteRate**|IO requests write rate of the disk volume|long|
|**projectid**|The project ID of the VPN|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**quiescevm**|Need quiesce VM or not when taking snapshot|boolean|
|**hypervisor**|Hypervisor the volume belongs to|string|
|**serviceofferingid**|ID of the service offering for root disk|string|
|**provisioningtype**|Provisioning type used to create volumes|string|
|**domain**|The domain associated with the disk volume|string|
|**clusterid**|Cluster ID of the volume|string|
|**id**|ID of the disk volume|string|
|**path**|The path of the volume|string|
|**diskBytesWriteRate**|Bytes write rate of the disk volume|long|
|**virtualsize**|The bytes actually consumed on disk|long|
|**storageid**|ID of the primary storage hosting the disk volume; returned to admin user only|string|
|**diskofferingdisplaytext**|The display text of the disk offering|string|
|**isextractable**|True if the volume is extractable, false otherwise|boolean|
|**vmname**|Name of the virtual machine|string|
|**created**|The date the disk volume was created|date|
|**snapshotid**|ID of the snapshot from which this volume was created|string|
|**state**|The state of the disk volume|string|
|**zonename**|Name of the availability zone|string|
|**podname**|Pod name of the volume|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**status**|The status of the volume|string|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**serviceofferingname**|Name of the service offering for root disk|string|
|**miniops**|Min IOPS of the disk volume|long|
|**tags**|The list of resource tags associated|set|
|**deviceid**|The ID of the device on user VM the volume is attached to. This tag is not returned when the volume is detached.|long|
|**physicalsize**|The bytes allocated|long|
|**destroyed**|The boolean state of whether the volume is destroyed or not|boolean|
|**storagetype**|Shared or local storage|string|
|**project**|The project name of the VPN|string|
|**size**|Size of the disk volume|long|
|**podid**|Pod ID of the volume|string|
|**diskofferingid**|ID of the disk offering|string|
|**displayvolume**|An optional field whether to the display the volume to the end user or not|boolean|
|**clustername**|Cluster name where the volume is allocated|string|
|**serviceofferingdisplaytext**|The display text of the service offering for root disk|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**diskIopsReadRate**|IO requests read rate of the disk volume|long|
|**vmstate**|State of the virtual machine|string|
|**chaininfo**|The chain info of the volume|string|
|**vmdisplayname**|Display name of the virtual machine|string|

## uploadVolume

This command uploads a data disk.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command uploads a data disk:

```shell
$ cmk uploadVolume
```

`uploadVolume`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**checksum**|The checksum value of this volume. The parameter containing the checksum will be considered a MD5sum if it is not prefixed and just a plain Ascii/UTF8 representation of a hexadecimal string. If it is required to use another algorithm, the hexadecimal string is to be prefixed with a string of the form, "{<algorithm>}", not including the double quotes. In this <algorithm> is the exact string representing the Java supported algorithm, i.e. MD5 or SHA-256. Note that Java does not contain an algorithm called SHA256 or one called sha-256, only SHA-256.|string|255|false|
|**account**|An optional accountName. Must be used with domain ID.|string|255|false|
|**diskofferingid**|The ID of the disk offering. This must be a custom sized offering since during uploadVolume volume size is unknown.|uuid|255|false|
|**url**|The URL of where the volume is hosted. Possible URL include http:// and https://.|string|2048|true|
|**imagestoreuuid**|Image store uuid|string|255|false|
|**zoneid**|The ID of the zone the volume is to be hosted on|uuid|255|true|
|**name**|The name of the volume|string|255|true|
|**projectid**|Upload volume for the project|uuid|255|false|
|**format**|The format for the volume. Possible values include QCOW2, OVA, and VHD.|string|255|true|
|**domainid**|An optional domain ID. If the account parameter is used, domain ID must also be used.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**miniops**|Min IOPS of the disk volume|long|
|**project**|The project name of the VPN|string|
|**diskIopsWriteRate**|IO requests write rate of the disk volume|long|
|**displayvolume**|An optional field whether to the display the volume to the end user or not|boolean|
|**size**|Size of the disk volume|long|
|**projectid**|The project ID of the VPN|string|
|**isextractable**|True if the volume is extractable, false otherwise|boolean|
|**templatename**|The name of the template for the virtual machine|string|
|**vmname**|Name of the virtual machine|string|
|**destroyed**|The boolean state of whether the volume is destroyed or not|boolean|
|**storagetype**|Shared or local storage|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**state**|The state of the disk volume|string|
|**vmdisplayname**|Display name of the virtual machine|string|
|**tags**|The list of resource tags associated|set|
|**attached**|The date the volume was attached to a VM instance|date|
|**isoid**|The ID of the ISO attached to the virtual machine|string|
|**id**|ID of the disk volume|string|
|**clusterid**|Cluster ID of the volume|string|
|**serviceofferingdisplaytext**|The display text of the service offering for root disk|string|
|**templatedisplaytext**|An alternate display text of the template for the virtual machine|string|
|**templateid**|The ID of the template for the virtual machine. A -1 is returned if the virtual machine was created from an ISO file.|string|
|**created**|The date the disk volume was created|date|
|**serviceofferingname**|Name of the service offering for root disk|string|
|**domain**|The domain associated with the disk volume|string|
|**podid**|Pod ID of the volume|string|
|**vmstate**|State of the virtual machine|string|
|**domainid**|The ID of the domain associated with the disk volume|string|
|**zoneid**|ID of the availability zone|string|
|**diskBytesReadRate**|Bytes read rate of the disk volume|long|
|**provisioningtype**|Provisioning type used to create volumes|string|
|**podname**|Pod name of the volume|string|
|**diskofferingdisplaytext**|The display text of the disk offering|string|
|**diskofferingname**|Name of the disk offering|string|
|**virtualmachineid**|ID of the virtual machine|string|
|**storage**|Name of the primary storage hosting the disk volume|string|
|**utilization**|The disk utilization|string|
|**isoname**|The name of the ISO attached to the virtual machine|string|
|**deviceid**|The ID of the device on user VM the volume is attahed to. This tag is not returned when the volume is detached.|long|
|**type**|Type of the disk volume (ROOT or DATADISK)|string|
|**isodisplaytext**|An alternate display text of the ISO attached to the virtual machine|string|
|**path**|The path of the volume|string|
|**zonename**|Name of the availability zone|string|
|**diskofferingid**|ID of the disk offering|string|
|**virtualsize**|The bytes actually consumed on disk|long|
|**maxiops**|Max IOPS of the disk volume|long|
|**quiescevm**|Need quiesce VM or not when taking snapshot|boolean|
|**diskBytesWriteRate**|Bytes write rate of the disk volume|long|
|**hypervisor**|Hypervisor the volume belongs to|string|
|**name**|Name of the disk volume|string|
|**account**|The account associated with the disk volume|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**serviceofferingid**|ID of the service offering for root disk|string|
|**status**|The status of the volume|string|
|**snapshotid**|ID of the snapshot from which this volume was created|string|
|**diskIopsReadRate**|IO requests read rate of the disk volume|long|
|**chaininfo**|The chain info of the volume|string|
|**clustername**|Cluster name where the volume is allocated|string|
|**physicalsize**|The bytes allocated|long|
|**storageid**|ID of the primary storage hosting the disk volume; returned to admin user only|string|

# VPC

## createStaticRoute

This command creates a static route.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a static route:

```shell
$ cmk createStaticRoute
```

`createStaticRoute`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**cidr**|Static route CIDR|string|255|true|
|**gatewayid**|The gateway ID we are creating static route for|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**domain**|The domain associated with the static route|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**domainid**|The ID of the domain associated with the static route|string|
|**project**|The project name of the static route|string|
|**cidr**|Static route CIDR|string|
|**tags**|The list of resource tags associated with static route|list|
|**id**|The ID of static route|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**account**|The account associated with the static route|string|
|**gatewayid**|VPC gateway the route is created for|string|
|**projectid**|The project ID of the static route|string|
|**state**|The state of the static route|string|
|**vpcid**|VPC the static route belongs to|string|

## createVPC

This command creates a VPC.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a VPC:

```shell
$ cmk createVPC
```

`createVPC`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**start**|If set to false, the VPC won't start (VPC VR will not get allocated) until its first network gets implemented. True by default.|boolean|255|false|
|**vpcofferingid**|The ID of the VPC offering|uuid|255|true|
|**domainid**|The domain ID associated with the VPC. If used with the account parameter returns the VPC associated with the account for the specified domain.|uuid|255|false|
|**projectid**|Create VPC for the project|uuid|255|false|
|**displaytext**|The display text of the VPC|string|255|true|
|**networkdomain**|VPC network domain. All networks inside the VPC will belong to this domain|string|255|false|
|**cidr**|The CIDR of the VPC. All VPC guest networks' CIDRs should be within this CIDR|string|255|true|
|**zoneid**|The ID of the availability zone|uuid|255|true|
|**fordisplay**|An optional field, whether to the display the VPC to the end user or not|boolean|255|false|
|**account**|The account associated with the VPC. Must be used with the domainId parameter.|string|255|false|
|**name**|The name of the VPC|string|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**networkdomain**|The network domain of the VPC|string|
|**displaytext**|An alternate display text of the VPC|string|
|**network**|The list of networks belonging to the VPC|list|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name of the VPC|string|
|**tags**|The list of resource tags associated with the project|list|
|**vpcofferingid**|VPC offering ID the VPC is created from|string|
|**projectid**|The project ID of the VPC|string|
|**cidr**|The CIDR the VPC|string|
|**state**|State of the VPC. Can be Inactive/Enabled.|string|
|**id**|The ID of the VPC|string|
|**regionlevelvpc**|True if VPC is region level|boolean|
|**domainid**|The domain ID of the VPC owner|string|
|**zoneid**|Zone ID of the VPC|string|
|**redundantvpcrouter**|If this VPC has redundant router|boolean|
|**zonename**|The name of the zone the VPC belongs to|string|
|**domain**|The domain name of the owner|string|
|**created**|The date this VPC was created|date|
|**distributedvpcrouter**|Is VPC uses distributed router for one hop forwarding and host based network ACL's|boolean|
|**restartrequired**|True VPC requires restart|boolean|
|**name**|The name of the VPC|string|
|**service**|The list of supported services|list|
|**fordisplay**|Is VPC for display to the regular user|boolean|
|**account**|The owner of the VPC|string|

## deleteStaticRoute

This command deletes a static route.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a static route:

```shell
$ cmk deleteStaticRoute
```

`deleteStaticRoute`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the static route|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|

## deleteVPC

This command deletes a VPC.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes a VPC:

```shell
$ cmk deleteVPC
```

`deleteVPC`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|The ID of the VPC|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|

## listStaticRoutes

This command lists all static routes.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists all static routes:

```shell
$ cmk listStaticRoutes
```

`listStaticRoutes`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|
|**page**|Page|integer|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**gatewayid**|List static routes by gateway ID|uuid|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domainId till leaves|boolean|255|false|
|**vpcid**|List static routes by VPC ID|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**tags**|List resources by tags (key/value pairs)|map|255|false|
|**id**|List static route by ID|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**projectid**|List objects by project|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**tags**|The list of resource tags associated with static route|list|
|**id**|The ID of static route|string|
|**state**|The state of the static route|string|
|**project**|The project name of the static route|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**projectid**|The project ID of the static route|string|
|**vpcid**|VPC the static route belongs to|string|
|**domainid**|The ID of the domain associated with the static route|string|
|**account**|The account associated with the static route|string|
|**domain**|The domain associated with the static route|string|
|**gatewayid**|VPC gateway the route is created for|string|
|**cidr**|Static route CIDR|string|

## listVPCOfferings

This command lists VPC offerings.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists VPC offerings:

```shell
$ cmk listVPCOfferings
```

`listVPCOfferings`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**state**|List VPC offerings by state|string|255|false|
|**name**|List VPC offerings by name|string|255|false|
|**isdefault**|True if need to list only default VPC offerings. Default value is false.|boolean|255|false|
|**supportedservices**|List VPC offerings supporting certain services|list|255|false|
|**id**|List VPC offerings by ID|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**pagesize**|Page size|integer|255|false|
|**page**|Page|integer|255|false|
|**displaytext**|List VPC offerings by display text|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**isdefault**|True if VPC offering is default, false otherwise|boolean|
|**id**|The ID of the VPC offering|string|
|**created**|The date this VPC offering was created|date|
|**supportsregionLevelvpc**|Indicated if the offering can support region level VPC|boolean|
|**distributedvpcrouter**|Indicates if the VPC offering supports distributed router for one-hop forwarding|boolean|
|**name**|The name of the VPC offering|string|
|**state**|State of the VPC offering. Can be Disabled/Enabled.|string|
|**service**|The list of supported services|list|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|An alternate display text of the VPC offering|string|

## listPrivateGateways

This command lists private gateways.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists private gateways:

```shell
$ cmk listPrivateGateways
```

`listPrivateGateways`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**page**|Page|integer|255|false|
|**state**|List gateways by state|string|255|false|
|**vpcid**|List gateways by VPC|uuid|255|false|
|**keyword**|List by keyword|string|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domain ID till leaves|boolean|255|false|
|**pagesize**|Page size|integer|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**ipaddress**|List gateways by IP address|string|255|false|
|**id**|List private gateway by ID|uuid|255|false|
|**account**|List resources by account. Must be used with the domainId parameter.|string|255|false|
|**vlan**|List gateways by VLAN|string|255|false|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**vpcid**|VPC the private gateway belongs to|string|
|**vlan**|The network implementation URI for the private gateway|string|
|**account**|The account associated with the private gateway|string|
|**projectid**|The project ID of the private gateway|string|
|**ipaddress**|The private gateway's IP address|string|
|**gateway**|The gateway|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**zonename**|The name of the zone the private gateway belongs to|string|
|**id**|The ID of the private gateway|string|
|**state**|State of the gateway, can be Creating, Ready, Deleting|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**sourcenatsupported**|Souce Nat enable status|boolean|
|**zoneid**|Zone ID of the private gateway|string|
|**physicalnetworkid**|The physical network ID|string|
|**domainid**|The ID of the domain associated with the private gateway|string|
|**aclid**|ACL ID set for private gateway|string|
|**netmask**|The private gateway's netmask|string|
|**project**|The project name of the private gateway|string|
|**domain**|The domain associated with the private gateway|string|

## updateVPC

This command updates a VPC.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates a VPC:

```shell
$ cmk updateVPC
```

`updateVPC`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**customid**|An optional field, in case you want to set a custom id to the resource. Allowed to Root Admins only.|string|255|false|
|**name**|The name of the VPC|string|255|false|
|**id**|The ID of the VPC|uuid|255|true|
|**fordisplay**|An optional field, whether to the display the VPC to the end user or not|boolean|255|false|
|**displaytext**|The display text of the VPC|string|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**cidr**|The CIDR the VPC|string|
|**zonename**|The name of the zone the VPC belongs to|string|
|**created**|The date this VPC was created|date|
|**id**|The ID of the VPC|string|
|**distributedvpcrouter**|Is VPC uses distributed router for one hop forwarding and host based network ACL's|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**service**|The list of supported services|list|
|**domainid**|The domain ID of the VPC owner|string|
|**project**|The project name of the VPC|string|
|**projectid**|The project ID of the VPC|string|
|**tags**|The list of resource tags associated with the project|list|
|**fordisplay**|Is VPC for display to the regular user|boolean|
|**restartrequired**|True VPC requires restart|boolean|
|**vpcofferingid**|VPC offering ID the VPC is created from|string|
|**domain**|The domain name of the owner|string|
|**regionlevelvpc**|True if VPC is region level|boolean|
|**name**|The name of the VPC|string|
|**network**|The list of networks belongign to the VPC|list|
|**displaytext**|An alternate display text of the VPC.|string|
|**redundantvpcrouter**|If this VPC has redundant router|boolean|
|**state**|State of the VPC. Can be Inactive/Enabled.|string|
|**networkdomain**|The network domain of the VPC|string|
|**zoneid**|Zone ID of the vpc|string|
|**account**|The owner of the VPC|string|

# VPN

## createRemoteAccessVpn

This command creates a L2TP/IPsec remote access VPN.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command creates a L2TP/IPsec remote access VPN:

```shell
$ cmk createRemoteAccessVpn
```

`createRemoteAccessVpn`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**openfirewall**|If true, firewall rule for source/end public port is automatically created; if false - firewall rule has to be created explicitly. Has value true by default.|boolean|255|false|
|**fordisplay**|An optional field, whether to the display the VPN to the end user or not|boolean|255|false|
|**account**|An optional account for the VPN. Must be used with domain ID.|string|255|false|
|**publicipid**|Public IP address ID of the VPN server|uuid|255|true|
|**iprange**|The range of IP addresses to allocate to VPN clients. The first IP in the range will be taken by the VPN server.|string|255|false|
|**domainid**|An optional domain ID for the VPN. If the account parameter is used, domain ID must also be used.|uuid|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**publicip**|The public IP address of the VPN server|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domain**|The domain name of the account of the remote access VPN|string|
|**projectid**|The project ID of the VPN|string|
|**account**|The account of the remote access VPN|string|
|**id**|The ID of the remote access VPN|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**fordisplay**|Is VPN for display to the regular user|boolean|
|**publicipid**|The public IP address of the VPN server|string|
|**project**|The project name of the VPN|string|
|**presharedkey**|The IPsec preshared key|string|
|**domainid**|The domain ID of the account of the remote access VPN|string|
|**iprange**|The range of IPs to allocate to the clients|string|
|**state**|The state of the rule|string|

## deleteRemoteAccessVpn

This command destroys an L2TP/IPsec remote access VPN.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command destroys an L2TP/IPsec remote access VPN:

```shell
$ cmk deleteRemoteAccessVpn
```

`deleteRemoteAccessVpn`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**publicipid**|Public IP address ID of the VPN server|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobid**|The UUID of the latest async job acting on this object|string|

## deleteVpnConnection

This command delete site to site VPN connection.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command delete site to site VPN connection:

```shell
$ cmk deleteVpnConnection
```

`deleteVpnConnection`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|ID of VPN connection|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|

## deleteVpnCustomerGateway

This command deletes site to site VPN customer gateway.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command deletes site to site VPN customer gateway:

```shell
$ cmk deleteVpnCustomerGateway
```

`deleteVpnCustomerGateway`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|ID of customer gateway|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**success**|True if operation is executed successfully|boolean|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|

## deleteVpnGateway

This command deletes site to site VPN gateway.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command delete site to site VPN gateway:

```shell
$ cmk deleteVpnGateway
```

`deleteVpnGateway`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|Id of customer gateway|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**displaytext**|Any text associated with the success or failure|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**success**|True if operation is executed successfully|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|

## listVpnCustomerGateways

This command lists site to site VPN customer gateways.

<aside class='notice'>
Not asynchronous
</aside>

### Command

> The following command lists site to site VPN customer gateways:

```shell
$ cmk listVpnCustomerGateways
```

`listVpnCustomerGateways`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**domainid**|List only resources belonging to the domain specified|uuid|255|false|
|**pagesize**|Page size|integer|255|false|
|**id**|ID of the customer gateway|uuid|255|false|
|**account**|List resources by account. Must be used with the domain ID parameter.|string|255|false|
|**projectid**|List objects by project|uuid|255|false|
|**isrecursive**|Defaults to false, but if true, lists all resources from the parent specified by the domainId till leaves|boolean|255|false|
|**keyword**|List by keyword|string|255|false|
|**page**|Page|integer|255|false|
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false.|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**ikelifetime**|Lifetime of IKE SA of customer gateway|long|
|**forceencap**|If Force NAT Encapsulation is enabled for customer gateway|boolean|
|**project**|The project name|string|
|**id**|The VPN gateway ID|string|
|**gateway**|Public IP address ID of the customer gateway|string|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**removed**|The date and time the host was removed|date|
|**domainid**|The domain ID of the owner|string|
|**domain**|The domain name of the owner|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**ikepolicy**|IKE policy of customer gateway|string|
|**account**|The owner|string|
|**dpd**|If DPD is enabled for customer gateway|boolean|
|**esppolicy**|IPsec policy of customer gateway|string|
|**cidrlist**|Guest CIDR list of the customer gateway|string|
|**ipsecpsk**|IPsec preshared-key of customer gateway|string|
|**ipaddress**|Guest IP of the customer gateway|string|
|**projectid**|The project ID|string|
|**name**|Name of the customer gateway|string|
|**esplifetime**|Lifetime of ESP SA of customer gateway|long|

## resetVpnConnection

This command resets site to site VPN connection.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command resets site to site VPN connection:

```shell
$ cmk resetVpnConnection
```

`resetVpnConnection`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**account**|An optional account for connection. Must be used with domainId.|string|255|false|
|**domainid**|An optional domainId for connection. If the account parameter is used, domain ID must also be used.|uuid|255|false|
|**id**|ID of VPN connection|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**ipsecpsk**|IPsec Preshared-Key of the customer gateway|string|
|**forceencap**|If Force NAT Encapsulation is enabled for customer gateway|boolean|
|**ikepolicy**|IKE policy of the customer gateway|string|
|**domain**|The domain name of the owner|string|
|**id**|The connection ID|string|
|**ikelifetime**|Lifetime of IKE SA of customer gateway|long|
|**created**|The date and time the host was created|date|
|**gateway**|Public IP address ID of the customer gateway|string|
|**passive**|State of VPN connection|boolean|
|**esplifetime**|Lifetime of ESP SA of customer gateway|long|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**project**|The project name|string|
|**fordisplay**|Is connection for display to the regular user|boolean|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**publicip**|The public IP address|string|
|**state**|State of VPN connection|string|
|**removed**|The date and time the host was removed|date|
|**s2svpngatewayid**|The VPN gateway ID|string|
|**s2scustomergatewayid**|The customer gateway ID|string|
|**account**|The owner|string|
|**dpd**|If DPD is enabled for customer gateway|boolean|
|**projectid**|The project ID|string|
|**domainid**|The domain ID of the owner|string|
|**esppolicy**|ESP policy of the customer gateway|string|
|**cidrlist**|Guest CIDR list of the customer gateway|string|

## updateVpnConnection

This command updates site to site VPN connection.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates site to site VPN connection:

```shell
$ cmk updateVpnConnection
```

`updateVpnConnection`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**id**|ID of VPN connection|uuid|255|true|
|**fordisplay**|An optional field, whether to the display the VPN to the end user or not|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**id**|The connection ID|string|
|**fordisplay**|Is connection for display to the regular user|boolean|
|**ipsecpsk**|IPsec Preshared-Key of the customer gateway|string|
|**ikepolicy**|IKE policy of the customer gateway|string|
|**s2svpngatewayid**|The VPN gateway ID|string|
|**created**|The date and time the host was created|date|
|**state**|State of VPN connection|string|
|**passive**|State of VPN connection|boolean|
|**project**|The project name|string|
|**account**|The owner|string|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**s2scustomergatewayid**|The customer gateway ID|string|
|**removed**|The date and time the host was removed|date|
|**publicip**|The public IP address|string|
|**forceencap**|If Force NAT Encapsulation is enabled for customer gateway|boolean|
|**gateway**|Public IP address ID of the customer gateway|string|
|**esplifetime**|Lifetime of ESP SA of customer gateway|long|
|**ikelifetime**|Lifetime of IKE SA of customer gateway|long|
|**esppolicy**|ESP policy of the customer gateway|string|
|**dpd**|If DPD is enabled for customer gateway|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**cidrlist**|Guest CIDR list of the customer gateway|string|
|**domain**|The domain name of the owner|string|
|**domainid**|The domain ID of the owner|string|
|**projectid**|The project ID|string|

## updateVpnCustomerGateway

This command updates site to site VPN customer gateway.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates site to site VPN customer gateway:

```shell
$ cmk updateVpnCustomerGateway
```

`updateVpnCustomerGateway`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**forceencap**|Force encapsulation for Nat Traversal|boolean|255|false|
|**ipsecpsk**|IPsec Preshared-Key of the customer gateway. Cannot contain newline or double quotes.|string|255|true|
|**account**|The account associated with the gateway. Must be used with the domain ID parameter.|string|255|false|
|**gateway**|Public IP address ID of the customer gateway|string|255|true|
|**cidrlist**|Guest CIDR of the customer gateway|string|255|true|
|**name**|Name of this customer gateway|string|255|false|
|**esplifetime**|Lifetime of phase 2 VPN connection to the customer gateway, in seconds|long|255|false|
|**ikepolicy**|IKE policy of the customer gateway|string|255|true|
|**esppolicy**|ESP policy of the customer gateway|string|255|true|
|**dpd**|If DPD is enabled for VPN connection|boolean|255|false|
|**domainid**|The domain ID associated with the gateway. If used with the account parameter returns the gateway associated with the account for the specified domain.|uuid|255|false|
|**ikelifetime**|Lifetime of phase 1 VPN connection to the customer gateway, in seconds|long|255|false|
|**id**|ID of customer gateway|uuid|255|true|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**id**|The VPN gateway ID|string|
|**cidrlist**|Guest CIDR list of the customer gateway|string|
|**domain**|The domain name of the owner|string|
|**ipaddress**|Guest IP of the customer gateway|string|
|**name**|Name of the customer gateway|string|
|**gateway**|Public IP address ID of the customer gateway|string|
|**project**|The project name|string|
|**ikepolicy**|IKE policy of customer gateway|string|
|**dpd**|If DPD is enabled for customer gateway|boolean|
|**projectid**|The project ID|string|
|**esppolicy**|IPsec policy of customer gateway|string|
|**forceencap**|If Force NAT Encapsulation is enabled for customer gateway|boolean|
|**esplifetime**|Lifetime of ESP SA of customer gateway|long|
|**ipsecpsk**|IPsec preshared-key of customer gateway|string|
|**ikelifetime**|Lifetime of IKE SA of customer gateway|long|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**account**|The owner|string|
|**removed**|The date and time the host was removed|date|
|**domainid**|The domain ID of the owner|string|

## updateVpnGateway

This command updates site to site VPN local gateway.

<aside class='notice'>
Asynchronous
</aside>

### Command

> The following command updates site to site VPN local gateway:

```shell
$ cmk updateVpnGateway
```

`updateVpnGateway`

### Query parameters

|Parameter Name|Description|Type|Length|Required|
|--------------|-----------|----|------|--------|
|**id**|ID of customer gateway|uuid|255|true|
|**customid**|An optional field, in case you want to set a custom ID to the resource. Allowed to Root Admins only.|string|255|false|
|**fordisplay**|An optional field, whether to the display the VPN to the end user or not|boolean|255|false|

### Response schema

|Element|Description|Type|
|-------|-----------|----|
|**fordisplay**|Is VPN gateway for display to the regular user|boolean|
|**jobstatus**|The current status of the latest async job acting on this object|integer|
|**jobid**|The UUID of the latest async job acting on this object|string|
|**domainid**|The domain ID of the owner|string|
|**projectid**|The project ID|string|
|**id**|The vpn gateway ID|string|
|**vpcid**|The VPC ID of this gateway|string|
|**publicip**|The public IP address|string|
|**project**|The project name|string|
|**account**|The owner|string|
|**removed**|The date and time the host was removed|date|
|**domain**|The domain name of the owner|string|

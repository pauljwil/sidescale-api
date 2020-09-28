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

# AutoScale

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
|**listall**|If set to false, list only resources belonging to the command's caller; if set to true - list resources that the caller is authorized to see. Default value is false|boolean|255|false|
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

# Network

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
undefined
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

# Project

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
undefined
|**description**|The description of the security group|string|
|**egressrule**|The list of egress rules associated with the security group|set|
|**projectid**|The project id of the group|string|
|**domainid**|The domain ID of the security group|string|
|**id**|The ID of the security group|string|
|**tags**|The list of resource tags associated with the rule|set|
undefined
|**jobid**|The UUID of the latest async job acting on this object|string|
|**virtualmachinecount**|The number of virtualmachines associated with this securitygroup|integer|

# Snapshot

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

# SSH

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

# Virtual machine

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
undefined
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

# Volume

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
